# Trojan Horse - Implementation Plan

Spec: `docs/specs/Trojan Horse - Spec.md`
Recon: `docs/specs/and6-recon-results.md`
Generated: 2026-04-30

Repo target: `scarlot-safety-data` (new GitHub repo, registered as a git submodule of `scarlot-poc`).
Stack: Python 3.11+, Playwright, PostgreSQL 15, SQLAlchemy 2.0 async, Alembic, FastAPI, APScheduler, Typer, structlog, phonenumbers, Docker Compose.

Each step ends in a state where the project is working, tests pass, and a single commit lands. Test commands assume `make test` once scaffolding exists.

---

## Phase 0 - Recon completion

The big DOM recon already happened. Two tail-end recon items remain before adapter coding so we don't bake assumptions in.

- [ ] **0.1 - Capture the backing XHR.** Open `/my/escort/client-blacklist/escorts-list` with DevTools Network tab open *at navigation*, log all requests on initial load, page-2 click, and search. Save a sanitised HAR to `recon/and6-network.har` (in the new repo). Decide: DOM-walk vs. JSON-API. Document the decision in `recon/and6-decision.md`.
  - Test: HAR file exists, decision doc names the chosen path with one-paragraph rationale.
- [ ] **0.2 - Snapshot fixture HTML.** Save 3 representative `sc-escort-blacklist-item` outerHTML snippets (one with unmasked phone in body, one with name only, one with city missing) to `tests/fixtures/and6/`. These drive unit tests with no network.
  - Test: 3 `.html` files exist; each parses with `BeautifulSoup` without error.
- [ ] **0.3 - DPIA stub.** Write `docs/dpia-stub.md` listing the open legal/privacy questions: account custody, And6 ToS posture, retention, takedowns, transfer to API consumers, nFADP DPIA timing. Stub only - flags issues, does not resolve them.
  - Test: file exists, lists at least 6 distinct questions, each with a "decide by" milestone.

---

## Phase 1 - Repo scaffold

Goal: a runnable empty project with the same stack and idioms as `scarlot-market-data`, registered as a submodule.

- [ ] **1.1 - Create the repo.** `gh repo create scarlot-safety-data --private --clone`. Add MIT-style placeholder LICENSE, empty README placeholder.
  - Test: `gh repo view scarlot-safety-data` resolves; local clone exists.
- [ ] **1.2 - Pyproject + src layout.** `pyproject.toml` with hatchling build, deps mirrored from `scarlot-market-data` minus pgvector / pillow / imagehash (no image work in Phase 1). Add `httpx`, `playwright`, `playwright-stealth`, `apscheduler`, `phonenumbers`, `pydantic-settings`, `structlog`, `typer[all]`, `fastapi`, `uvicorn[standard]`, `sqlalchemy[asyncio]`, `asyncpg`, `alembic`. Dev deps: `pytest`, `pytest-asyncio`, `ruff`, `mypy`.
  - Test: `pip install -e .[dev]` succeeds in a fresh venv.
- [ ] **1.3 - Project skeleton.** Create `src/scarlot_safety/` with empty modules: `cli/`, `db.py`, `config.py`, `logging.py`, `phone.py`, `models/`, `scrapers/`, `api/`, `scheduler.py`. Add a `safety` console_script entry point pointing at `scarlot_safety.cli.app:app`.
  - Test: `safety --help` runs and shows the Typer banner.
- [ ] **1.4 - Docker Compose.** `docker-compose.yml` with three services: `db` (postgres:15), `api` (uvicorn), `scheduler` (the APScheduler process). Same env-file pattern as market-data. Add `.env.example`.
  - Test: `docker compose config` validates; `docker compose up db` boots Postgres and `psql` connects.
- [ ] **1.5 - Alembic init.** `alembic init -t async alembic`, point at `DATABASE_URL` from `config.py`, baseline empty migration.
  - Test: `safety db migrate` (Typer wrapper) runs `alembic upgrade head` against the dockerised db without error.
- [ ] **1.6 - Register as submodule.** From `scarlot-poc`: `git submodule add git@github.com:<owner>/scarlot-safety-data.git scarlot-safety-data`. Update `.gitmodules`. Update `CLAUDE.md` "Related Repositories" table.
  - Test: `git submodule status` shows the new submodule; `cd scarlot-safety-data && safety --help` works from the parent.

---

## Phase 2 - Core domain

Goal: phone normalization, models, migrations, category inference - all with unit tests, no network, no scrapers.

- [ ] **2.1 - `phone.py` normalization.** Wrap `phonenumbers.parse` with default region CH and a fallback list (DE, FR, IT, AT). Functions: `to_e164(raw, default_region="CH")`, `is_phone(s)`, `extract_phones(text) -> list[str]`. Returns `None` cleanly when unparseable.
  - Test: `pytest tests/test_phone.py` - cases for `+41 79 752 35 03`, `0791234567`, `+33 6 ...`, garbage strings, mixed-format inputs.
- [ ] **2.2 - Masked-phone reconciliation.** Function `reconcile_masked(masked: str, candidates: list[str]) -> str | None`. Match `+417975*3503` against `+41 79 752 35 03` by normalising both to digits, comparing length, prefix, suffix, and skipping the masked positions.
  - Test: `pytest tests/test_phone.py::test_masked_reconcile` - positive match, negative match, ambiguous (multiple candidates), no candidates.
- [ ] **2.3 - SQLAlchemy models.** `models/sources.py`, `models/scrape_runs.py`, `models/reports.py`, `models/phone_aliases.py`, `models/tenants.py`, `models/api_keys.py`, `models/api_request_log.py`. Async session in `db.py`. Indexes on `reports.phone_e164`, `reports.phone_masked`, `reports.source_id`, `reports.scraped_at`, `api_keys.key_hash`, `api_request_log.tenant_id`.
  - Test: model imports cleanly; `Base.metadata.tables` lists 7 tables.
- [ ] **2.4 - Alembic baseline migration.** Auto-generate from models, hand-edit for index correctness, apply.
  - Test: fresh `alembic upgrade head` against an empty db creates all 7 tables with expected indexes (`pytest tests/test_migrations.py` using a throwaway test db).
- [ ] **2.5 - Raw category inference.** `scrapers/inference.py` with multilingual keyword regexes per spec. Function `infer_raw_category(comment: str) -> tuple[RawCategory, severity | None]`. Falls through to `(unknown, None)`.
  - Test: `pytest tests/test_inference.py` - real comment samples lifted from recon hit expected raw categories; ambiguous comments fall through to `unknown`.
- [ ] **2.6 - Public-category mapping + status derivation.** `taxonomy.py` with `RAW_TO_PUBLIC` mapping, `derive_status(raw_categories: list[RawCategory]) -> Status`, `public_categories(raw_categories) -> list[PublicCategory]`. Status enum: `blacklist | greylist | clean | unknown`. Public enum: `time_waster | no_show | abusive | scammer | dangerous`.
  - Test: `pytest tests/test_taxonomy.py` - exhaustive coverage. `unknown` (no reports), `clean` (only `safe`), `greylist` (only `time_waster`/`no_show`), `blacklist` (any `abusive`/`scammer`/`dangerous`). Edge: `safe` + `aggressive` → `blacklist` (warnings dominate).
- [ ] **2.7 - Confidence scoring.** `scoring.py` implementing the formula in the spec (volume_factor, source_diversity, recency_factor, category_agreement). Documented in `docs/scoring.md`.
  - Test: `pytest tests/test_scoring.py` - boundary cases (1 report → low; 20 reports across 3 sources, recent, consistent → ≥ 0.85).
- [ ] **2.8 - Summary generator.** `summary.py` with `make_summary(reports) -> str` returning the templated one-liner per spec. Single short sentence, no PII beyond what was in source comments.
  - Test: `pytest tests/test_summary.py` - 0 reports (empty string, never returned to public API), 1 report, many reports same category, mixed.
- [ ] **2.9 - Dedup hashing.** `scrapers/dedup.py` with `comment_hash(text) -> str` (sha256 of normalized whitespace). Dedup key tuple: `(source_id, phone_masked, reported_at, comment_hash)`.
  - Test: `pytest tests/test_dedup.py` - same row → same key; whitespace variants → same hash; phone differs → different key.

---

## Phase 3 - And6 adapter

Goal: a working adapter that ingests the And6 client blacklist.

- [ ] **3.1 - `BaseScraper` + `runner`.** Lift the abstract pattern from `scarlot-market-data/scrapers/base.py`. `BaseScraper` defines `name`, `requires_auth: bool`, `auth_state_path | None`, and `async iter_records(page) -> AsyncIterator[ReportRecord]`. `runner.py` opens the browser, loads `storage_state` if present, dispatches to the adapter, persists records inside per-page transactions, writes `scrape_runs` row.
  - Test: `pytest tests/test_runner.py` with a stub adapter that yields 3 records - all 3 land in the db, run row records `status=ok`, `stats.records=3`.
- [ ] **3.2 - `safety auth and6` command.** Typer subcommand that launches Playwright in headed mode, navigates to the And6 login page, waits for the operator to log in (detects redirect to `/my/escort/...`), persists `storage_state` to the path in `config.AND6_AUTH_PATH`, exits.
  - Test: manual smoke - operator runs `safety auth and6`, logs in, file is written. (No automated test; documented in README.)
- [ ] **3.3 - `And6BlacklistScraper` row extraction.** Adapter that, given a Playwright page already on the blacklist URL, locates `sc-escort-blacklist-item`, extracts the masked phone, the date, the city, the body header (name OR unmasked phone), the comment body, the photo URLs (if "Afficher les photos" is clicked - skip in v1), and yields `ReportRecord` instances. Uses the selectors from the recon report verbatim.
  - Test: `pytest tests/test_and6_extract.py` - parses each of the 3 fixture HTML files into expected `ReportRecord` shapes.
- [ ] **3.4 - And6 pagination.** Loop logic: detect last page via `i.sc-arrow-next` parent class containing `disabled`. Click `span.page-item.actions:has(i.sc-arrow-next)`, wait for DOM settle (`page.wait_for_load_state("networkidle")` then re-locate rows), increment page counter. Synthesize `source_url = f"{URL}#page={n}&offset={i}"` per row. If 0.1 found a JSON API, **skip DOM pagination** and call the API directly.
  - Test: `pytest tests/test_and6_pagination.py` with a mocked Playwright page exposing 3 paginated views - all 3 pages traversed, terminates on `disabled`, no infinite loop.
- [ ] **3.5 - Auth-expiry handling.** Detect mid-run redirect to a login page. Mark `scrape_runs.status = auth_expired`, log at ERROR with run-id, exit cleanly.
  - Test: `pytest tests/test_and6_auth.py` with a mocked navigation that redirects to `/login` after page 2.
- [ ] **3.6 - Smoke run.** `safety scrape and6 --max-pages 1` against live And6 with valid auth state. Should write 1 `scrape_runs` row and ~20 `reports` rows. Manually inspect output for sanity.
  - Test: manual; capture stdout to `recon/and6-first-run.log` (gitignored if it leaks PII; sanitised excerpt committed).
- [ ] **3.7 - Full run.** Drop the `--max-pages` cap; let it walk all 518 pages with jittered delays (2-5s between page clicks). Expect ~10,000 rows ingested. Verify metrics: zero parse errors, expected dedup ratio.
  - Test: query `select count(*) from reports where source = 'and6'` ≥ 9000 (allow some unparseable rows).

---

## Phase 4 - Exploratory data analysis + taxonomy refinement

Goal: confirm the v1 schema and category model against real And6 data before locking the public API on top. Re-running inference does not require re-scraping (we store `comment` verbatim and `raw_payload_jsonb`). This phase produces a notebook, a findings doc, and zero or more deltas to the spec, models, taxonomy, and inference rules.

- [ ] **4.1 - EDA notebook scaffold.** `analysis/and6-eda.ipynb` connecting to the dev db (read-only role). Standard imports: pandas, sqlalchemy, matplotlib, `langdetect` (or `lingua`), `phonenumbers`. Cell 1 loads `reports where source = 'and6'` into a dataframe. Smoke test runs end to end.
  - Test: `jupyter nbconvert --to notebook --execute` succeeds against a seeded test db.
- [ ] **4.2 - Volume + coverage panel.** Total reports, total unique phones, % masked-only vs reconcilable to E.164, reports-per-phone histogram (head/long-tail shape), city top-30, time-series of `reported_at` by month.
  - Output: 5-6 plots + a small numeric summary table embedded in the notebook.
- [ ] **4.3 - Language + comment-shape panel.** Detected language mix per row (de/fr/en/it/other). Comment-length histogram by language. Top bigrams/trigrams per language (lowercased, stopwords removed). Quick eyeball of whether language detection is reliable on short comments.
  - Output: language pie, length histogram facets, n-gram tables.
- [ ] **4.4 - Category hit-rate audit.** For each raw category in the v1 taxonomy: % of rows hit, sample of 20 hits and 20 non-hits per category for spot-check. Single big table `unknown` bucket: top 100 most-frequent terms in `unknown` rows that do not match any keyword. Export `analysis/and6-unknown-sample.csv` (200 sanitised rows) for human review.
  - Output: hit-rate table, unknown-bucket term frequency, csv export.
- [ ] **4.5 - Severity sanity sample.** For each public-category bucket, draw 20 random rows. Human (you) eyeballs the sample and writes a short paragraph: does the bucket look right? Are there sub-themes worth promoting to first-class categories? Are there miscategorisations the regexes are causing?
  - Output: paragraph per bucket appended to the findings doc.
- [ ] **4.6 - Co-occurrence + dedup-quality check.** Reports-per-phone distribution. Phones with multiple raw categories (do they look like the same person across multiple incidents, or noise?). Same-comment-text-across-different-phones (likely copy-paste templates worth normalising). Same-phone-with-near-duplicate-comments (dedup escapes).
  - Output: histogram, two example tables.
- [ ] **4.7 - Findings doc.** `analysis/and6-findings.md` consolidating the panels into a 1-2 page writeup with explicit deltas:
  - Categories to add, drop, or rename
  - Severity reassignments
  - Mapping changes (raw → public)
  - Inference-rule additions (new keywords, new regexes, languages currently underserved)
  - Schema changes (e.g. promote a derived column, add an index, normalize handle-vs-phone in `name_or_handle`)
  - Cases where the API contract may need to flex (e.g. a 6th public category)
  - Each delta is a concrete checkbox; deltas with low confidence are flagged for human decision.
- [ ] **4.8 - Apply spec deltas.** Update `Trojan Horse - Spec.md` with the resolved deltas (taxonomy enums, mapping table, severity tiers, scoring weights). Mark this revision in the spec with a "v1.1 (post-EDA)" note. If a contract-shape change is unavoidable, flag it in writing before merging - the contract is in `Trojan Horse - Shape expected.md` and changing it has downstream consequences.
  - Test: spec compiles (markdown lints clean); diff is small and traceable.
- [ ] **4.9 - Apply taxonomy + inference deltas.** Update `taxonomy.py` and `scrapers/inference.py` per the deltas. Bump tests in `tests/test_taxonomy.py` and `tests/test_inference.py` to cover new categories / mappings.
  - Test: full test suite green.
- [ ] **4.10 - Re-infer in place.** Add `safety reinfer <source>` Typer command that walks the existing `reports` rows for that source, recomputes `category_raw` and `severity` from `comment`, and updates rows in batches inside transactions. Idempotent. Logs a before/after histogram.
  - Test: `pytest tests/test_reinfer.py` against a seeded fixture set; histogram diff matches expected; second run is a no-op.
- [ ] **4.11 - Notebook re-run + signoff.** Re-execute the EDA notebook against the re-inferred data. Confirm `unknown` bucket shrunk, category proportions look defensible, no regressions. Commit the executed notebook (with outputs).
  - Test: `unknown` bucket share strictly lower than v1 baseline; sample-by-bucket eyeball still passes.

---

## Phase 5 - Multi-tenant API + scheduler

Goal: contract-shaped reverse-lookup endpoint live behind tenant-scoped keys; scheduled scraping.

- [ ] **5.1 - Tenant + key management.** `auth.py` with argon2id key hashing (one-time-shown plaintext key, never stored). Typer commands: `safety tenants create <name> --contact <email>`, `safety tenants list`, `safety keys issue <tenant_id> --label <name>`, `safety keys revoke <key_id>`. Plaintext key returned only on `issue`.
  - Test: `pytest tests/test_auth.py` - issue → verify with original plaintext succeeds; verify with wrong plaintext fails; revoke → verification fails; argon2id parameters within sane bounds.
- [ ] **5.2 - FastAPI app + middleware.** `api/app.py` with `GET /healthz`, `GET /metrics` (prometheus_client format), `POST /v1/phones/lookup`. Bearer-auth middleware: extract `Bearer <key>`, look up by hash, attach `tenant_id` and `key_id` to request state, write `api_request_log`. 401 on missing/bad/revoked key. Pydantic request model: `{"phone_e164": "+E164..."}` with strict regex validation; 422 on bad shape.
  - Test: `pytest tests/test_api_auth.py` - valid key 200, missing header 401, malformed header 401, revoked key 401, malformed body 422, log row written with correct tenant.
- [ ] **5.3 - Lookup service.** `lookup.py` with `async lookup_phone(phone_e164: str) -> LookupResponse`. Pulls all `reports` for that exact `phone_e164`, derives status via `taxonomy.derive_status`, maps to public categories, computes `confidence` via `scoring`, generates `summary` via `summary`, returns the response model. Empty result → `status=unknown`, `categories=[]`, `report_count=0`, `confidence=0.0`, `summary=""`, both timestamps null.
  - Test: `pytest tests/test_lookup_service.py` with seeded fixtures - exact match (greylist), exact match (blacklist), exact match all-safe (clean), no match (unknown), masked-only match returned in admin endpoint but **not** the public one.
- [ ] **5.4 - Internal admin endpoint.** `GET /_admin/phones/{phone_e164}/reports` returns the raw underlying records (full comment, source url, raw_payload). Gated by a separate admin-token env var. Documented as not-part-of-public-contract.
  - Test: `pytest tests/test_admin_api.py` - admin token 200, public tenant key 403, no token 401.
- [ ] **5.5 - Response contract conformance.** Snapshot test asserting the response shape matches `Trojan Horse - Shape expected.md` exactly: keys, types, enums, value bounds.
  - Test: `pytest tests/test_contract.py` runs the response through a JSON Schema lifted from the shape doc.
- [ ] **5.6 - APScheduler integration.** `scheduler.py` runs `safety scrape and6` on a cron (default `0 4 * * *` with 30-min jitter). Run-id propagates through structlog context. SIGTERM-clean.
  - Test: `pytest tests/test_scheduler.py` - patched cron, verify the job triggers and run-id appears in captured logs.
- [ ] **5.7 - CLI completeness.** `safety scrape <source>`, `safety lookup <phone>` (CLI accepts loose input, normalizes, then queries), `safety run-api`, `safety run-scheduler`, `safety auth <source>`, `safety db migrate`, `safety tenants ...`, `safety keys ...`, `safety reinfer <source>`. All show help; all exit 0 on happy path.
  - Test: `pytest tests/test_cli.py` invoking each via Typer's `CliRunner`.

---

## Phase 6 - Ops + docs

- [ ] **6.1 - README.** Setup, env vars, `make` targets, how to run (`docker compose up`), how to acquire And6 auth state, how to add a new adapter, the privacy posture, link back to the spec.
  - Test: `markdownlint README.md` clean.
- [ ] **6.2 - Adapter-author guide.** `docs/adapters.md` walking through writing a new adapter (extend `BaseScraper`, implement `iter_records`, register in `runner.py`, add fixture tests).
  - Test: a second engineer (or a fresh session) can read the doc and produce a stub adapter without further questions.
- [ ] **6.3 - Auth-state runbook.** `docs/auth-runbook.md` - one-page steps for refreshing `storage_state` when the And6 session expires.
  - Test: doc exists; review by co-founder.
- [ ] **6.4 - CLAUDE.md for new repo.** Project-level conventions, build/test commands, link to spec.
  - Test: file exists in the new repo root.
- [ ] **6.5 - Update parent CLAUDE.md.** Add the new submodule and its purpose to `scarlot-poc/CLAUDE.md`'s "Related Repositories" table.
  - Test: parent CLAUDE.md mentions `scarlot-safety-data`.

---

## Phase 7 - Validate against spec

- [ ] **7.1 - Acceptance walk.** Re-run every checkbox in the spec's "Acceptance Criteria" section; confirm or fix.
- [ ] **7.2 - Full test suite + lint + typecheck.** `make test && make lint && make type` all green.
- [ ] **7.3 - Boundary review.** Read the spec's "Constraints" and "Out of Scope" sections; grep the codebase for any violation (e.g. references to `pgvector`, `comments/`, scraping outside `/my/escort/client-blacklist/`).
- [ ] **7.4 - Open backlog issue.** GitHub issue listing the next-phase candidates from the spec's Open Questions: admin-alerts tab, "Ajouter Client" form schema, `/comments/*` public surfaces (with policy decision), CheckClient.ch, Telegram safety groups.

---

## Notes

- **Commit cadence**: one commit per checked step. Commit message refers to the step number, e.g. `[2.3] add SQLAlchemy models for sources/runs/reports/phone_aliases`. No mention of Claude in any commit.
- **Privacy gate**: no real phone numbers, comments, or operator credentials in committed fixtures. The 3 fixture HTML files in 0.2 must be sanitised (replace digits in masked phones with `X`s, replace unmasked phones with placeholder, keep the structural markup intact).
- **Naming**: spell out full names in docs/filenames/commits (per project convention; no acronyms).
- **No em dashes** in any committed prose (per project convention).

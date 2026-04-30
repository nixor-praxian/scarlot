# Trojan Horse - Implementation Plan

Spec: `docs/specs/Trojan Horse - Spec.md`
Recon: `docs/specs/and6-recon-results.md`
Generated: 2026-04-30

Repo target: `scarlot-safety-data` (new GitHub repo, registered as a git submodule of `scarlot-poc`).
Stack: Python 3.11+, Playwright, PostgreSQL 15, SQLAlchemy 2.0 async, Alembic, FastAPI, APScheduler, Typer, structlog, phonenumbers, Docker Compose.

Each step ends in a state where the project is working, tests pass, and a single commit lands. Test commands assume `make test` once scaffolding exists.

---

## Phase 0 - Recon completion

- [x] **0.1 - Capture the backing API.** DONE. Investigation of the Angular bundle (chunk `487.b6ddd1991dc40268.js`) revealed the SPA talks GraphQL to `https://api.and6.com/graphql`. Direct `POST` with the operator's storage_state cookies + `Authorization: Bearer <usertoken>` returns the full structured blacklist. Decision: **GraphQL-API-direct**. See `scarlot-safety-data/recon/and6-decision.md` for the full writeup including operations, fragments, auth model, and risks.
- [x] **0.2 - Snapshot fixtures.** DONE. Three representative records saved at `scarlot-safety-data/tests/fixtures/and6/blacklisted_clients_page1.json` (sanitised). Raw 20-record sample at `scarlot-safety-data/recon/and6-blacklist-sample.raw.json` (gitignored).
- [x] **0.3 - DPIA stub.** DONE. `scarlot-safety-data/docs/dpia-stub.md` lists 10 open privacy/legal questions, each with a decide-by milestone.

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
- [ ] **2.2 - Masked-phone helpers.** And6's GraphQL response provides both forms as structured fields, so the reconciliation regex spec'd in v1 is **no longer needed**. Keep `mask_e164(e164: str) -> str` (renders `+41797523503` as `+417975*3503`) and `is_masked(s: str) -> bool` for symmetry and future sources that only emit one form.
  - Test: `pytest tests/test_phone.py::test_masking` - round-trip, edge cases.
- [ ] **2.3 - SQLAlchemy models.** `models/sources.py`, `models/scrape_runs.py`, `models/reports.py`, `models/phone_aliases.py`, `models/tenants.py`, `models/api_keys.py`, `models/api_request_log.py`. `reports` carries `source_record_id` plus the per-phone columns from the spec (one row per source-record × phone). Async session in `db.py`. Indexes on `reports.phone_e164`, `reports.phone_masked`, `reports.source_id`, `reports.source_record_id`, `reports.scraped_at`, `api_keys.key_hash`, `api_request_log.tenant_id`. Unique constraint `(source_id, source_record_id, COALESCE(phone_e164, phone_masked))`.
  - Test: model imports cleanly; `Base.metadata.tables` lists 7 tables; the unique constraint exists.
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
- [ ] **2.9 - Upsert helper.** `scrapers/upsert.py` exposing `upsert_report(session, fields)` that runs `INSERT ... ON CONFLICT (source_id, source_record_id, COALESCE(phone_e164, phone_masked)) DO UPDATE` to set scraped_at and append to seen_at. Idempotent.
  - Test: `pytest tests/test_upsert.py` - first call inserts, second call updates `seen_at`; payload changes are reflected.

---

## Phase 3 - And6 adapter

Goal: a working adapter that ingests the And6 client blacklist via GraphQL.

The adapter is materially smaller than the v1 plan anticipated because Phase 0 confirmed a JSON-API path. The GraphQL operation library lives in `scarlot_safety.scrapers.and6_graphql` (already committed in Phase 0); this phase wires it to the `BaseScraper` contract and persists results.

- [ ] **3.1 - `BaseScraper` + `runner`.** Lift the abstract pattern from `scarlot-market-data/scrapers/base.py` and adapt for non-Playwright sources. `BaseScraper` defines `name`, `requires_auth: bool`, `auth_state_path: Path | None`, and `async iter_records() -> AsyncIterator[ReportRecord]`. `runner.py` resolves the adapter, dispatches `iter_records()`, persists each record via the upsert helper from 2.9 (per-record transactions or batched), writes a `scrape_runs` row with start/finish/status/stats. No Playwright in the runner; adapters that need it (e.g. `safety auth and6`) own their own browser lifecycle.
  - Test: `pytest tests/test_runner.py` with a stub adapter that yields 3 records - all 3 land in the db, run row records `status=ok`, `stats.records=3`.
- [ ] **3.2 - `safety auth and6` command.** Typer subcommand that launches Playwright in headed mode, navigates to `https://www.and6.com/my/escort/client-blacklist/escorts-list` (auto-redirects to login if not authenticated), waits for the operator to log in (detects URL containing `/my/`), persists `storage_state` to `config.and6_auth_state_path`, exits. Promote the recon script `recon/and6_browser_session.py` into the CLI verb (drop the HAR/DOM-dump side-effects which were only needed during Phase 0).
  - Test: manual smoke - operator runs `safety auth and6`, logs in, file is written. README documents the runbook.
- [ ] **3.3 - `And6BlacklistScraper`.** Adapter built on top of `scarlot_safety.scrapers.and6_graphql.iter_blacklisted_clients`. Loads `And6Auth.from_storage_state(config.and6_auth_state_path)`, iterates the GraphQL pages, transforms each record into one or more `ReportRecord`s (one per phone number in the source `phone_numbers[]` array, sharing `source_record_id`). Captures `geo_node.id` and `geo_node.name`, the first `emails[]` entry if present, and stores the full source record verbatim in `raw_payload`.
  - Test: `pytest tests/test_and6_extract.py` against `tests/fixtures/and6/blacklisted_clients_page1.json` (3 records, including: one with single phone + no email + with city; one with `name` field that is itself a phone string; one with email + image_ids + null geo_node + masked-only phone). Adapter produces the expected number of `ReportRecord`s with the expected field values.
- [ ] **3.4 - GraphQL pagination + jitter.** The adapter pages via `limit=100, offset=0..total_count` (configurable). Sleep `random.uniform(0.5, 1.5)` between requests. Stop when `offset >= total_count` from the first page. No DOM, no Playwright in the hot path.
  - Test: `pytest tests/test_and6_pagination.py` with `respx`-mocked GraphQL responses returning 3 pages totalling 250 records - exact count emitted, terminates correctly.
- [ ] **3.5 - Auth-expiry detection.** Inspect GraphQL errors for `Permission "blacklisted.client.approved.read" is required` or HTTP 401/403. Mark `scrape_runs.status = auth_expired`, log at ERROR with run-id, exit cleanly. Surface a clear message in the CLI.
  - Test: `pytest tests/test_and6_auth.py` with `respx`-mocked permission error - run row written with `status=auth_expired`, no records persisted, exit code non-zero.
- [ ] **3.6 - Smoke run.** `safety scrape and6 --max-records 20` against live And6 with valid auth state. Should write 1 `scrape_runs` row and 20-25 `reports` rows (≥ 20 because some records have multiple phones). Manually inspect for sanity.
  - Test: manual; capture stdout to `recon/and6-first-run.log` (gitignored - may contain real PII; sanitised excerpt committed if useful).
- [ ] **3.7 - Full run.** Drop the `--max-records` cap. Expect ~104 GraphQL requests, ~3 minutes total wall time, ~10,000-11,000 `reports` rows ingested.
  - Test: `select count(*) from reports where source = 'and6'` ≥ 10000; `select count(distinct source_record_id) from reports where source = 'and6'` within 5% of `total_count` returned by the API.

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

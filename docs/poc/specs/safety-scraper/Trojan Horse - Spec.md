# Trojan Horse - Safety Lookup Service Spec

Target repo: `scarlot-safety-data` (new, registered as a git submodule of `scarlot-poc`)
Stack: mirrors `scarlot-market-data` (Python 3.11+, Playwright, PostgreSQL, SQLAlchemy 2.0 async, Alembic, FastAPI, APScheduler, structlog, phonenumbers, Typer, Docker Compose)

## Purpose

Build the safety-data backbone that turns Scarlot from a productivity tool into a wedge product. Today, blacklist and bad-client information is scattered across platforms, private groups, and per-worker spreadsheets. CheckClient.ch is the only structured Swiss source and it is small, walled, and tied to one platform. The Trojan Horse service aggregates and normalizes this information across sources so that any Scarlot client (the WhatsApp bot, an internal review UI, a future API customer) can perform a reverse lookup by phone number and receive a consolidated view of what is known about a number.

This is not a worker-profile pipeline. `scarlot-market-data` already handles that. This service is keyed on the **client** (caller) phone number, not the worker.

## Requirements

### Functional

1. **Source adapters** - pluggable scraper interface so new sources can be added without changing core code. Phase 1 ships exactly one adapter: `and6`.
2. **Scrape scheduling** - APScheduler running each adapter on a configurable cron (default: 1x daily, with jitter). Each run produces an append-only snapshot.
3. **Phone-keyed normalization** - every scraped record is reduced to:
   - `source_record_id` (stable identifier from the source, e.g. And6's `_id`; required for upsert/dedup)
   - `phone_e164` (E.164-normalized via `phonenumbers`; populated when the source provides an unmasked international number, OR when normalization of a masked-only form is unambiguous; otherwise null)
   - `phone_masked` (verbatim masked form when the source emits one, e.g. And6's `+417975*3503`)
   - `source` (adapter name)
   - `source_url` (best-effort permalink; null when the source has no per-record permalink)
   - `category` (normalized enum, see Schema below; populated by post-scrape inference for free-text sources)
   - `severity` (0-5; null when category is `unknown`)
   - `comment` (raw text)
   - `city` (best-effort, nullable; for And6, derived from `geo_node.name`)
   - `geo_node_id` (stable region/city ID from the source when available, nullable)
   - `name_or_handle` (free-text name/handle that appears in the record, nullable; **not** a reporter handle)
   - `email` (when the source carries email contact info, nullable)
   - `reported_at` (date/time on the source, nullable; ISO-aware parsing)
   - `scraped_at` (timestamp, server-side)
   - `raw_payload` (JSONB, full source-specific record for debugging and re-inference)

   When a source emits multiple phones for one record (And6's `phone_numbers[]`), one `reports` row is written per phone, all sharing the same `source_record_id`. Dedup uses `(source_id, source_record_id, phone_e164 OR phone_masked)`.
4. **Identity resolution** - records sharing `phone_e164` cluster on lookup. No silent merging in storage; every raw report is preserved. The public API returns only the consolidated verdict (status, categories, counts, summary, confidence). Raw records are reachable via the internal admin endpoint.
5. **Reverse lookup HTTP API** - FastAPI. Public contract is one endpoint:

   ```
   POST /v1/phones/lookup
   Authorization: Bearer <tenant-scoped api key>

   Request:
   {
     "phone_e164": "+41791234567"   // strict E.164, validated server-side
   }

   Response (status always 200; clean and unknown are first-class):
   {
     "phone_e164": "+41791234567",
     "status": "blacklist" | "greylist" | "clean" | "unknown",
     "categories": ["time_waster", "no_show", "abusive", "scammer", "dangerous"],
     "report_count": 12,
     "first_reported_at": "2024-09-01T12:34:00Z",
     "last_reported_at": "2025-12-15T08:11:00Z",
     "summary": "Multiple operators report ghosting after extensive negotiation.",
     "confidence": 0.85
   }
   ```

   Auxiliary endpoints: `GET /healthz`, `GET /metrics`. Internal-only admin endpoint `GET /_admin/phones/{phone_e164}/reports` returns the raw underlying records for moderation/debugging; not part of the public contract; gated by a separate admin token.

   - **Strict E.164 in request.** Caller is responsible for normalization. Server returns 422 on malformed input. The reverse-lookup parsing helpers are exposed in the CLI (`safety lookup +41 79 ...`) for human use, not the HTTP API.
   - **Tenant-scoped auth.** API keys are stored hashed (argon2id), one or more keys per tenant. Each request is logged with `tenant_id` and `key_id`. Token rotation = issue new key, revoke old.
6. **Deduplication** - within a single source, identical `(phone_e164, source_url, comment_hash)` collapses to one record across snapshots; we still preserve a `seen_at[]` array of run timestamps for change tracking.
7. **CLI** - Typer commands: `safety scrape <source>`, `safety lookup <phone>`, `safety run-api`, `safety run-scheduler`, plus `safety db migrate`.
8. **Logging** - structlog JSON logs in production, colored console in dev. Every scrape run gets a run-id propagated through child logs.
9. **Phase 1 source: and6** - scraper targets the And6 backend at `POST https://api.and6.com/graphql`, calling the `blacklistedClients(query, limit, offset)` GraphQL operation lifted from the Angular bundle. Recon confirms 10,355 reports total at recon time. Each record exposes: stable `_id`, structured `phone_numbers[]` with both `international_number` and `masked_international_number` per phone, ISO-formatted `date`, `geo_node{id,name}`, `name` (sometimes a real name, sometimes a phone string), `emails[]`, free-text behavioural `comment` in mixed languages (de/en/fr/it), and `image_ids[]` references. Reporter identity is not exposed. Auth: cookies + `Authorization: Bearer <usertoken>` header (where `usertoken` is the JWT cookie persisted from the operator's login). The "Alerte de l'admin" sibling surface uses the `blacklistedEntries` query and the `/comments/*` public surfaces are out of scope for Phase 1. See `docs/poc/specs/and6/and6-recon-results.md` and `scarlot-safety-data/recon/and6-decision.md` for the full reverse-engineering writeup and operation library.

### Non-functional

- **Privacy by design** - the database holds plaintext phone numbers because the service must do reverse lookup. We mitigate by: (a) Swiss hosting, (b) bearer-token API only, (c) no PII other than phone numbers and free-text comments, (d) no joining with `scarlot-market-data` worker tables in the same DB.
- **Idempotent scrapes** - re-running a scrape never duplicates records.
- **Resilient to source HTML changes** - one broken adapter must not break others. Errors are logged with run-id, not fatal.
- **Bot-detection mitigation** - reuse the playwright-stealth + UA rotation + jitter pattern from `scarlot-market-data`. Run with translator extensions disabled (And6 explicitly warns against them).
- **Authenticated session handling** - And6 is auth-walled. The scraper loads an Angular SPA after navigation, so Playwright is mandatory; `httpx`/`requests` cannot reach the data. We persist a Playwright `storage_state` produced from a manual one-time login; the runbook for refreshing it lives in the repo. The auth-state file is treated as a secret (env-pointer to filesystem path; never committed).
- **Selector stability** - Angular hashes (`_ngcontent-*`) change per build. Adapters must select on stable class names and tag names, never on hashed attributes.
- **Robots policy** - `/my/*` is allowed (auth-walled). Disallowed paths from And6's `robots.txt` (`/comments/*`, `/member`, `/search`, etc.) are off-limits even if they look juicy. Phase 1 stays inside `/my/escort/client-blacklist/`.
- **Test coverage** - phone normalization, masked-phone reconciliation, category inference, dedup logic, and the lookup endpoint must have unit tests. Adapters get fixture-based tests using saved HTML snapshots from recon.

## Constraints

- **Stack parity** with `scarlot-market-data` so the team carries one mental model. Reuse `phone.py`-equivalent normalization and the `BaseScraper`/`runner.py` adapter pattern.
- **No PII beyond phone + free-text comment.** No client names, addresses, or photos in the schema.
- **No write API** in Phase 1. All data ingress is via scrapers. Manual entry comes later (and will require a moderation layer).
- **Submodule, not vendored.** `scarlot-safety-data` lives in its own GitHub repo; `scarlot-poc` references it via `.gitmodules`.
- **Swiss legal posture** - documented DPIA stub at `docs/dpia-stub.md` flags the open questions for legal counsel before any source goes beyond recon. Phase 1 proceeds with this acknowledged risk; production launch does not.
- **No mention of Claude in commits.**
- **Naming: spell out full names in docs/filenames/commits** (per project convention).

## Edge Cases

- **Phone is only available masked.** And6's GraphQL response gives both `international_number` and `masked_international_number` per phone, but `international_number` is sometimes null (record predates a schema migration, reporter only entered the masked form, etc.). When `international_number` is present, store as `phone_e164`. When only the masked form is present, store `phone_masked` and leave `phone_e164` null. A secondary "masked search" endpoint can return masked-only matches as low-confidence hits.
- **Phone number cannot be parsed** - record retained with `phone_e164=null`, logged at WARN level. Counter exposed in metrics.
- **Phone number from another country** (DE, FR, IT, AT) - accepted; `phone_e164` retains country code. Lookup must handle local-format input by trying CH first, then a configurable fallback list.
- **Source carries multiple phone numbers per record** (And6's `phone_numbers[]` is an array) - one `reports` row per phone, all sharing the same `source_record_id`. Comment text duplicated across rows.
- **Stable per-record ID exists** (And6 emits `_id`) - dedup key is `(source_id, source_record_id, phone_e164 OR phone_masked)`. No content-hash needed.
- **No real permalink per record** (And6 GraphQL response has no public URL per record) - `source_url` is null. The And6 web UI also does not produce per-record permalinks. Document this in the admin endpoint output.
- **Source schema changes** - GraphQL queries are pinned in `scarlot_safety.scrapers.and6_graphql`. A schema drift produces a `RuntimeError: graphql errors` from the adapter, which marks the run failed; no partial commit (per-page transactions). Ops fixes the query and re-runs.
- **Auth state expires mid-run** - the GraphQL endpoint returns a permission error (`Permission "blacklisted.client.approved.read" is required`) on a stale or lower-privilege token. The adapter inspects errors, marks the run `auth_expired`, exits cleanly; metric exposed; ops refreshes via `safety auth and6` and re-runs.
- **Bearer token leak** - tokens are versioned via env; rotating means redeploying the API. Acceptable for Phase 1.
- **Lookup hits zero records** - return 200 with empty `reports: []`, not 404. Distinguishes "we have no data" from "wrong endpoint."
- **Same phone, contradictory reports across sources** - the API does not adjudicate. Consolidated view returns max severity and the full list; the caller decides.

## Out of Scope (Phase 1)

- Writes from end users (no public submission API).
- Telegram or any private-group ingestion.
- CheckClient.ch, Bemygirl, Fgirl ingestion.
- Projet Jasmine ingestion (cross-border legal complexity).
- Image-based identity matching (no `pgvector` in this DB - that is a market-data concern).
- Worker-side identity resolution (the consumer of the lookup may be a worker, but we do not authenticate workers in Phase 1).
- Front-end UI for review/moderation. Postponed to Phase 2.
- Federation with `scarlot-market-data` (no shared DB, no cross-table joins).

## Acceptance Criteria

- [ ] `scarlot-safety-data` repo exists on GitHub and is registered as a submodule of `scarlot-poc`.
- [ ] `docker compose up` starts: Postgres, the API, the scheduler.
- [ ] `safety db migrate` applies an Alembic baseline migration creating `sources`, `reports`, `phone_aliases`, `scrape_runs` tables.
- [ ] `safety auth and6` opens a Playwright browser, lets the operator log in manually, persists `storage_state` to the configured path, and exits.
- [ ] `safety scrape and6` runs against live And6 using the persisted auth state, calls the `blacklistedClients` GraphQL operation paginated by `limit`/`offset`, writes one `scrape_runs` row, and upserts report rows on `(source_id, source_record_id, phone)`. A first end-to-end run captures at least the first 20 records before tested in full (~10,355 records, ~104 GraphQL requests).
- [ ] `safety lookup +41 79 752 35 03` (CLI) prints the consolidated lookup result and the underlying raw reports.
- [ ] `POST /v1/phones/lookup` with a valid tenant API key returns the contract shape: `phone_e164`, `status` ∈ {`blacklist`, `greylist`, `clean`, `unknown`}, `categories[]` from the 5-value public enum, `report_count`, `first_reported_at`, `last_reported_at`, `summary` (one sentence), `confidence` (0..1). Bad/missing/revoked token returns 401. Malformed `phone_e164` returns 422.
- [ ] Status derivation, public-category mapping, and confidence formula are unit-tested against fixture report sets covering every status branch.
- [ ] `tenants` and `api_keys` tables exist; `safety tenants create <name>` and `safety keys issue <tenant>` Typer commands work; key hash is argon2id; raw key is shown once at creation and never stored.
- [ ] `api_request_log` records each lookup with `tenant_id`, `key_id`, requested phone, status, latency.
- [ ] Unit tests pass: phone normalization (incl. masked reconciliation), category inference, dedup logic, lookup endpoint. Adapter tests pass against committed HTML fixtures lifted from the recon report.
- [ ] Scheduler triggers a scrape on its cron. Run-id propagates through logs.
- [ ] README documents: setup, env vars, how to run, how to add a new adapter, the auth-state runbook, and the privacy posture.
- [ ] DPIA stub committed at `docs/dpia-stub.md` listing the legal/privacy questions still open (auth-account custody, scraped-data retention, takedown requests, And6 ToS posture).
- [ ] EDA pass after first full And6 ingest: `analysis/and6-eda.ipynb` runs end to end and `analysis/and6-findings.md` documents the proposed taxonomy/schema/inference deltas with concrete checkboxes. The deltas have been resolved and applied (spec, taxonomy, inference) before any external API consumer is wired in.
- [ ] `safety reinfer <source>` re-runs inference over already-stored comments without re-scraping; documented as the iteration loop for taxonomy work.

## Schema (sketch)

```
sources(id, name, base_url, enabled, created_at)

scrape_runs(id, source_id, started_at, finished_at, status, stats_jsonb, error)

reports(
  id, source_id, run_id,
  source_record_id,            -- stable id from the source (e.g. And6 _id)
  phone_e164, phone_masked, phone_raw,
  category_raw,                -- internal 10-value enum
  severity,                    -- 0-5 nullable
  comment, city, geo_node_id, name_or_handle, email,
  reported_at, source_url, raw_payload_jsonb,
  scraped_at, seen_at_array,
  UNIQUE (source_id, source_record_id, COALESCE(phone_e164, phone_masked))
)

phone_aliases(phone_e164, alt_format, source_id)  -- for cross-format lookup acceleration

tenants(id, name, contact, created_at, disabled_at)
api_keys(id, tenant_id, key_hash, label, created_at, last_used_at, revoked_at)
api_request_log(id, tenant_id, key_id, phone_e164, status, latency_ms, requested_at)
```

**Internal raw-category enum** (what we extract from sources, kept for debugging and future mapping work):

`safe`, `time_waster`, `no_show`, `price_negotiator`, `non_payment`, `drugs`, `health_risk`, `aggressive`, `dangerous`, `unknown`.

**Public-API category enum** (what `POST /v1/phones/lookup` returns) is reduced to 5 values to match the contract:

| Public category | Internal raw categories that feed it |
|---|---|
| `time_waster` | `time_waster`, `price_negotiator` |
| `no_show` | `no_show` |
| `abusive` | `aggressive` |
| `scammer` | `non_payment`, `drugs` (deceptive intent) |
| `dangerous` | `dangerous`, `health_risk` |

`safe` is not a public category; one or more `safe` reports with no warning reports drives `status: clean`. Mapping lives in `scarlot_safety.taxonomy` and is unit-tested.

**Status derivation rules** (`status` field of the API response):

- `unknown` if `report_count == 0`.
- `clean` if `report_count > 0` AND all underlying raw categories are `safe`.
- `blacklist` if any public category in `{abusive, scammer, dangerous}` appears.
- `greylist` if `report_count > 0` AND only `time_waster` and/or `no_show` and/or `unknown` appear (no `safe`-only path; that is `clean`).

**Confidence scoring** (`confidence` field, 0..1). Phase 1 formula:

```
confidence = clamp(0, 1,
   0.40 * volume_factor       # tanh(report_count / 5)
 + 0.30 * source_diversity    # min(1, distinct_sources / 3)
 + 0.20 * recency_factor      # 1.0 within 90 days, decays to 0.0 at 2 years
 + 0.10 * category_agreement  # share of reports voting for the dominant public category
)
```

Documented in `docs/scoring.md` so callers can interpret it. Tunable via env once we have real volume data.

**Summary field**. Phase 1 generates a templated one-liner:
`"N reports across M source(s); most common: <public_category>. Most recent: <date>."`
Phase 2 replaces this with a small-LLM compression of the underlying comments. The contract guarantees a single short sentence regardless of generator.

**Free-text inference for And6 (Phase 1)**: keyword-regex inference (multilingual seed list: `no.show|no-show`, `timewaster|time.waster|perdeur de temps`, `aggressive|aggressiv|agressif`, `fake|faux|escroquerie`, `non.payment|nicht.zahlen|impayé|gratis`, `drug|drogue|droge`, `pressure|drücker|preisdrücker`, `dangerous|gefährlich|dangereux`, `manipulieren|manipulate|manipulation`). Misses fall through to internal-raw-category `unknown` with severity null and contribute to `status: greylist` per the rules above. The comment is always stored verbatim. A later phase introduces a stronger extraction pass.

## Open Questions (do not block spec approval)

1. Bearer-token rotation cadence - decide before any external consumer is wired in.
2. Per-phone "mute" / takedown mechanism in the schema now, or later? Not blocking but cheap to include and matters for nFADP.
3. Auth-account custody: who owns the And6 escort account used for scraping? This is a legal-posture question (And6 ToS likely prohibits automated access; using a legitimate escort account at least keeps it inside the platform's intended user base). Resolve before public launch, not before Phase 1 dev.
4. Backing-API discovery: the recon flagged a likely XHR endpoint behind the SPA. Plan Step 0.1 captures it; if found, we use it and skip DOM walk.
5. Future Phase 1.5 candidates from the recon: `/my/escort/client-blacklist/admin-alerts` (admin-issued warnings, possibly with severity), the "Ajouter Client" submission form (canonical schema source), and `/comments/*` public surfaces (currently `Disallow` in robots.txt - decide policy).

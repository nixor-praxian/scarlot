# Scarlot safety-data recap

_Date: 2026-05-01 · Source repo: `scarlot-safety-data`_

## What Scarlot is

Scarlot is a personal-AI-agent platform for independent service providers. The first vertical is independent sex workers (TDS) in Switzerland; the same infrastructure is designed to extend to therapists, tutors, nannies, and other sole practitioners later. The agent meets users where they already work (messaging apps), and is built around modular **coverages**: Safety, Memory, Booking, Payments, Reputation, and so on. Each coverage is independently useful; together they replace the patchwork of spreadsheets, screenshots, and Telegram groups that workers use today to run their business.

Scarlot itself lives in the parent repo `scarlot-poc`. The product specification, market research, financial model, and decision history live there. The `scarlot-safety-data` sibling repo is one of the two data services that Scarlot is built on.

## The problem we are solving

From eight discovery interviews, the validated priority stack is consistent: the most acute, most urgent gap is **client safety screening**. Workers have no centralised way to check whether an unknown number is a known threat. The community-curated lists that do exist are platform-bound, slow to query, multilingual in unpredictable ways, and do not interoperate. Workers cope by maintaining personal blacklists, asking peers in side channels, or accepting bookings blind. The cost of misclassification is high: no-shows waste irreplaceable revenue windows; abusive or dangerous clients endanger health and safety.

This is the highest-evidence problem in the discovery dataset and the natural wedge for the platform.

## Why phone lookup as the first product

Phones are the universal identifier across messaging apps, ad platforms, and community blacklists. A phone-keyed reverse lookup gives the worker a single binary answer at the moment they need it (before agreeing to a booking) without forcing them to re-organise their workflow around our app. The wedge fits messaging-app UX (paste a number, get an answer), respects the "agent-first, no UI" principle, and produces a usage signal we can build the rest of the platform on.

The value proposition is "is this number safe to engage with, right now, in one query." Everything downstream (memory of past clients, booking, payments, identity) builds on the relationship we earn by getting that one answer right.

## How the safety wedge is built

`scarlot-safety-data` is the data backbone for the Safety coverage. It scrapes phone-keyed client reports from sex-worker platforms, normalises them, and exposes a tenant-scoped reverse-lookup API. The architecture in 30 seconds:

- **Adapters** scrape each source (Phase 1: And6, a Swiss-DACH community blacklist).
- **Inference** classifies each comment into a category using a deterministic regex pass and a fuzzy fallback.
- **Storage** persists the normalised report under a phone-keyed unique constraint.
- **API** (`POST /v1/phones/lookup`) returns a consolidated verdict (`blacklist | greylist | clean | unknown`), the categories that drove it, a confidence score, and a one-line summary.

The first source (And6) holds 10,228 ingested reports across roughly 7,876 distinct phone numbers, growing roughly linearly since 2012. A second source is planned for Phase 5.

## Current state (May 2026)

| Phase | What ships | State |
|---|---|---|
| 0 | Source recon, sanitised fixture, DPIA stub | done |
| 1 | Repo scaffold, Postgres + Alembic, CLI surface | done |
| 2 | Phone normalisation, taxonomy, inference, scoring, summary, upsert | done |
| 3 | And6 adapter, runner, full ingest | done |
| 4 | EDA + LLM-assisted inference quality pass | done |
| 5 | Multi-tenant API + scheduler | not started |
| 6 | Operations and documentation | partial |
| 7 | Acceptance walk against spec | not started |

The Phase 4 LLM work is the most recent. It is documented in `scarlot-safety-data/analysis/and6-dgx-findings.md` and summarised below.

## What the LLM pass validated

A local large-language model (`qwen2.5:7b-instruct`, run on a NVIDIA DGX Spark) was used to enrich the deterministic regex classifier. Key outcomes:

- The deterministic regex labels 51.9% of records as `unknown`. The LLM closes that gap to 3.2% on the merged corpus (regex first, LLM fills the rest).
- Co-assigned agreement (where both regex and LLM gave a public bucket) is **79.9%** on the public 7-bucket axis. This sits at the boundary of "augment" versus "replace"; for now, regex stays as the request-path classifier and the LLM runs as an enrichment pass.
- The LLM independently rediscovered a category gap (`health_risk`, for unprotected-sex and condom-refusal records). 286 records (2.8% of the corpus) fall in this bucket. It is already in the v1.1 raw enum, mapped to the public `dangerous` status, and will be surfaced as a first-class raw label going forward.
- The LLM is **decisively better than the regex on the highest-severity bucket (`dangerous`)**, both correcting regex over-fires (78% of the regex-flagged cell turned out not to be life-threatening) and recovering 208 real high-severity records the regex missed entirely.

The four-tier classification stack, the per-language localisation workflow, and the taxonomy-evolution rules are documented in `scarlot-safety-data/docs/taxonomy-evolution.md`. The reusable iteration loop runs end-to-end in roughly one engineering day plus three hours of compute on the DGX Spark per source.

## What's next

1. **Phase 5: multi-tenant API.** Auth, scheduler, observability. The taxonomy is locked in; the API contract (`scarlot-poc/docs/poc/specs/safety-scraper/Trojan Horse - Shape expected.md`) is locked. Implementation is the remaining work.
2. **Second source.** A second platform feeds the multi-source diversity factor into the confidence formula and stress-tests the per-language localisation workflow.
3. **`health_risk` as a public category.** Decide with the API contract owner whether to expose it as the sixth public bucket or leave it as a raw category surfaced via the existing `dangerous` mapping.
4. **DPIA closeout.** Open privacy questions in `scarlot-safety-data/docs/dpia-stub.md` must resolve before any external consumer is wired in.

## Pointers

| Topic | Where |
|---|---|
| Product specification (canonical) | `scarlot-poc/docs/poc/specs/safety-scraper/Trojan Horse - Spec.md` |
| Implementation plan | `scarlot-poc/docs/poc/specs/safety-scraper/Trojan Horse - Plan.md` |
| Locked API response contract | `scarlot-poc/docs/poc/specs/safety-scraper/Trojan Horse - Shape expected.md` |
| Taxonomy v1.1 | `scarlot-safety-data/docs/taxonomy.md` |
| Taxonomy evolution and localisation runbook | `scarlot-safety-data/docs/taxonomy-evolution.md` |
| Phase 4 LLM findings | `scarlot-safety-data/analysis/and6-dgx-findings.md` |
| Privacy / DPIA | `scarlot-safety-data/docs/dpia-stub.md` |
| Phase changelog | `scarlot-safety-data/docs/progress.md` |
| Architecture | `scarlot-safety-data/docs/architecture.md` |

## Constraints worth keeping in mind

- **Privacy is the binding constraint.** Phone numbers and free-text comments are PII; the scraped raw payloads never leave gitignored directories or the user's hardware. Local LLM inference (DGX Spark + Tailscale) was chosen over hosted APIs for this reason.
- **Swiss legal posture.** Flat-subscription only, no commission (firewalls Art. 195 StGB). nFADP compliance required pre-launch.
- **Single-tenant first.** Phase 1 ships a single tenant; multi-tenancy lands in Phase 5 to avoid prematurely solving the access-control surface.
- **Conservative public taxonomy.** The five public categories (`time_waster`, `no_show`, `abusive`, `scammer`, `dangerous`) are deliberately small. The internal raw enum (eleven values) is wider and grows as we learn; the public surface only grows when a downstream consumer asks for it.

# Scarlot

Discovery and strategy workspace for **Scarlot** -- the first coverage pack on **Maia**, a personal AI agent platform for independent service providers.

Scarlot serves independent sex workers (TDS) in Switzerland. The agent lives where the TDS already works (messaging apps), managing client relationships, safety screening, and business operations through conversation.

This repository is not the product codebase. It holds the research, user interviews, product specification, market intelligence, financial model, and fundraising materials that inform the product being built. A test implementation of the bot engine is included as a submodule but is not the canonical architecture.

## The problem (validated across 8 interviews)

| Rank | Problem | Evidence |
|------|---------|----------|
| 1 | No centralized blacklist | Gold - one interviewee built her own scraper |
| 2 | No inbound filtering | Gold - even power users don't check systematically |
| 3 | Cognitive overload | Gold - screening, scheduling, remembering, warning all manual |
| 4 | No-shows and time-wasters | Gold - mood-dependent blacklisting loses data |
| 5 | No client memory | Gold - 6/8 interviewees built workarounds |

Full priority ledger with 16 problems and evidence tiers in [`docs/priority-evolution.md`](docs/priority-evolution.md).

## The approach

**Maia** is the platform: modular coverages (Safety, Memory, Booking, Payments, Reputation, Compliance, Identity) that any independent service provider can combine. The platform is channel-agnostic and provider-agnostic. Vertical specialization lives in the coverage configuration, not the legal entity.

**Scarlot** is the first coverage pack, tuned for TDS. Sex workers are the first vertical because the pain is most acute and the co-founder has insider community access. Other coverage packs (Soigneur for therapists, Mentor for tutors, Gardien for nannies, Artisan for freelancers) are mapped in [`docs/claw-platform-vision.md`](docs/claw-platform-vision.md) and come after Scarlot validation.

### Design principles

1. **Agent-first, no UI** - the agent lives in messaging. No standalone app in Phase 1.
2. **Semi-automatic, not automatic** - the agent responds when asked. It does not read messages passively or reply to clients autonomously.
3. **Individual first, collective second** - personal tools must work before any shared features.
4. **Flat subscription only** - never commission (Art. 195 StGB firewall).

Full product specification in [`docs/scarlot-product-spec.md`](docs/scarlot-product-spec.md).

## Stage

Discovery complete. Product spec written. Market intelligence pipeline running. Financial model v1 done. Legal analysis complete. Pre-seed fundraising prep underway.

- **8 user interviews** with evidence-tiered analysis
- **5,229+ unique Swiss TDS identities** tracked via proprietary scraper (separate repo)
- **Product specification** integrating co-founder product brief with discovery findings
- **Business plan foundation** with TAM/SAM/SOM, unit economics, risk analysis
- **Financial model** with 36-month monthly projections, scenario toggles, Swiss-specific costs

## Repository structure

```
docs/
├── scarlot-product-spec.md              # Canonical product specification
├── claw-platform-vision.md              # Maia platform vision
├── business-plan-structure.md           # Two-round fundraising structure
├── counter-arguments.md                 # Rebuttals to 10 objections
├── founder-memo.md                      # Narrative memo
├── scarlot-market-research-report.md    # Market research (22+ sources)
├── scarlot-market-research.html         # Interactive visualization
├── safety-lookup-landscape.md           # Competitive catalog
├── poc-architecture.md                  # POC architecture notes
├── priority-evolution.md                # Priority evolution ledger
├── scarlot_discovery_report_v1.md       # Original discovery report
├── scarlot_visual_report_v1.html        # Interactive visual report
├── interviews/                          # 8 structured interview records
├── syncs/                               # Internal sync records
├── financial/                           # Financial model (xlsx)
├── market-intelligence/                 # Policy and market landscape notes
├── screenshots/                         # Competitive evidence
└── skills/                              # Skill installation guide

claw/CLAUDE.md                           # Group chat co-pilot prompt
engine/                                  # NanoClaw - bot engine (submodule, POC only)
CLAUDE.md                                # Project guidance for Claude Code
SETUP.md                                 # How to run the POC bot
```

## Related repositories

| Repo | Purpose |
|------|---------|
| `scarlot-market-data` | Proprietary market intelligence pipeline. Scrapes 8 Swiss platforms continuously. 7,221+ deduplicated worker identities. Private. |
| `nanoclaw` | Messaging bot engine used as the POC. Included here as submodule. |

## Running the POC

The `engine/` submodule contains NanoClaw, a WhatsApp bot used to test conversation flows during discovery. It is not the production architecture.

```bash
git clone --recursive https://github.com/nixor-praxian/scarlot.git
cd scarlot/engine
claude
```

Then run `/setup` inside Claude Code. Full setup guide in [`SETUP.md`](SETUP.md).

## Privacy

All files in this repository are free of personally identifiable information. Interview participants use pseudonyms. Team members are referenced by role (co-founder, advisor). Data storage must comply with Swiss nFADP. Legal review pending for collective blacklist features.

## License

Private repository. All rights reserved.

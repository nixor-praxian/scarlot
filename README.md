# Scarlot

Discovery and strategy workspace for **Scarlot** -- the first coverage pack on **Maia**, a personal AI agent platform for independent service providers.

Scarlot serves independent sex workers (TDS) in Switzerland. The agent lives where the TDS already works (messaging apps), managing client relationships, safety screening, and business operations through conversation.

This repository is not the product codebase. It holds the research, user interviews, product specification, market intelligence, financial model, and fundraising materials that inform the product being built. The product code lives in the sibling repositories listed below.

## The problem (validated across 8 interviews)

| Rank | Problem | Evidence |
|------|---------|----------|
| 1 | No centralized blacklist | Gold - one interviewee built her own scraper |
| 2 | No inbound filtering | Gold - even power users don't check systematically |
| 3 | Cognitive overload | Gold - screening, scheduling, remembering, warning all manual |
| 4 | No-shows and time-wasters | Gold - mood-dependent blacklisting loses data |
| 5 | No client memory | Gold - 6/8 interviewees built workarounds |

Full priority ledger with 16 problems and evidence tiers in [`docs/discovery/priority-evolution.md`](docs/discovery/priority-evolution.md).

## The approach

**Maia** is the platform: modular coverages (Safety, Memory, Booking, Payments, Reputation, Compliance, Identity) that any independent service provider can combine. The platform is channel-agnostic and provider-agnostic. Vertical specialization lives in the coverage configuration, not the legal entity.

**Scarlot** is the first coverage pack, tuned for TDS. Sex workers are the first vertical because the pain is most acute and the co-founder has insider community access. Other coverage packs (Soigneur for therapists, Mentor for tutors, Gardien for nannies, Artisan for freelancers) are mapped in [`docs/product/claw-platform-vision.md`](docs/product/claw-platform-vision.md) and come after Scarlot validation.

### Design principles

1. **Agent-first, no UI** - the agent lives in messaging. No standalone app in Phase 1.
2. **Semi-automatic, not automatic** - the agent responds when asked. It does not read messages passively or reply to clients autonomously.
3. **Individual first, collective second** - personal tools must work before any shared features.
4. **Flat subscription only** - never commission (Art. 195 StGB firewall).

Full product specification in [`docs/product/product-spec.md`](docs/product/product-spec.md).

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
├── product/                             # Product spec, platform vision, counter-arguments, founder memo
├── discovery/                           # Discovery report, priority ledger, interviews, sync records
├── market/                              # Market research, safety-lookup landscape, policy snapshots
├── business/                            # Business plan structure, financial model
├── poc/                                 # POC architecture and specs (Trojan Horse, Landing Page)
└── meta/                                # Skill installation and usage guides

CLAUDE.md                                # Project guidance for Claude Code
```

## Related repositories

| Repo | Purpose |
|------|---------|
| `hernest` | Product codebase. WhatsApp-resident assistant for TDS. Per-tenant isolation, admin-chat-only orchestration, 17 tools across clients, appointments, payments, blacklist. |
| `scarlot-website` | Public landing page and signup flow. Phone in, QR out. |
| `scarlot-safety-data` | Reverse-lookup backbone. Aggregates community blacklists into a phone-keyed API. Phase 1 source ingested (And6, 10,228 reports). |
| `scarlot-market-data` | Proprietary market intelligence pipeline. Scrapes 8 Swiss platforms continuously. 7,221+ deduplicated worker identities. Private. |

A snapshot of how the three product repos fit together is in [`docs/state-2026-05-01.md`](docs/state-2026-05-01.md).

## Privacy

All files in this repository are free of personally identifiable information. Interview participants use pseudonyms. Team members are referenced by role (co-founder, advisor). Data storage must comply with Swiss nFADP. Legal review pending for collective blacklist features.

## License

Private repository. All rights reserved.

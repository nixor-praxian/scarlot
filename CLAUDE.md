# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Scarlot** is the first coverage pack on **Maia** (codename) - a personal AI agent platform for independent service providers. Scarlot serves independent sex workers (TDS) in Switzerland. The agent lives where the TDS already works (messaging apps), managing client relationships, safety screening, and business operations through conversation.

The platform concept: modular **coverages** (Safety, Memory, Booking, Payments, Reputation, etc.) that any independent service provider can combine. Sex workers are the first vertical because the need is most acute and we have insider community access. The same infrastructure serves therapists, tutors, nannies, and other sole practitioners.

**Stage**: Discovery complete (8 interviews). Product spec written. Market intelligence pipeline running. POC development next.

**Co-founder**: Insider credibility within the TDS community, Geneva-based. Authored the product brief (Feb 2026) defining the 7 feature blocs and client file schema.
**Advisor**: Product strategy, 15+ years B2B software.

## Privacy

**All files in this repository must be free of personally identifiable information.** Use roles (co-founder, advisor, interviewer) instead of names. Use pseudonyms for interview participants. Generalize locations narrower than region level.

## Repository Structure

```
CLAUDE.md                                # This file - project guidance
SETUP.md                                 # How to run the bot (local or VPS)
scarlot-interview.skill                  # Interview analysis skill (ZIP archive)
scarlot-sync.skill                       # Internal sync record skill (ZIP archive)
claw/
└── CLAUDE.md                            # NanoClaw group prompt - project co-pilot bot
engine/                                  # NanoClaw submodule - the WhatsApp bot engine
docs/
├── scarlot-product-spec.md             # Unified product specification (canonical)
├── claw-platform-vision.md             # Platform vision - coverages, verticals, flywheel
├── scarlot-market-research-report.md   # Market research with risk analysis (22+ sources)
├── scarlot-market-research.html        # Interactive market research visualization
├── safety-lookup-landscape.md          # Competition & substitutes for safety lookup (comprehensive)
├── counter-arguments.md                # Naysayer rebuttals - 10 objections countered
├── business-plan-structure.md          # Two-round fundraising structure with real data
├── financial-model-prep.md             # Financial model preparation and assumptions
├── founder-memo.md                     # Narrative memo for co-founder recruitment
├── founder-onepager.html               # Visual one-pager for coffee meetings
├── scarlot_discovery_report_v1.md      # Original discovery report (historical - see note)
├── scarlot_visual_report_v1.html       # Interactive visual report
├── scarlot-graph.html                  # Ecosystem map visualization
├── poc-architecture.md                 # POC architecture (see status note in doc)
├── priority-evolution.md               # Priority evolution ledger (append-only, 8 entries)
├── screenshots/
│   ├── checkclient/                    # CheckClient.ch (Bemygirl) lookup tool screenshots
│   └── procore/                        # ProCoRe "Bad Client List" survey screenshots
├── skills/
│   └── how-to-use-skills.md            # Skill installation and usage guide
├── syncs/
│   └── SCARLOT_SYNC_*.md              # Internal sync/strategy session records
└── interviews/
    ├── SCARLOT_INT_*.md                # Structured interview records (8 total)
    └── transcripts/                    # Raw source material (docx, txt)
```

### Related Repositories

| Repo | Purpose |
|------|---------|
| `scarlot-market-data` | Proprietary market intelligence pipeline. Scrapes 7 Swiss platforms continuously. 5,229 deduplicated worker identities. Runs on DGX Spark. |
| `hermes` | CLI text-to-speech tool (provider-agnostic). Used for audio content generation. |

## Key Findings (Current State)

### Validated Priority Stack (from 8 interviews, updated via priority-evolution.md)

| Rank | Problem | Evidence |
|------|---------|----------|
| 1 | P5 - No centralized blacklist | Gold - GABRIELLE built a scraper to solve this |
| 2 | P3 - No inbound filtering | Gold - even power users don't check systematically |
| 3 | P4 - Cognitive overload | Gold - screening, scheduling, remembering, warning - all manual |
| 4 | P6 - No-shows & time-wasters | Gold - mood-dependent blacklisting = data loss |
| 5 | P2 - No client memory | Gold - 6/8 interviewees built workarounds |
| 6 | P9 - No client KYC | Silver - universal screening behavior |
| 7 | P8 - Manual repetition | Gold - WAB templates, pre-recorded messages |
| 8 | P15 - Data siloing across platforms | Silver - first real signal from INT8 |
| 9 | P16 - No income tracking | Silver - GABRIELLE built a bank notification parser |
| 10 | P1 - Messaging fragmentation | Weakening - 8 interviews, zero pain signal |

New candidate problems: P17 (platform identity mismatch), P18 (client reputation threats).

### Proprietary Market Data (from scarlot-market-data)

| Metric | Value |
|--------|-------|
| Unique workers identified (Switzerland) | 5,229 |
| FR-CH launch market | 1,238 |
| Geneva | 547 |
| Already paying for premium visibility | 56.4% |
| On 2+ platforms | 17.6% |

### Product Architecture - Coverages

The co-founder's 7 feature blocs map to platform coverages:

| Bloc | Coverage | Evidence | Phase |
|------|----------|----------|-------|
| Safety (MVP entry) | Safety + Memory | Gold | MVP |
| Framework & interactions | Switchboard | Silver | Phase 1 |
| Professional organization | Memory + Booking | Gold + Bronze | Phase 1 |
| Transactions | Payments + Compliance | Silver | Phase 2 |
| Analysis | Compliance | Silver | Phase 3 |
| Collective blacklist | Reputation | Supported but legally unresolved | Phase 2-3 |
| Resources | Identity (partial) | Co-founder insight | Phase 1 |

See `docs/scarlot-product-spec.md` for the full unified specification.

### Design Principles

1. **Agent-first, no UI** - the Claw lives in messaging. No standalone app in Phase 1. UI only if beta demands it.
2. **Semi-automatic, not automatic** - the Claw responds when asked. It does not read messages passively or reply to clients autonomously.
3. **Individual first, collective second** - personal tool must work before any shared features.
4. **Flat subscription only** - never commission. This is the legal firewall (Art. 195 StGB).

### Key Risks

| Risk | Severity | Mitigation |
|------|----------|------------|
| Messaging platform dependency | HIGH | Channel-agnostic architecture. No single platform is existential. |
| nFADP compliance (sensitive data) | HIGH | DPIA required pre-launch. On-device where possible. Swiss hosting. |
| FOSTA-SESTA reach | MEDIUM | Swiss entity, no US nexus, dual criminality defense. |
| Blacklist defamation | MEDIUM | Credit bureau model + NUM precedent + dispute mechanism. |

See `docs/counter-arguments.md` for full rebuttal briefing on 10 objections.

## Technical Direction

**Current engine**: NanoClaw (git submodule in `engine/`) - lightweight messaging bot on Claude Agent SDK. This is the POC implementation, not the permanent architecture.

**Principle**: Technology, channels, and interfaces are implementation details driven by market needs. The platform adapts to whatever channel the market uses.

**Patterns to borrow**: Memory extraction and contact schema from Aura.

### Still Pending

- Swiss legal counsel on Art. 195, nFADP, blacklist defamation - before launch
- Channel abstraction layer (WhatsApp is current, must support alternatives)
- AI provider evaluation (US-based providers create potential jurisdictional nexus)
- Hosting decision for beta (DGX Spark for scraper, separate for bot)

## Constraints

- **Privacy-first**: Swiss nFADP compliance required. No PII in committed files.
- **Legal structure**: Swiss Sàrl. Flat subscription pricing only. Never commission.
- **Semi-automatic**: TDS controls every decision. AI assists, never decides.
- **Conversational-native**: Primary UX through existing messaging apps.
- **Multilingual**: Users operate in French, English, German, and other languages.

## Skills

### Interview Skill
The `scarlot-interview` skill (installed at `~/.claude/skills/scarlot-interview/`) governs all user research analysis. Reference files live in the skill, not in this repo. See `references/context.md` within the skill for the source of truth on the 16 known problems and 10 open assumptions.

### Sync Skill
The `scarlot-sync` skill (installed at `~/.claude/skills/scarlot-sync/`) processes internal conversations into structured records. Two modes: Full Record (8-section) and Summary (6-section). Records go in `docs/syncs/`.

### Discovery Toolkit
The `discovery-toolkit` skill (installed at `~/.claude/skills/discovery-toolkit/`) is the generic, self-feeding version of the interview + sync skills. Used for other projects. Not Scarlot-specific.

### Meeting Minutes
The `meeting-minutes` skill (installed at `~/.claude/skills/meeting-minutes/`) processes any meeting transcript into structured records. Not discovery-specific.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Scarlot is a conversational-native platform for independent sex workers (TDS) in Switzerland. The platform aims to reduce operational friction and improve safety through WhatsApp/Telegram integration, a collective blacklist system, client CRM, and booking tools.

**Stage**: Pre-code discovery/POC. Research artifacts and user interviews are being collected. POC development is next.

**Co-founder**: Insider credibility within the TDS community, Geneva-based.
**Advisor**: Product strategy and POC lead, 15+ years B2B software.

## Privacy

**All files in this repository must be free of personally identifiable information.** Use roles (co-founder, advisor, interviewer) instead of names. Use pseudonyms for interview participants. Generalize locations narrower than region level.

## Repository Structure

```
CLAUDE.md                               # This file — project guidance for Claude Code
SETUP.md                                # How to run the bot (local or VPS)
scarlot-interview.skill                 # Interview analysis skill (ZIP archive)
claw/
└── CLAUDE.md                           # NanoClaw group prompt — project co-pilot bot
docs/
├── scarlot_discovery_report_v1.md      # Primary discovery report (canonical reference)
├── scarlot_visual_report_v1.html       # Interactive visual report
├── scarlot-graph.html                  # Ecosystem map visualization
├── poc-architecture.md                 # POC architecture, onboarding, contacts/calendar sync
├── priority-evolution.md               # Priority evolution ledger (append-only)
└── interviews/
    ├── SCARLOT_INT_*.md                # Structured interview records
    └── *-transcript.txt                # Raw transcripts
```

## Key Discovery Findings

### Priority Problems (build order)
1. **Gold (build with confidence)**: No inbound filtering (P3), cognitive overload (P4), no centralized blacklist (P5)
2. **Silver (build with validation)**: Messaging fragmentation (P1), no client memory (P2), time-wasters (P6), blacklist cost (P7), manual repetition (P8)

### Proposed Solution Concepts
- **C-01**: Shared Blacklist Network — collective flagged-number database
- **C-02**: Conversational Interface — WhatsApp/Telegram agent (no new app)
- **C-03**: Client Intelligence Profile — auto-populated lightweight CRM
- **C-04**: Inbound Message Triage — automatic flag/filter with user control
- **C-05**: Pre-Booking Verification — phone lookup + optional ID
- **C-06**: Availability Broadcasting — natural language scheduling
- **C-07**: Revenue Tracker — income logging (V2)

### POC Scope (next phase)
- WhatsApp bot on TDS's own number (linked device via baileys)
- TDS configures the bot via conversational onboarding in admin chat
- Bot screens incoming client messages based on TDS-defined rules
- Personal blacklist + client notes (no shared complexity initially)
- Success criteria: self-test passes, then 1 beta TDS runs it for a week
- See `docs/poc-architecture.md` for full architecture and onboarding flow

## Technical Direction

**Base**: Fork/adapt NanoClaw (`/Users/node/GitHub/nanoclaw`) — lightweight WhatsApp bot on Claude Agent SDK.
**Patterns to borrow**: Memory extraction and contact schema from Aura (`/Users/node/GitHub/aura`).
**Not using**: Twenty CRM — too heavy for this use case.

**Key architecture decision**: TDS uses their own WhatsApp number. Bot connects as a linked device. TDS manages the bot through a private admin chat within WhatsApp. See `docs/poc-architecture.md` for full detail.

### Still pending
- Hosting (advisor's machine for self-test, then cloud for beta)
- Telegram support timeline (NanoClaw has a skill for this)
- Swiss nFADP compliance review for data storage

## Constraints

- **Privacy-first**: Swiss nFADP compliance required for all data storage. No PII in committed files.
- **Legal risk**: Collective blacklist may have defamation liability — legal review needed
- **Conversational-native**: Primary UX must be through existing messaging apps, not a standalone app
- **Multilingual**: Users operate in French, English, and other languages

## Interview Skill

The `scarlot-interview` skill (installed at `~/.claude/skills/scarlot-interview/`) governs all user research analysis. Reference files live in the skill, not in this repo. See `references/context.md` within the skill for the source of truth on the priority stack, 16 known problems, and 10 open assumptions.

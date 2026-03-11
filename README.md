# Scarlot

A conversational assistant for independent sex workers (TDS) in Switzerland. Scarlot connects to a TDS's existing WhatsApp number and handles client screening, booking, and safety — all through the messaging app they already use.

No new app to install. No dashboard to learn. The TDS configures everything by chatting with the bot.

## How It Works

```
TDS's WhatsApp number
│
├── Personal conversations (friends, family)
│   → Bot stays silent
│
├── Client conversations (unknown numbers)
│   → Bot handles triage, screening, booking
│   → TDS can take over any conversation at any time
│
└── Admin chat (TDS ↔ Scarlot, private)
    → Configure tone, rates, availability, rules
    → Review flagged conversations
    → Update blacklist
    → See daily summary
```

The bot connects as a **linked device** (like WhatsApp Web). The TDS keeps their phone, their number, their contacts. Scarlot handles the rest.

### What the client sees

A normal WhatsApp conversation. They message the number from an ad, get a professional response matching the TDS's style, and either book or don't. They never know it's an assistant.

### What the TDS controls

Everything — through natural language in the admin chat:

| What they say | What happens |
|---|---|
| "Je suis dispo demain de 14h a 22h" | Updates availability |
| "Bloque le dernier numero qui m'a ecrit" | Adds to blacklist |
| "Sois plus directe avec les clients" | Updates tone |
| "Pause" | Bot stops responding |
| "Je prends des vacances jusqu'au 15" | Auto-responds to all clients |

## Project Status

**Stage**: Discovery / early POC. User interviews are underway, core conversation flow is being tested.

### Validated problems (Gold evidence)

| Priority | Problem | Why it matters |
|---|---|---|
| P4 | Cognitive overload | TDS juggle 3-5 tools, constant context switching |
| P3 | No inbound filtering | Clients have unrestricted access, no screening |
| P5 | No centralized blacklist | Scattered across platforms, expensive |

### What we're building first

1. Bot responds to unknown numbers using TDS-defined rules
2. Time-waster detection (disengages after configurable threshold)
3. Personal blacklist (phone number lookup)
4. Client notes extraction (auto-summarize after each conversation)

See [`docs/poc-architecture.md`](docs/poc-architecture.md) for the full architecture, onboarding flow, and contacts/calendar sync plan.

## Repository Structure

```
scarlot/
├── claw/CLAUDE.md           # Bot personality for project co-pilot (WhatsApp group)
├── engine/                  # NanoClaw — WhatsApp bot engine (git submodule)
├── docs/
│   ├── scarlot_discovery_report_v1.md   # Full discovery report
│   ├── poc-architecture.md              # POC architecture & onboarding
│   ├── priority-evolution.md            # Priority ledger (updated per interview)
│   └── interviews/                      # Structured interview records
├── CLAUDE.md                # Project guidance for Claude Code
└── SETUP.md                 # How to run the bot
```

## Getting Started

### Prerequisites

- Node.js 20+
- Docker
- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) with an active subscription or API key
- A WhatsApp account you can link a device to

### Clone and set up

```bash
git clone --recursive https://github.com/nixor-praxian/scarlot.git
cd scarlot/engine
claude
```

Then run `/setup` inside Claude Code. It handles dependency installation, container build, and WhatsApp authentication (QR code scan).

See [`SETUP.md`](SETUP.md) for the full guide including VPS deployment.

## Tech Stack

- **Engine**: [NanoClaw](https://github.com/nixor-praxian/nanoclaw) — lightweight WhatsApp bot on Claude Agent SDK
- **Runtime**: Node.js + TypeScript
- **WhatsApp**: baileys (linked device protocol)
- **AI**: Claude via Claude Agent SDK (runs in isolated containers)
- **Database**: SQLite (messages, sessions, client notes)
- **Contacts/Calendar** (planned): CalDAV/CardDAV via tsdav — works with Google, Apple, any provider

## Privacy

All files in this repository are free of personally identifiable information. Interview participants use pseudonyms. Team members are referenced by role (co-founder, advisor), not by name.

Data storage must comply with Swiss nFADP. Legal review pending for collective blacklist features.

## License

Private repository. All rights reserved.

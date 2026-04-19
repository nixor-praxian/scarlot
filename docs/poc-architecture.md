# Scarlot POC - Architecture & Onboarding

> **Status note (April 2026):** This document describes the original WhatsApp-linked-device architecture. The unified product spec (`scarlot-product-spec.md`) now governs product direction with two key changes:
>
> 1. **Semi-automatic principle** - the agent responds when asked. It does not read messages passively or reply to clients autonomously. This reflects the co-founder's non-intrusion principle ("Scarlot ne voit pas les conversations") and the A7 finding from 8 interviews (passive checks = yes, active conversation = no).
> 2. **Channel resilience** - WhatsApp dependency is the #1 risk (Meta is a US company, ToS prohibits adult services, baileys causes bans). The architecture must survive channel eviction. No single messaging platform is existential.
>
> Core concepts in this document - admin chat, onboarding flow, client interaction model, takeover detection - remain valid inputs. The delivery mechanism will be validated through beta. See `scarlot-product-spec.md` for the current product direction.

## Decision: TDS uses their own WhatsApp number

The TDS keeps their existing phone number - the one already published on ad platforms. Scarlot connects to it as a **linked device** (like WhatsApp Web), running alongside the TDS's normal phone usage.

The TDS never installs a new app. They manage their assistant through a **private admin chat** within WhatsApp itself.

## How it works

### Two layers on one number

```
TDS's WhatsApp number
│
├── Personal conversations (friends, family)
│   → Bot does NOT touch these
│
├── Client conversations (unknown numbers)
│   → Bot handles triage, screening, booking
│   → TDS can take over any conversation at any time
│
└── Admin chat (TDS ↔ Scarlot bot, private)
    → Configure the bot: tone, rates, availability, rules
    → Review flagged conversations
    → Update blacklist
    → See daily summary
```

### How the bot knows what to handle

Default rule: **if the number is not in the TDS's phone contacts, it's a client.** The bot responds.

If the number IS a saved contact, the bot stays silent - it's a personal conversation.

The TDS can override this in the admin chat:
- "Handle this number too" (a regular who should still go through the bot)
- "Ignore this number" (a new contact the TDS wants to talk to directly)

### Takeover

The TDS can jump into any client conversation at any time by simply typing on their phone. When the bot detects the TDS replied directly, it steps back and stops responding in that thread until told otherwise (or until a configurable cooldown, e.g., 2 hours of silence from the TDS).

## Onboarding flow

### Step 1 - Connect (2 min)

Scarlot runs on a server (or the advisor's laptop for the POC). The TDS opens a link that shows a QR code. They scan it from WhatsApp > Linked Devices. Done - the bot is connected to their number.

### Step 2 - Admin chat appears

The bot sends a first message to the TDS's self-chat (the "Note to self" chat in WhatsApp, or a dedicated group created for this purpose):

> Salut! Je suis ton assistant Scarlot. Je vais t'aider a gerer tes messages clients.
>
> Je vais te poser quelques questions pour savoir comment me comporter. Tu peux changer tout ca a tout moment en me parlant ici.

### Step 3 - Guided configuration (10-15 min, conversational)

The bot asks questions one at a time. Each answer updates the agent's configuration (stored as a `CLAUDE.md` file on the server).

**Identity & tone:**
> Comment tu veux que je m'appelle quand je parle a tes clients?

> Comment tu veux que je parle? Plutot pro et direct, ou chaleureux et decontracte?

> En quelle(s) langue(s)? (francais, anglais, les deux, autre?)

**Services & rates:**
> Quels sont tes tarifs? (par exemple: 1h = 300 CHF, 2h = 500 CHF)

> Incall, outcall, ou les deux?

> Y a-t-il des choses que tu ne proposes PAS et que les clients demandent souvent? (je refuserai poliment pour toi)

**Availability:**
> Quels jours et heures tu travailles cette semaine?

> Combien de temps a l'avance minimum pour un rendez-vous?

**Safety & filtering:**
> Y a-t-il des numeros a bloquer? (envoie-les moi, je les ajouterai a ta liste noire)

> Apres combien de messages sans reservation je dois couper la conversation poliment? (par defaut: 5)

**Booking:**
> Comment tu veux confirmer un rendez-vous? Je te previens ici et tu confirmes, ou je confirme directement si tout est ok?

### Step 4 - Test (5 min)

> C'est bon! Essaie de m'envoyer un message depuis un autre telephone pour voir comment je reponds. Si tu veux changer quelque chose, dis-le moi ici.

### Step 5 - Go live

> Tout est pret. Je gere tes messages entrants. Tu recevras un resume chaque soir ici.
>
> Tu peux me dire a tout moment:
> - "change mes dispos" - mettre a jour tes horaires
> - "bloque ce numero" - ajouter a la liste noire
> - "montre mes conversations" - voir les clients du jour
> - "pause" - j'arrete de repondre
> - "reprends" - je recommence

## Admin chat commands (natural language)

The TDS talks to the bot in natural language in the admin chat. No slash commands, no menus. Examples:

| What the TDS says | What happens |
|---|---|
| "Je suis dispo demain de 14h a 22h" | Updates availability |
| "Bloque le dernier numero qui m'a ecrit" | Adds to blacklist |
| "Montre-moi la conversation avec le +41..." | Shows transcript |
| "Sois plus directe avec les clients" | Updates tone in config |
| "Combien de messages aujourd'hui?" | Shows daily stats |
| "Pause" | Bot stops responding to clients |
| "Je prends des vacances jusqu'au 15" | Sets unavailable, auto-responds to all clients |

## What the client sees

A client finds the TDS on an ad platform. The ad shows a WhatsApp number (the TDS's real number). They tap it. Normal WhatsApp conversation:

> **Client:** Salut, tu es dispo ce soir?
>
> **Bot (as the TDS):** Bonsoir! Oui, je suis disponible ce soir a partir de 20h. Outcall uniquement (hotel). 1h 300 CHF, 2h 500 CHF. Tu as un hotel en tete?
>
> **Client:** C'est quoi tes services exactement?
>
> **Bot:** Je prefere discuter des details en personne. Si tu veux reserver, dis-moi l'heure et le lieu.
>
> **Client:** [vague question]
> **Client:** [another vague question]
> **Client:** [still no booking]
>
> **Bot:** Je suis disponible ce soir a 20h. Souhaites-tu confirmer un rendez-vous? Sinon, n'hesite pas a me recontacter quand tu es pret.

The client never knows it's an assistant. The conversation feels like a professional, efficient TDS who doesn't waste time.

### Time-waster handling

If a client keeps asking questions without booking (threshold configurable, default 5 messages):

> Merci pour ton interet. Je suis disponible [next slot]. Contacte-moi quand tu es pret a reserver. Bonne soiree!

The bot stops responding after that. The number gets flagged as "low-intent" in client notes.

### Blacklisted number

If a number is on the blacklist:

> Desole, je ne suis pas disponible pour le moment.

No further engagement. No explanation.

## Technical foundation

### Base: NanoClaw (fork/adapt)

NanoClaw provides the plumbing:
- WhatsApp connection via baileys (linked device protocol)
- Per-conversation agent with its own `CLAUDE.md` system prompt
- SQLite message persistence
- Claude Agent SDK for AI responses
- Container isolation per conversation
- Task scheduler for reminders and daily summaries
- MCP tools for send_message, schedule_task

Repository: `/Users/node/GitHub/nanoclaw`

### What to add for Scarlot

1. **Contact classification** - personal contact vs. client (check against phone contacts list or a whitelist)
2. **Takeover detection** - when TDS replies directly, bot steps back
3. **Blacklist table** - phone number lookup in SQLite, shared across all client conversations
4. **Client notes extraction** - after each conversation, extract: intent level, preferences, red flags (pattern from Aura's memory system)
5. **Onboarding conversation** - guided Q&A in admin chat that builds the `CLAUDE.md`
6. **Daily summary** - scheduled task that sends the TDS a recap in the admin chat
7. **Admin command routing** - messages in admin chat go to config mode, not client-response mode

### Contacts & calendar sync

Goal: OS and device agnostic. A TDS on iPhone with Google Calendar should work the same as one on Android with Apple Calendar.

**Standards used:**
- **CardDAV** - open standard for contact sync (Google, Apple, Nextcloud all support it)
- **CalDAV** - open standard for calendar sync (same providers)
- **Library**: `tsdav` (npm) - handles both CardDAV and CalDAV, works in Node.js

**Contacts - phased approach:**

| Phase | Method | How it works |
|---|---|---|
| Self-test | Manual whitelist | TDS lists personal numbers in admin chat. Bot stores them. |
| Beta | baileys `getContacts()` | Bot reads the WhatsApp contact list directly via the linked device connection. If a number is a saved contact, it's personal. No extra auth needed. |
| V1 | CardDAV via tsdav | Full sync with Google Contacts / iCloud / any CardDAV provider. TDS authorizes once. Bot sees all contacts including notes and labels. |

**Calendar - phased approach:**

| Phase | Method | How it works |
|---|---|---|
| Self-test | Text in config | TDS says "dispo mardi-samedi 10h-22h" in admin chat. Stored in CLAUDE.md. |
| Beta | CalDAV read-only via tsdav | Bot reads TDS's existing calendar. If a time slot has an event, the bot doesn't offer it. One-time OAuth. |
| V1 | CalDAV read-write via tsdav | Bot creates calendar events when bookings are confirmed. TDS sees them on their phone like any appointment. |

**Why CalDAV/CardDAV and not provider-specific APIs:**
- Works with any provider: Google, Apple, Microsoft (via connector), Nextcloud, Radicale, Baikal
- TDS doesn't need to know or care what protocol is used - they authorize their account once
- If a TDS wants full data sovereignty, they can self-host a Radicale server (Python, flat-file storage)
- One library (`tsdav`) handles all providers

### Patterns to borrow from Aura

Repository: `/Users/node/GitHub/aura`

- **Memory extraction** (`src/memory/extract.ts`): after each conversation, a fast-model call extracts facts about the client
- **Contact schema** (`src/db/schema.ts`): people + addresses tables, channel-agnostic
- **System prompt engineering** (`src/personality/system-prompt.ts`): dense personality prompt structure

### Not using Twenty

Twenty CRM is too heavy for this use case. A TDS's "CRM" is:
- Conversation history (already in SQLite via NanoClaw)
- Client notes per phone number (new, lightweight)
- A blacklist (new, lightweight)
- Availability (in the CLAUDE.md config)

No GraphQL, no multi-tenant workspaces, no workflow engine needed.

## Self-test plan

Before involving any TDS, test with our own numbers:

1. Set up NanoClaw on the advisor's machine
2. Connect to a personal WhatsApp number (scan QR)
3. Write a test `CLAUDE.md` with a made-up persona
4. Have the co-founder message from their phone as a "client"
5. Evaluate: Does the bot respond appropriately? Is the tone right? Does it filter correctly?
6. Iterate on the `CLAUDE.md` until the conversation feels natural
7. Then try the onboarding flow: wipe the config and let the bot ask the questions

### Success criteria for self-test

- [ ] Bot responds to unknown numbers within 10 seconds
- [ ] Bot does NOT respond to saved contacts
- [ ] Tone matches what was configured
- [ ] Bot refuses out-of-scope requests gracefully
- [ ] Bot disengages from time-wasters after threshold
- [ ] TDS can take over a conversation by replying directly
- [ ] Admin chat commands update the bot's behavior
- [ ] Daily summary arrives at configured time

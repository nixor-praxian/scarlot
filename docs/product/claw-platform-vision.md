# Maia Platform - Strategic Vision

*An AI agent platform for independent service providers.*
*Codename: Maia - Greek Titan, mother of Hermes the messenger god. She nurtures what the messenger carries.*
*Draft - April 2026*

---

## The Insight

Every independent service provider - sex worker, masseuse, therapist, personal trainer, freelance consultant, private tutor, nanny - faces the same operational friction:

- **Who is this person?** (Safety / screening)
- **Have I seen them before?** (Client memory / CRM)
- **When am I available?** (Scheduling / booking)
- **Did they pay?** (Payments / escrow)
- **How is my business doing?** (Revenue tracking / compliance)

Today, each of these is solved by a disconnected tool designed for someone else. The independent provider stitches together WhatsApp, a spreadsheet, a calendar app, a payment link, and a notes app - and does the integration in their head.

**Maia replaces the stitching.** It is a personal AI agent that runs on the provider's existing messaging app, manages their client relationships, and handles operational friction through conversation.

---

## Architecture

### The Core Principle

Maia lives where the user already communicates with their clients. It meets them on their channel - whatever that channel is today, and whatever it becomes tomorrow.

The technology, the channel, and the interface are implementation details driven by market needs. What is constant is the concept: **a personal AI agent that manages your client relationships through conversation.**

The engine is channel-agnostic, AI-provider-agnostic, and interface-agnostic. It adapts to the market - not the other way around.

### Coverages: Modular Capability Bundles

A **coverage** is a named bundle of skills, configuration, and data schema that gives Maia a specific capability. Think of it like an insurance policy - you pick the coverages you need.

| Coverage        | Capability                                                                       | Data Schema                                          | Skills                                                                          |
| --------------- | -------------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------- |
| **Safety**      | Inbound screening, number lookup, risk flagging, collective warnings             | Contact risk flags, incident reports, warning alerts | `screen-inbound`, `check-blacklist`, `report-incident`, `alert-network`         |
| **Memory**      | Client CRM - who, when, preferences, notes, grades, history                      | Contact records, meeting logs, tags, notes           | `save-contact`, `recall-client`, `tag-contact`, `search-history`                |
| **Booking**     | Availability declaration, appointment confirmation, reminders, no-show tracking  | Calendar slots, bookings, cancellation records       | `set-availability`, `confirm-booking`, `send-reminder`, `log-noshow`            |
| **Payments**    | Deposit collection, cancellation fees, payment confirmation, receipt generation  | Transaction records, payment links, fee schedules    | `request-deposit`, `confirm-payment`, `charge-cancellation`, `generate-receipt` |
| **Switchboard** | Inbound routing, auto-reply, availability broadcast, multi-channel consolidation | Routing rules, message templates, broadcast lists    | `route-message`, `auto-reply`, `broadcast-availability`                         |
| **Reputation**  | Collective client database, shared warnings, peer reviews                        | Anonymized contact signals, community flags          | `contribute-signal`, `query-reputation`, `manage-consent`                       |
| **Compliance**  | Income logging, tax categorization, expense tracking, receipt archival           | Financial records, tax categories, receipts          | `log-income`, `categorize-expense`, `export-tax-report`                         |
| **Identity**    | Self-controlled profile, broadcast channels, direct marketing, brand management  | Profile data, content templates, subscriber lists    | `manage-profile`, `broadcast-update`, `manage-subscribers`                      |

### Coverage Packs: Vertical Presets

A **coverage pack** is a pre-configured combination of coverages tuned for a specific vertical. It is the onboarding accelerator - instead of picking coverages one by one, a new user selects a pack and customizes from there.

| Pack         | Target                             | Coverages Included                                  | Specific Tuning                                                              |
| ------------ | ---------------------------------- | --------------------------------------------------- | ---------------------------------------------------------------------------- |
| **Scarlot**  | Independent sex workers (TDS)      | Safety + Memory + Reputation + Booking              | Blacklist focus, safety-first screening, community warnings, WhatsApp-native |
| **Soigneur** | Independent therapists / masseuses | Memory + Booking + Payments + Compliance            | Health record notes, insurance integration hooks, appointment reminders      |
| **Mentor**   | Tutors / coaches / consultants     | Memory + Booking + Payments + Switchboard           | Session notes, package tracking, availability broadcasting                   |
| **Gardien**  | Nannies / home-care / pet-sitters  | Safety + Memory + Booking + Payments                | Reference checking, family profiles, recurring schedule management           |
| **Artisan**  | Freelance creatives / trades       | Memory + Booking + Payments + Compliance + Identity | Portfolio integration, quote generation, invoice management                  |

Scarlot is not a company - it is a coverage pack. The first one. The one that proves the platform.

---

## The Fine-Tuning Flywheel

### Level 1: Individual Tuning (Day 1)

Each user's Claw learns from its owner through conversation:

- **Screening rules:** "I don't take outcalls after 10pm" → Maia declines or flags late requests
- **Client vocabulary:** "Tag anyone who cancels twice as unreliable" → Maia auto-tags
- **Response patterns:** "When someone asks my rates, send them this" → template auto-reply
- **Risk thresholds:** "If a new number sends more than 5 messages before I reply, flag it" → behavioral detection

This is configuration through conversation, not through a settings panel. The TDS tells her Claw what she wants. Maia adapts.

### Level 2: Vertical Tuning (Month 1–6)

As users within a vertical accumulate patterns, the coverage pack gets smarter:

- **50 TDS flag the same phone number** → signal strength increases. No individual contributed "a blacklist" - the collective behavior emerges from individual safety actions.
- **200 masseuses see no-shows on Monday mornings** → the Booking coverage can warn new users: "Monday morning bookings in your area have a 40% no-show rate. Consider requiring deposits."
- **Pattern recognition:** Maia learns what "suspicious first message" looks like for sex workers (different from what it looks like for therapists). Each vertical develops its own behavioral fingerprint.

### Level 3: Cross-Vertical Tuning (Year 1+)

Patterns that transfer across verticals are the platform's deepest moat:

- **No-show prediction** looks similar across all personal services - message timing, cancellation history, day-of-week patterns
- **Payment friction** follows universal patterns - deposit avoidance, last-minute cancellations, negotiation behavior
- **Inbound screening heuristics** - volume, timing, language patterns of genuine vs. time-waster contacts
- **Revenue seasonality** - GABRIELLE already tracks this for sex work. The same analytics apply to any recurring-client business.

### The Defensibility

The technology is not the moat. Technology choices will evolve with the market.

The moat is:
1. **Accumulated behavioral intelligence** across verticals - trained by real users in real interactions
2. **Community trust** - each vertical's coverage pack is shaped by insiders, not outsiders
3. **Network effects on reputation data** - the collective blacklist/reputation system gets more valuable with each user
4. **Switching cost** - once your Claw knows your clients, your patterns, and your preferences, switching means starting from zero
5. **Channel resilience** - the platform adapts to whatever channel the market uses. If WhatsApp dies, Maia moves. The client data and intelligence survive.

---

## Legal Positioning

### The Entity

The legal entity is a **personal service provider platform** - not a sex work company, not an escort platform, not an adult services marketplace.

| What it is | What it is NOT |
|------------|---------------|
| AI agent infrastructure for independent service providers | A sex work platform |
| Modular capability platform (coverages) | An escort marketplace |
| Personal productivity + safety tool | A buyer-seller connector |
| CRM + booking + payments for sole practitioners | An adult content platform |

Scarlot is a brand, a community, a coverage pack. It is not the company.

### How This Changes the Risk Profile

| Risk                             | Scarlot-as-company                  | Claw-as-platform                        |
| -------------------------------- | ----------------------------------- | --------------------------------------- |
| Payment processing (Stripe etc.) | BLOCKED - sex work platform         | POSSIBLE - generic service marketplace  |
| App store approval               | BLOCKED - adult services            | POSSIBLE - personal service assistant   |
| FOSTA-SESTA exposure             | MEDIUM - purpose-built for sex work | LOW - generic tool, vertical-agnostic   |
| Investor framing                 | Niche, stigma risk                  | "Doctolib for independents" - large TAM |
| Founder reputation               | Sex work company founder            | Service platform founder                |
| Marketing/PR                     | Restricted channels                 | Standard B2B SaaS marketing             |

### The Critical Nuance

This is NOT about hiding the sex work use case. It is about building infrastructure that is genuinely generic - because the problems ARE generic - and letting vertical specialization live in the coverage configuration, not the legal entity.

The sex work vertical is the first because:
1. The pain is most acute (safety is life-or-death, not just convenience)
2. The co-founder has insider trust and community access
3. The discovery research (8 interviews, 16 validated problems) provides the deepest problem understanding
4. No one else is building for this population - zero competition for attention
5. If the system works for the hardest vertical, it works for everyone

---

## Go-To-Market

### Phase 1: Prove Maia (Now - Month 6)

- **Vertical:** Sex work (Scarlot coverage pack)
- **Market:** FR-CH (Geneva + Vaud)
- **Channel:** Whatever the users already use (currently WhatsApp-dominant per discovery research)
- **Coverages:** Safety + Memory
- **Users:** 10 beta → 50 active
- **Goal:** Product-market fit signal. Does Maia reduce operational friction? Do users come back?
- **Revenue:** None (or nominal - CHF 10/mo to test WTP)

### Phase 2: First Revenue + Second Vertical (Month 6–18)

- **Vertical 1:** Sex work (CH-wide expansion, Germany market entry)
- **Vertical 2:** First adjacent vertical (therapists/masseuses - chosen for similar pain profile and legal simplicity)
- **Coverages:** Safety + Memory + Booking + Payments
- **Users:** 200–500 across both verticals
- **Goal:** Revenue, second-vertical validation, coverage architecture proven
- **Revenue:** CHF 200K–500K ARR

### Phase 3: Platform (Month 18–36)

- **Verticals:** 3–5 (add tutors/coaches, nannies, freelance creatives)
- **Markets:** CH + DE + AT + BE + NL + UK + AU/NZ
- **Coverages:** Full suite available
- **Users:** 2,000–5,000
- **Goal:** Platform economics. Coverage packs self-serve. Community fine-tuning active.
- **Revenue:** CHF 1–3M ARR

---

## Pricing Model

Pricing follows the coverage model:

| Tier     | Coverages                                       | Price     | Target                          |
| -------- | ----------------------------------------------- | --------- | ------------------------------- |
| **Free** | Memory (basic - up to 50 contacts)              | CHF 0     | Trial, adoption                 |
| **Solo** | Memory + Booking                                | CHF 15/mo | Light users, second vertical    |
| **Pro**  | Memory + Booking + Safety + Payments            | CHF 35/mo | Primary use case                |
| **Pro+** | All coverages + Reputation network + Compliance | CHF 55/mo | Power users (GABRIELLE profile) |

Collective features (Reputation network) require Pro+ - this creates the upgrade incentive and funds the most legally sensitive feature.

---

*"Build generic infrastructure. Let vertical specialization emerge from community need. The hardest vertical first - if it works there, it works everywhere."*

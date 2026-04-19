# Scarlot - Product Specification

*Unified spec integrating co-founder's product brief (Feb 2026) with Claw platform architecture and 8 interviews of discovery research.*

*April 2026 - Living document. Evolves through beta.*

---

## Positioning

Le travail du sexe est un vrai travail. Scarlot est le premier outil professionnel conçu spécifiquement pour les TDS indépendant·es.

Scarlot is not another platform. It is professional infrastructure for an industry that has never had any. It integrates into existing channels without replacing them. It structures what was until now invisible and unmanaged: time-wasting interactions, boundaries that are hard to enforce, scattered client information, safety carried alone.

---

## Design Principles

### 1. Agent-first, no UI (Phase 1)

The first iteration has no standalone app, no dashboard, no separate interface. Maia lives where the TDS already works - inside their messaging app. Everything happens through conversation.

If and when a UI is needed, that decision comes from beta feedback, not from assumptions. The hypothesis: a conversational agent reduces friction more than a separate app because there is nothing new to learn, nothing new to open, and zero context-switching.

### 2. Semi-automatic, not automatic

Maia does not surveil. It does not read messages without being asked. It does not reply to clients autonomously.

What it does:
- **Responds when the TDS asks** - "Check this number," "What do I know about this person?"
- **Surfaces information proactively** - "This number is on your blacklist" (when the TDS forwards a contact)
- **Structures what the TDS tells it** - "Log this as a no-show," "Add a note: negotiated aggressively"

The TDS is always in control. The AI assists. It never decides.

This respects:
- The co-founder's non-intrusion principle ("Scarlot ne voit pas les conversations")
- A7 validated finding: passive checks = yes, active conversation = no
- Community resistance to AI replacing human interaction

### 3. Individual first, collective second

The personal tool must work and be trusted before any shared features are introduced. Collective blacklist (Phase 2) happens only after the individual MVP is validated. No one is asked to share anything until they've experienced the value of the personal tool.

### 4. Built from lived experience, not a feature list

Every capability maps to a validated problem from the 8 interviews. If it doesn't connect to a real pain point with Gold or Silver evidence, it waits.

### 5. Optional and customizable

Each TDS has their own rules, their own boundaries, their own workflow. Maia adapts to them - not the other way around. Configuration happens through conversation, not through settings panels.

---

## The Two Moments

*From the co-founder's product brief - validated by P3 (no inbound filtering) and P5 (no centralized blacklist).*

Every client interaction has two decision points:

### Moment 1 - First Contact
The TDS receives a message from a new number. They need to read the signals: tone, first requests, language, timing. **"Is it worth continuing?"**

What Maia does: When the TDS asks ("Check this number"), Maia instantly returns:
- Unknown - not in contacts. Offer to create a file.
- Known - already in contacts. Show the file: status, notes, history.
- Blacklisted - immediate alert with reason and date.

### Moment 2 - Verification
The first contact seems acceptable. But is this person real, serious, safe? **"Can I trust this?"**

What Maia does: On request, surfaces everything known:
- Personal blacklist match
- Collective blacklist match (Phase 2)
- Client history: previous appointments, no-shows, payment behavior
- Behavioral tags: negotiator, time-waster, aggressive, reliable
- Colleague reference: "GABRIELLE knows this person" (Phase 2)

---

## Blocs - Feature Architecture

*Mapped from co-founder's 7 blocs to platform coverages. Priority based on evidence tier from 8 interviews.*

### Bloc 1 - Safety (MVP Entry Point)

*Coverage: Safety + Memory*
*Evidence: P3 Gold, P5 Gold, P6 Gold - validated across all 8 interviews*
*Co-founder: "Porte d'entrée MVP - répond à une peur réelle et immédiate"*

| Feature | How it works in Maia | Priority |
|---------|------------------------|----------|
| Number lookup | TDS sends a number → Claw returns status instantly | MVP Day 1 |
| Client file (basic) | Created in 10 seconds: phone + name/pseudo + channel | MVP Day 1 |
| Personal blacklist | "Blacklist this number" → status set with mandatory reason + date | MVP Day 1 |
| Auto-alert on blacklisted contact | TDS checks a number → Claw warns if blacklisted | MVP Day 1 |
| Check-in / safety signal | "I'm going to [location]" → Claw notes it. "I'm safe" → Claw logs return | MVP |

### Bloc 2 - Framework & Interactions

*Coverage: Switchboard (partial)*
*Evidence: P8 Silver (manual repetition), P4 Gold (cognitive overload)*
*Co-founder: "Classement et archivage des messages inappropriés, raccourcis décisionnels"*

| Feature | How it works in Maia | Priority |
|---------|------------------------|----------|
| Predefined decision rules | TDS configures: "If no-show 2x → auto-tag unreliable" | Phase 1 |
| Structured responses | "Send my rates" → Claw provides the TDS's pre-written template | Phase 1 |
| Inappropriate message logging | "Log this as inappropriate" → archived with timestamp | Phase 1 |

### Bloc 3 - Professional Organization

*Coverage: Memory + Booking*
*Evidence: P2 Gold, P11 Bronze*
*Co-founder: "Agenda avec rappels et notes de RDV personnalisées"*

| Feature | How it works in Maia | Priority |
|---------|------------------------|----------|
| Appointment logging | "Meeting with [name] tomorrow at 14h at [location]" → Claw records | Phase 1 |
| Reminders | Claw reminds before appointments (configurable: 1h, 2h, day before) | Phase 1 |
| No-show tracking | "He didn't show" → logged on client file, tag updated | Phase 1 |
| Client history | "What do I know about [name]?" → full timeline | MVP |
| Intention notes | Auto-surface at each new appointment: "Paiement au début", "Tenir ferme sur les tarifs" | Phase 1 |

### Bloc 4 - Transactions (Light)

*Coverage: Payments (partial), Compliance (partial)*
*Evidence: P16 Silver (from INT8 - GABRIELLE built her own tracker)*
*Co-founder: "Suivi simple des entrées liées aux RDV"*

| Feature | How it works in Maia | Priority |
|---------|------------------------|----------|
| Income logging | "Received 300 from [name]" → logged with date and contact | Phase 2 |
| Payment status on file | "Paid" / "Not paid" / "Deposit received" per appointment | Phase 2 |

### Bloc 5 - Analysis & Insights

*Coverage: Compliance*
*Evidence: P16 Silver (GABRIELLE's seasonality analysis)*
*Co-founder: "Revenus, patterns, fréquence des demandes - version 2"*

| Feature | How it works in Maia | Priority |
|---------|------------------------|----------|
| Revenue summary | "How much did I make this month?" → Claw totals from logged payments | Phase 3 |
| Seasonality insights | "How was March vs January?" → Claw compares | Phase 3 |
| Client frequency | "Who are my regulars?" → Claw lists by visit count | Phase 2 |

### Bloc 6 - Collective Blacklist

*Coverage: Reputation*
*Evidence: A1 Supported (strongest behavioral - GABRIELLE built a scraper). A9 Open (legal risk).*
*Co-founder: "Possibilité de contribuer volontairement. Aucune obligation, aucune exposition. Conditionné à la validation du MVP individuel."*

| Feature | How it works in Maia | Priority |
|---------|------------------------|----------|
| Opt-in contribution | "Share this blacklist entry" → anonymized, contributed to network | Phase 2 |
| Network alerts | When checking a number: "3 other users flagged this number" | Phase 2 |
| No automatic sharing | Nothing is shared without explicit, per-entry consent | Design principle |

**Legal prerequisite:** Swiss counsel review on defamation risk, nFADP compliance, and data architecture (federated vs. centralized) before any collective feature is built.

**Competitive context:** ProCoRe (umbrella of 27 Swiss counseling centers) is building a national "Bad Client List" for 2027 -- web-based, criminal offenses only, free. Projet Jasmine (France) already operates a searchable 8-tier alert database. NUM (UK) processes ~1M alerts/year. P411 (US) charges ~$100-150/yr for mutual verification. No existing tool is conversational, AI-augmented, or combines collective data with personal CRM. See `docs/safety-lookup-landscape.md` for full analysis.

### Bloc 7 - Resources

*Coverage: Identity (partial)*
*Evidence: No direct interview signal - co-founder insight*
*Co-founder: "Un canal d'information fiable et centralisé"*

| Feature | How it works in Maia | Priority |
|---------|------------------------|----------|
| Partner org info | "How do I contact Aspasie?" → Claw provides contact details | Phase 1 |
| Practical guides | "What are my rights in Geneva?" → Claw links to Aspasie guide | Phase 1 |
| Sector alerts | Periodic safety alerts pushed through Maia (if subscribed) | Phase 2 |

---

## Client File Schema

*Integrated from co-founder's spec (Section 5 of her brief). This is the data model for the Memory coverage.*

### Created in 10 seconds (minimum viable record)
- Phone number (E.164 normalized - primary key)
- Name or pseudo
- Contact channel (WhatsApp / Telegram / Signal / email / phone / other)

### Enriched over time (the TDS adds as they learn)

**Identity & Profile**
- Photo of the contact
- Language(s)
- City / location
- Referral source: via which ad, via which colleague
- ID verification status: seen or not
- Colleague who knows them and can confirm

**Financial Behavior**
- Agreed rate
- Deposit requested: yes/no, honored: yes/no

**Behavior & Safety**
- Status: Safe / To verify / Caution / Blacklisted
  - Blacklist requires mandatory reason + date
- Behavioral tags (multiple): no-show, negotiates rates, inappropriate requests, aggressive, reliable, respectful, generous, discreet, etc.
- Boundaries set: what was refused and why (trace of limits)
- Incidents: physical or verbal, during appointments

**Notes & Memory**
- Intention note (auto-displays at each new appointment with this contact)
  - Examples: "Paiement au début", "Transport à facturer en extra", "A déjà négocié - tenir ferme"
- Free timestamped notes (unlimited)

**Appointment History**
- Date, time, location
- Present or not (no-show recorded)
- Paid or not (amount if logged)
- Post-appointment note
- Configurable reminder (1h, 2h, day before)

---

## Onboarding Flow

*Conversational, not form-based. Maia guides the TDS through setup in their messaging app.*

### Step 1 - Welcome
Maia introduces itself. Explains: "I'm your personal assistant. I help you track clients, check safety, and stay organized. I don't read your messages. You tell me what you need."

### Step 2 - Channels
"Where do your clients contact you?" → TDS says WhatsApp, Telegram, calls, etc. → Claw notes this for context.

### Step 3 - Base Rules
"Here are some common rules other TDS use. Which ones would you like to activate?"
- Auto-tag anyone who cancels twice as unreliable
- Remind me 2 hours before every appointment
- Always ask for deposit for outcalls
- Flag numbers that aren't in my contacts

TDS activates or deactivates. All optional.

### Step 4 - First Contact
"Want to add an existing client? Send me a number and a name, and I'll create their file."

Or: "You're all set. Next time someone contacts you, send me their number and I'll check it for you."

---

## What We're Testing in Beta

The beta is not testing features. It's testing hypotheses.

| Hypothesis | How we test | Success signal |
|------------|------------|----------------|
| A4: TDS prefer conversational interface over a separate app | Do they use Maia, or ask for "an app"? | Usage frequency, unsolicited feature requests |
| A7: Semi-automatic is acceptable | Do they trust Maia's information? Do they want it to do more or less? | Feedback, override rate |
| P3: Number lookup reduces screening friction | Do they check numbers before meetings? | Lookup count per user per week |
| P2: Client files reduce memory load | Do they add notes? Do they consult files before repeat clients? | Note creation rate, file consultation rate |
| P5: Personal blacklist is used | Do they blacklist anyone? How often? | Blacklist entries per user |
| Pricing: Will they pay CHF 15-55/mo? | Introduce pricing after 4 weeks of free use | Conversion rate, churn |

---

## Phasing

### Phase 1 - Beta Pilot (Month 0–6)
- Bloc 1 (Safety) + Bloc 3 (Organization) + Bloc 7 (Resources)
- 10 beta users in Geneva via co-founder network
- Conversational interface only, no UI
- Data stored on-device or minimal Swiss-hosted backend
- Success: users come back daily without being prompted

### Phase 2 - Validation (Month 6–12)
- Add Bloc 2 (Framework) + Bloc 4 (Transactions light)
- Expand to 50 users in FR-CH
- Test pricing (CHF 15–35/mo)
- Begin collective blacklist design (legal review first)
- Evaluate: do we need a UI? What do users ask for that conversation can't handle?

### Phase 3 - Growth (Month 12–18)
- Add Bloc 6 (Collective blacklist) if legally cleared
- Add Bloc 5 (Analysis) if there's demand
- Expand to full Switzerland
- Introduce UI only if beta feedback demands it
- Begin second vertical exploration (Soigneur coverage pack)

---

## Mapping: Co-founder's Blocs → Claw Coverages

| Co-founder Bloc | Claw Coverage | Evidence Tier | Phase |
|----------------|---------------|---------------|-------|
| Bloc 1 - Safety | Safety + Memory | Gold | MVP |
| Bloc 2 - Framework | Switchboard | Silver | Phase 1 |
| Bloc 3 - Organization | Memory + Booking | Gold + Bronze | Phase 1 |
| Bloc 4 - Transactions | Payments + Compliance | Silver | Phase 2 |
| Bloc 5 - Analysis | Compliance | Silver | Phase 3 |
| Bloc 6 - Collective | Reputation | Supported (A1) but legally unresolved (A9) | Phase 2–3 |
| Bloc 7 - Resources | Identity (partial) | Co-founder insight | Phase 1 |

---

## Open Design Questions (to resolve through beta)

1. **How does the TDS "forward" a number to Maia?** In WhatsApp, they could share a contact card, type the number, or forward a message. Which is most natural?
2. **Where does data live?** On-device (strongest privacy) or Swiss-hosted backend (enables multi-device, collective features)? Start on-device, migrate if needed.
3. **What triggers Maia proactively?** Appointment reminders are clearly wanted. But should Maia ever initiate a message unprompted? ("You have a meeting in 2 hours with someone who has a caution flag.")
4. **How do we handle the admin chat vs. client-facing messages?** The POC architecture proposes a private admin chat within WhatsApp. Is this the right UX, or should Maia be a separate WhatsApp contact?
5. **Multilingual from day 1?** FR-CH launch suggests French default, but many TDS (and their clients) operate in multiple languages. GABRIELLE's WAB templates are already multilingual.

---

*"Conçu à partir du vécu, pas d'une liste de features."*
*- Co-founder's product brief, February 2026*

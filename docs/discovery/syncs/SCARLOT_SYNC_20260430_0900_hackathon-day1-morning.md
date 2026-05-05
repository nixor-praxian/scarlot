---
date: 2026-04-30
participants: [Philippe, Andrew]
duration: ~3h 23min
type: working session
tags: [hackathon, poc, architecture, whatsapp, baileys, trojan-horse, scraper, esim, voice]
---

## Narrative

The session opened with Philippe walking Andrew through a complete map of the user-flow surface that Scarlot must eventually cover, drawn live in FigJam. The structure he laid out has eight conceptual layers stacked from "first contact" to "post-booking": filtering of inbound, lookup against a known-number database, capture of practical information, language selection, availability exchange, capabilities advertisement, calendar/contact integration, and onboarding. The map matters more for what it forces explicit than for any single feature: it surfaces every place where the agent has to decide between asking the user for input, inferring from context, or deferring to the user's own decision.

Within that frame the conversation focused on filtering. Philippe argued that filtering only works cleanly when the agent has the inbound number in advance, and that absent that, the next best mechanism is user-driven: the user forwards an unwanted message, marks it unwanted, and the agent stores the record. Andrew pushed further: the agent could also reply automatically to such messages once a pattern is established. They both flagged the tension this introduces. Reading user messages by default raises a "creepy barometer" question that the team cannot answer in the abstract; it has to be tested with users. They agreed that the prototype must support both modes (forward-driven and stream-driven) to let the cohort tell them which is acceptable.

The legal layer surfaced naturally. Philippe noted that under the Swiss data protection law (formally LPD, the federal counterpart to GDPR), Scarlot would act as a sub-processor for the data that flows through its agent. Andrew asked the awkward question: how would a "right to be forgotten" actually work between a TDS and a one-night client whose phone number sits in the agent's lookup index. They parked the question rather than resolving it, and Philippe noted it as a top open question for legal counsel.

Andrew then demonstrated his prototype, built with Bailey's (an open-source WhatsApp Web library). The prototype already supports tool calls like search_client, lookup_client, queue_client, add_note, and a payment-tracking flow. The demo worked end-to-end: Andrew sent himself a fake message, the agent created a contact record, attached a note, recorded a 300 CHF payment, and answered a "last week payments" query. The cost of the API calls during the demo was around 1.50 USD. They discussed using Open Router rather than direct Anthropic API access for vendor independence and cheaper experimentation across models; this would also let them test Gemini Flash for voice-to-text and other low-cost hops.

The conversation then hit the technical wall that determined everything that followed. Bailey's exposes a JID (a WhatsApp identifier) for each contact, but does not expose the underlying phone number. The phone number is the unique key for blacklist matching across other platforms, and without it the lookup cannot be performed against a third-party database. Forwarding a message also strips the source attribution, so the agent receives the content but not the originating identity. The team examined three workarounds. First, ask the TDS to share the contact card explicitly; this works but adds friction. Second, give the agent passive access to the full message stream and reconcile JID to phone via content matching when the user later identifies the contact; this works but requires reading messages from non-relevant senders, which is the privacy concern. Third, provision a separate phone number to each TDS and sit in front of inbound traffic; this removes the JID problem entirely but adds onboarding friction and potentially conflicts with how TDS already advertise their existing number.

Philippe then surfaced the idea that became the central architectural decision of the session: a Trojan Horse reverse-lookup scraper. Joséphine had told him the previous evening that she could provide login credentials to each of the major platforms that already aggregate flagged numbers (And6, CheckClient via BMG, ProCoRe survey data, and so on). Andrew immediately saw the implication: an agent that scrapes these platforms continuously, normalises the data, and exposes a single phone-number-keyed API would directly solve the highest-priority problem on the validated stack, P5 (no centralised blacklist). The team would become the de-facto reference once they had aggregated enough sources, and the willingness-to-pay test would be cleaner than for any other feature because the value is immediate and the user already does this work manually today. Philippe sent a message to Joséphine during the session asking for one platform login to start with.

Once the Trojan Horse was on the table, the rest of the session was about how to split the work and what the contract between the two halves of the system should be. Andrew would continue on the WhatsApp connector, the agent loop, and the user experience surface. Philippe would build the scraper, the data model, the normalisation layer, and the lookup API as a separate service. The two services would live in distinct repositories (Andrew's primary scarlot-poc and Philippe's new scarlot-safety-data, registered as a submodule), communicate over a clean HTTP contract, and be containerised separately so the agent never has direct access to the safety data and the safety service never sees the agent's user data. Andrew called this out as a useful security property in addition to a practical one: the safety service does not know which TDS is asking about which number.

The contract itself was negotiated in the back half of the session. Input: a phone number. Output: status (clean, greylist, blacklist), category tags, report count, first reported date, last reported date. Andrew preferred a category-only response in V1 because it requires no LLM-side judgement on his end. Philippe agreed, with the understanding that he would internally aggregate raw reports into categories using clustering and use Alison's DGX Spark for the model runs. Free-text comments from scraped reports would not be passed through; only structured tags. They explicitly agreed that this read-only contract was V1, and that V2 would add a write API allowing a TDS to submit new reports back to the safety database.

In parallel, Philippe kicked off a deep research run on WhatsApp Business capabilities. The headline conclusions Andrew flagged for follow-up: consumer WhatsApp has no API and never will (only Bailey's-style reverse-engineered access); the WhatsApp Business app has no programmatic access either; the WhatsApp Business Platform Cloud API requires Meta hosting, payment, and identity verification, with a 24-hour template rule for outbound messages. The implication for Scarlot is that they cannot build a clean Meta-blessed product in V1; they will live on Bailey's and accept the constraints. Andrew also raised Stalwart, a Rust-based AGPL CardDAV/CalDAV server, as a possible foundation for the contact-and-calendar synchronisation Philippe wanted; they decided not to bundle it but to keep the protocols (CardDAV, CalDAV) as the integration surface.

Phone-number provisioning came up again toward the end. Philippe walked Andrew through Republik, a Swiss eSIM provider that took five minutes to onboard, did the KYC themselves, and offered numbers at around 13 CHF per month. They both bought numbers during the session. The architectural implication: if Scarlot ever needed to give a TDS a Scarlot-provisioned number, the carrier handles KYC, removing one of the legal layers. This sits in tension with the current Bailey's-based approach, which uses the TDS's own number, but it provides a credible second path.

Voice processing was added as a stretch goal for the hackathon: speech-to-text via Gemini Flash through Open Router, with the ability to ingest WhatsApp voice notes and respond to them. Philippe noted that this was both a beta requirement (because TDS use voice notes heavily, especially the Spanish-speaking and lower-literacy segments Joséphine had emphasised) and a generalisable feature for many other verticals.

The session closed with Philippe scaffolding the scarlot-safety-data repo, registering it as a Git submodule in scarlot-poc, and producing an initial spec and plan with Claude. Both moved into independent build mode for the rest of the day.

## Decisions

**Build a Trojan Horse reverse-lookup scraper as the V1 anchor feature.**
The scraper aggregates flagged-number data from each major platform Joséphine has access to (And6 first, then CheckClient/BMG, ProCoRe data, and others), normalises it into a phone-keyed schema with categorical tags and report counts, and exposes a read-only HTTP lookup API. This solves P5 (no centralised blacklist), the highest-priority problem on the validated stack, and gives the POC a feature that delivers immediate value to a single user without depending on network effects. Alternatives considered: build only the local CRM and defer reputation; rely on direct Bailey's stream filtering. Both rejected because the lookup is the strongest willingness-to-pay test the team can run in beta.
Confidence: CERTAIN. Revisit if legal counsel forbids the scraping approach (anti-scraping risk in EU was discussed and considered low for compilations).

**Architecture split into two services with a clean API contract.**
scarlot-poc hosts the WhatsApp connector, agent loop, and user-facing experience. scarlot-safety-data hosts the scrapers, the unified data model, and the lookup API. The two communicate only over the contract: phone number in, status plus category plus counts plus first/last reported out. The safety service is read-only in V1; a write API arrives in V2 to let a TDS submit a new report. The two services are separate repositories, registered as Git submodules, with separate Docker containers and separate hosting concerns. Alternatives considered: a single monolith. Rejected because separation enforces a useful information-flow boundary (the safety service does not know which TDS is asking, the agent service does not have raw report data) and lets each side iterate independently.
Confidence: CERTAIN.

**Use Bailey's for V1 WhatsApp connection, accept its limitations.**
Consumer WhatsApp has no API. The WhatsApp Business Platform Cloud API requires Meta hosting, paid verification, and a 24-hour template rule that breaks the conversational pattern Scarlot needs. Bailey's is reverse-engineered, fragile, and the JID-not-phone limitation is real. The team accepts these tradeoffs for V1 because there is no better option, and because the architecture is channel-agnostic enough that a later migration to a more durable transport (Signal, Telegram, dedicated number with Twilio, eSIM) is feasible.
Confidence: PROBABLE. Revisit if Bailey's becomes structurally unstable or if Meta's stance on multi-device session tokens hardens further.

**Use Open Router rather than direct Anthropic API access.**
Open Router gives them vendor independence, cheaper experimentation across models, and a single integration surface that supports Claude (for reasoning), Gemini Flash (for voice transcription and high-volume cheap calls), and others. The cost of running the demo session directly on Anthropic was about 1.50 USD; Open Router is materially cheaper for the same workload. Alternatives considered: direct Anthropic, direct OpenAI. Rejected because vendor lock-in is a real risk during the experimentation phase.
Confidence: CERTAIN for V1.

**JID-to-phone reconciliation requires user action; both forward-mode and stream-mode prototyped.**
The Bailey's library does not expose the phone number behind a JID. There are two ways to reconcile. The user can share a contact card explicitly (forward-mode), which is friction but clean. Or the agent can passively receive the full message stream and match content when the user later identifies the contact (stream-mode), which is less friction but more intrusive. The team will prototype both and let the beta cohort tell them which is acceptable.
Confidence: PROBABLE. The acceptable-mode decision is empirical and will come from beta.

**Reports are tag-based with categories, no free text exposed in V1.**
The lookup response carries status, category tags, report counts, and first/last reported dates. Free-text comments from scraped sources are aggregated internally (clustering on the DGX Spark using Alison's tooling) but not surfaced in the response. This protects the team legally (no defamation surface), and aligns with how TDS already operate (instinctive blocking, not detailed reasoning). 
Confidence: CERTAIN for V1.

**Stretch goal: voice-to-text via Gemini Flash through Open Router.**
TDS use voice notes heavily. The agent must accept and respond to voice notes from V1 if at all possible. Gemini Flash is cheap enough and fast enough to make this viable on the day-one cost curve.
Confidence: PROBABLE. Will ship if the API integration time stays under a few hours.

**Provision a Scarlot phone number per TDS via Republik (Swiss eSIM) for the alternative architecture path.**
Both founders provisioned numbers during the session at around 13 CHF per month. The carrier handles KYC. This is not the V1 path but it is the credible fallback if Bailey's becomes unworkable, and it is the natural foundation for a future "switchboard" architecture where Scarlot owns the inbound channel.
Confidence: PROBABLE.

## Context Shifts

**The Trojan Horse changes the willingness-to-pay test.**
Until this session, the willingness-to-pay test was abstract: would a TDS pay for a CRM, would she pay for inbound filtering, would she pay for booking help. The reverse-lookup scraper changes this. The lookup is a feature she already pays for fragments of (BMG subscription, CheckClient inside BMG, time spent in Telegram groups asking about numbers). A consolidated lookup at Scarlot pricing is a direct substitution test, not a hypothetical. This is the strongest willingness-to-pay test the team can run in beta.

**JID-not-phone reconciliation reframes the V1 user journey.**
The first user interaction in V1 cannot be "the agent reads your inbound and tells you who is dangerous". It has to be "you forward me a contact card and I tell you what I know about that number". This is more friction than the team had assumed but it is the only honest implementation given the Bailey's constraint. It also happens to map well onto how Joséphine's network already operates (paste a number into a Telegram group, get answers from peers).

**WhatsApp Business Platform is closed off for the relevant use case.**
The deep research clarified that the only Meta-blessed path requires hosting, payment, verification, and template constraints that break the product. The team will stay on Bailey's, accept the fragility, and design the channel-abstraction layer carefully so a future migration is possible without rewriting the agent.

## Action Items

- [ ] Build the And6 scraper as the first source for scarlot-safety-data; write the spec and plan in docs/poc/specs/safety-scraper/ before coding — Philippe
- [ ] Wire the lookup tool call into the WhatsApp agent on the scarlot-poc side; mock the response shape until the safety service is live — Andrew
- [ ] Get a platform login from Joséphine to seed the first scrape (message sent during the session) — Joséphine, then Philippe
- [ ] Register scarlot-safety-data as a Git submodule of scarlot-poc — Philippe
- [ ] Switch the agent to Open Router for model access; first model to test is Claude Sonnet, then Gemini Flash for voice and cheap subagent calls — Andrew
- [ ] Add voice-note ingestion (Gemini Flash via Open Router) as a stretch goal — Andrew
- [ ] Pick a Swiss VPS host for the production-adjacent deployment (candidates discussed: Exoscale, Infomaniak, Postpoint, Init7); decide before the end of the hackathon — Both
- [ ] Document the channel-abstraction layer so V2 transports (Signal, Telegram, eSIM-backed number, Twilio) are pluggable — Andrew
- [ ] Write the V2 report-write API contract once V1 lookup is stable — Philippe

## To Think About

- The "creepy barometer" question. Stream-mode (the agent reads everything) is more powerful but possibly unacceptable. Forward-mode is safer but loses much of the value. The cohort answer is the only answer that matters; design the prototype so both modes can be tested per user.
- The blackmail surface. If the agent has stream access, a leak or a malicious operator could exfiltrate intimate conversations, and the journalistic story writes itself. The data-handling architecture must be defensible from day one, not retrofitted post-incident.
- Whether the safety database should ever expose source attribution for reports, even in a "request reveal from reporter" pattern. David will revisit this in the noon session, but the morning view was that exposing reporters at all is a non-starter for a community whose first principle is anonymity.
- Whether the Republik eSIM path is the right primary architecture, given the carrier handles KYC and the team avoids the JID problem entirely. Currently sidelined in favour of Bailey's for V1, but worth a serious comparison once the V1 friction is measured.
- Voice-first as the default modality rather than text-first. Joséphine's earlier "alpha and omega" framing keeps surfacing. The cost curve makes it credible now.

## Open Questions

- What is the Swiss legal status of compiling and serving the scraped reports? The team's working assumption from prior research is that database-rights protections in the EU do not bite for compilations of public reports, but this needs counsel.
- How does the right-to-be-forgotten between two private individuals (a TDS and a one-time client whose number lives in the lookup index) interact with the LPD?
- Is there any reliable way to map an iOS or macOS contact to its WhatsApp JID from the device side, so a returning contact already in the user's address book can be auto-recognised before the user shares a card? The morning session left this as a known unknown.
- Will Joséphine be able to provide platform logins fast enough to start the first scrape today or tomorrow?
- What is the fastest viable path to a working voice loop (Whisper via Gemini Flash, or some other transcription stack)? Is the round-trip latency acceptable for WhatsApp voice notes?
- Where should the scarlot-safety-data service be hosted? Spark for development is fine; production needs a Swiss VPS decision before beta.

## Key Quotes

> "Ce qui est sûr, c'est que c'est faisable techniquement. C'est du coup la question, c'est est-ce que c'est désirable d'un point de vue utilisateur."
> — Andrew, on stream-mode access to inbound messages

> "Si le pain que vous avez le plus soulevé et que les gens sont potentiellement prêts à payer pour ça, pour moi, je n'en aurais pas. On devrait essayer d'intégrer ce truc là."
> — Andrew, on the Trojan Horse decision

> "On va devenir la plateforme qui a les gets."
> — Andrew, framing the strategic upside of the reverse lookup

> "WhatsApp ne veut pas leak le numéro. C'est leur règle, c'est comme ça que ça fonctionne."
> — Andrew, summarising the Bailey's JID limitation

> "À un moment donné où on parlait d'éthique et de philosophie, le système il doit être plus éthique que nous-mêmes."
> — Philippe, on the data-handling posture for the safety service

## Connections

- **P5 (no centralised blacklist)** — the Trojan Horse decision is the direct architectural answer to P5. The session moves it from "future product surface" to "V1 anchor feature".
- **P3 (no inbound filtering) / P4 (cognitive overload)** — the JID-not-phone limitation forces a forward-driven UX in V1, which only partially addresses these. The full answer waits until either the eSIM path or the reveal-stream-mode path is validated.
- **P8 (manual repetition)** — the description-template feature (GFE / Domina / PSE personas) was sketched but not built; deferred to a later hackathon day.
- **A1 (collective blacklist contribution)** — V1 ships without contribution; only consumption. V2 adds the write path. Tests the consumption side first because it is the simpler willingness-to-pay test.
- **A4 (no-UI / conversational interface preferred)** — the V1 forward-driven UX is consistent with A4; the cohort feedback will tell whether the friction is acceptable.
- **A7 (AI triage acceptable with user control)** — both modes preserve user trigger; no automatic agent reply is in scope for V1.
- **A9 (collective blacklist legally defensible)** — the tag-only-no-free-text decision narrows the legal surface significantly. Counsel is still required pre-launch.
- **A10 (nFADP compliance achievable)** — the two-service architecture (safety service does not know who is asking) is a structural data-minimisation move that helps nFADP posture.
- **C-04 (inbound message triage)** — V1 scope is the lookup, not the triage. Triage waits for V2.

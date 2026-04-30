---
date: 2026-04-29
participants: [Philippe, Joséphine]
duration: ~3h 53min
type: worksession
tags: [poc, alignment, ux-design, switchboard, voice, b2b-agencies, pseudonyms, naming, framing, beta]
---

## Narrative

The session opened with explicit tension. Joséphine surfaced the question she had been sitting on for weeks: are we going or not going. Her framing was that she has been doing free work and speaking about a project that does not yet exist, and the cost of that is becoming visible in her network. Philippe agreed that the underlying question was real but reframed the answer: the team cannot incorporate, spend, or commit before validating the critical assumptions, and the only way to validate those assumptions is to put a POC into the hands of real users. He committed to no spend beyond what the POC strictly requires. Joséphine accepted the discipline and the reasoning. The opening tension dissolved into the rest of the session being constructive rather than evaluative.

Philippe walked through the funnel mechanics that matter: pitches per week, conversion rate per pitch, retention, erosion, and word-of-mouth multiplier. These are the numbers the POC must measure, and they make or break the unit economics regardless of the rest of the financial model. He gave a worked example: 6 CHF per user per month of infrastructure cost, 10 CHF revenue, leaves 4 CHF for everything else, including the entire commercial development effort, since the team will not be able to do paid web marketing for the TDS vertical. Joséphine did not push back; she registered the framing and asked clarifying questions on the vocabulary (erosion, capture rate).

A short tangent on naming surfaced. Joséphine said the codename "Maia" feels generic-tech-bro and triggered her negative pattern recognition (the "I am leaving low-tech and entering high-tech" cliché). Philippe agreed without resistance. Both will think. The platform-versus-coverage structure (Maia or replacement = the platform, Scarlot = the TDS coverage) was confirmed.

The conversation pivoted to TDS-friendly framing of the product. Joséphine's clearest contribution of the day: AI should not be the first word the user encounters. Lead with "tool" or "agent" or implicit phrasing; explicit AI naming should arrive only when it builds trust, not when it spooks. She named two distinct fear profiles in the community: low-tech unfamiliarity, and data sovereignty / leak / private-public separation concerns. Different people, different fears, different copy. Philippe formally delegated TDS-facing language and inclusive writing to Joséphine. She accepted.

A document review followed. The strategic vision document held up; counter-arguments needed sharpening on two fronts. First, "prostitution" stigmatises and was replaced by "sex work" wherever the document carries that connotation. Second, the FOSTA-SESTA flank was tightened: the product is a tool, not an ad/distribution platform, and that distinction must be explicit and defensible. Payment processing was deferred to V1+. The phrasing the team adopted: secure the engagement, do not process the prestation. Stablecoins, escrow, tipping flows are V2 considerations, currently out of scope.

Joséphine flagged a contact she had been holding: Karine Maradon, scientific lead at ProCoRe. She offered an exploratory call to learn what ProCoRe is actually building, framed as a discovery conversation rather than a project pitch. Philippe agreed with the discipline. The implicit hypothesis: ProCoRe is unlikely to ship a durable product, but they may eventually share a data layer, and the relationship is worth seeding now.

Philippe walked through the financial model. The dominant lines are co-founder salary and infrastructure (LLM tokens via OpenRouter, messaging via Twilio). The model's most fragile assumptions are message-per-user-per-month, input/output token cost, and per-message-routing logic. A reasoning model (Opus) is not needed for every query; a cheaper completion model (Sonnet or Haiku) is fine for many. Per-message routing by query complexity is a real lever. Pricing model in the current spreadsheet is monthly subscription, but Philippe flagged the trend toward pay-per-use across AI products. Re-examine post-beta. Joséphine's only correction was on baseline volume: the 20k registered TDS figure from a decade ago undercounts current activity because many work without registering, and "active at least once a year" is the more useful denominator.

Philippe replayed the Andrew sync from April 21. Andrew arrived independently at the WhatsApp-native thesis after reading the docs. Andrew's main concern is infrastructure economics: a naive VPS-per-user topology costs ~6 CHF/user/month and likely exceeds revenue per user; the correct topology is shared bare-metal with strong per-user isolation. The POC infrastructure is therefore not the production infrastructure, and the transition is a real engineering work item. The graylist + blacklist concept also lands here, anchored on JUSTYNA's INT10 behaviour: blacklist for the dangerous, graylist for the time-wasters who drain attention without being dangerous.

The session then entered its longest and most generative phase: a 90-minute UX design pass on the beta product, conducted live, sticky-note style. The team built the user-flow from QR-code onboarding through capabilities introduction, lookup, description exchange, availability exchange, and booking confirmation. Several decisions hardened. No automatic bot replies in MVP, the user always triggers manually. No bot access to client conversations in MVP, filtering is personal-first. Filtering rules cannot be set-in-stone; what is "ok" for one TDS is a red flag for another, so per-user keywords and examples are the model. The personal-versus-collective record split was articulated: people share bad experiences, keep good ones private, and most contacts will be neither (a third class, "neutral", was named). Joséphine added a hard requirement: the lookup feature is meaningless on day one if the database is empty, so a small dataset must be pre-seeded by the founders and trusted beta candidates pulling from BMG, M6, and Filénis-adjacent sources before launch.

In the final third of the session, three breakthrough ideas emerged that were not in the prior sync record. First, the switchboard concept: instead of a side-chat where the user converses with their agent, Scarlot issues a phone number that the TDS displays on her ads. The agent screens, filters, and surfaces only the actionable inbound. This solves the cognitive overload problem (P3, P4) at the channel level rather than the per-message level. Joséphine immediately offered herself as the first tester, before any external beta user, because the architecture difference matters and needs to be validated end-to-end with someone who can iterate without external pressure. Second, voice as a core modality, not an optional one. Joséphine: voice notes are alpha-and-omega for hispanophone and Latin-American users, and the lower-literacy segment of the cohort, and many TDS who simply hate typing. Test from beta. Third, B2B agencies as a plausible early market. Joséphine described Leila's Geneva agency, where a single standardiste handles many TDS via WhatsApp with a manual check-in / check-out / payment-confirmation rhythm. The same problems Scarlot is solving for solo independents exist at agency scale, manually, and could be productised. Adjacent: a recent assault incident in a Geneva agency where the agency was unprotective. The check-in / check-out + emergency button is a credible MVP+1 feature.

Philippe demoed the scarlot-interview skill: the workflow loads context, generates a structured per-interview record, and updates the priority-evolution ledger across sessions. Joséphine validated the pseudonym rule explicitly: the pseudonyms given are the ones the participants accepted, and the skill must not invent or substitute. (Already addressed in the skill update earlier this week.) The synchronisation question between Philippe's and Joséphine's Claude environments remains open; La Suite Numérique was floated as an open-source coordination layer. To be discussed with Andrew tomorrow.

The session closed with Joséphine surfacing a real-world example bank that anchors the abstract design work: Leila's WhatsApp-coordinated agency, the Berlin BDSM studio that used to run on Excel-on-website, the Studio Zürich (Eclatex) and similar independent practitioners hosted at lavish venues. The pattern is consistent: no agenda, contact directly, manual coordination. Underserved. She also flagged the festival circuit (Festiput in Toulouse in June, SNAP in Brussels) as a dissemination channel once the product is tangible, including possible sponsorship or workshops on site.

## Decisions

**The POC is the gate. No incorporation, no spend, no commitment until validated.**
The team will not incorporate the entity, hire, or run paid marketing before the POC has been deployed and tested with real users. Joséphine accepted the framing after the opening tension. Andrew's hackathon tomorrow sets up the infrastructure. Beta within the following weeks.
Confidence: CERTAIN. Revisit only if the POC reveals an existential infrastructure or unit-economics problem.

**Joséphine owns TDS-facing language, inclusive writing, and the trust-building copy.**
Philippe formally delegated. Joséphine accepted. This includes the rule "AI is not the first word", inclusive writing conventions, and the wording of value proposition surfaces. Philippe will not edit her output without her consent.
Confidence: CERTAIN.

**"Maia" is to be replaced as the platform name.**
Joséphine's reaction was visceral (generic tech-bro pattern). Philippe agreed without resistance. Both to think. No deadline.
Confidence: PROBABLE. Revisit if the team converges on a name or decides the question is parking-lot.

**"Prostitution" stigmatises; "sex work" is the operative term in copy.**
Applied across the strategic vision and counter-arguments documents. Stigma-language audit pass to be done.
Confidence: CERTAIN.

**Scarlot does not process the prestation. Phase-1 framing: secure the engagement.**
Payment processing in any form (escrow, tipping, stablecoins) is deferred to V2+. The current value proposition is filtering, screening, memory, coordination. This also reduces FOSTA-SESTA exposure.
Confidence: CERTAIN for V1. PROBABLE for V2 (escrow / stablecoins are interesting but unblocked work to do).

**Joséphine emails Karine Maradon (ProCoRe scientific lead) for an exploratory call.**
The call is framed as discovery, not pitch. Goal: understand what ProCoRe is actually building, whether they would share data, whether the relationship is worth investing in.
Confidence: PROBABLE. Revisit after the call.

**Beta = 8 motivated users, mixed CH / FR / UK / NL.**
Joséphine's network covers CH and parts of FR; Anna offers UK and NL contacts. Selection criterion: motivated to use the tool AND willing to give voice-note feedback. The pool is not yet finalised.
Confidence: PROBABLE.

**No automatic bot replies in MVP. Manual user trigger only.**
The agent never speaks for the user without explicit consent. This is a both an A7 alignment ("AI triage acceptable with user control") and a stigma-management decision (the bot must not say things the user did not authorise).
Confidence: CERTAIN for MVP.

**No bot access to client conversations in MVP. Filtering is personal-first.**
Filtering keywords and examples are per-user and locally derived. No "automatic detection of bad messages" in V1. Defer to V2 once trust is earned.
Confidence: CERTAIN for MVP.

**Joséphine seeds initial blacklist data before beta launch.**
A small pre-populated dataset is required for the lookup feature to be non-empty on day one. Sources: Joséphine's own records, Adèle (BMG access), M6, Filénis-adjacent contacts. The legal status of this seeding is to be reviewed.
Confidence: PROBABLE. Revisit when the seeding starts and the legal review is complete.

**Joséphine is the first switchboard tester, before any external beta user.**
The switchboard architecture (Scarlot-issued phone number on ads, agent screens inbound) is structurally different from the side-chat model. Joséphine validates it end-to-end before exposing to external users.
Confidence: PROBABLE. Revisit if the switchboard model proves unworkable in early testing or if the side-chat model dominates.

**The interview skill never invents pseudonyms. The interviewer must supply one.**
The pseudonyms participants accepted are the canonical ones. The skill update has been applied. Existing records remain as they are.
Confidence: CERTAIN.

## Context Shifts

**The switchboard concept moves the product from message-tool to channel-tool.**
Until this session, the working model was a side-chat where the user converses with their agent. The switchboard variant (Scarlot owns the inbound channel via an issued number) is a structurally different product with different value proposition, different unit economics (more messaging cost, but more retention), and different legal posture. The two models are not mutually exclusive but they are not the same product. The team will test both and choose. This is the most consequential conceptual shift of the session.

**Voice / multimodal moves from "future" to "MVP-adjacent".**
The prior assumption was a text-first interface with voice as a possible V2 feature. Joséphine's argument (hispanophone / Latin-American / lower-literacy users; many TDS hate typing) reframed voice as core. Cost implications: API calls for transcription and synthesis are higher than text-only. Worth it if the adoption uplift is real.

**B2B agencies become a plausible early market, not a V3 hypothesis.**
Until this session, agencies were treated as a future B2B vertical. Joséphine's Leila example revealed that agencies already do exactly the workflow the product is automating, manually, via WhatsApp. They are arguably better customers than solo independents (more volume, more pain, more willingness to pay). The team will not actively pursue agencies in the beta but will design the architecture to allow that pivot if signals point that way.

**FOSTA-SESTA framing tightens: tool not platform.**
The legal posture had been understood but not crisply phrased. The session produced the operative phrase: secure the engagement, do not process the prestation; we are a tool that you use, not a marketplace where transactions happen. This phrasing should be applied across all founder-facing communication.

## Action Items

- [ ] Run the hackathon with Andrew on 2026-04-30 / 05-01: deploy POC infrastructure for the first beta users — Philippe, Andrew
- [ ] Email Karine Maradon (ProCoRe) to schedule an exploratory call — Joséphine
- [ ] Take ownership of the inclusive language and TDS-friendly copy across strategic vision and counter-arguments — Joséphine
- [ ] Seed initial blacklist dataset (BMG via Adèle, M6, Filénis-adjacent sources) for beta lookup feature — Joséphine — before beta launch
- [ ] Set up switchboard test with Joséphine as first user before external beta — Philippe
- [ ] Find a replacement for the codename "Maia" — Both
- [ ] Add active-duration inference to the scraper (each platform has a different last-seen logic) — Philippe
- [ ] Apply "prostitution" → "sex work" pass across all founder-facing documents — Joséphine
- [ ] Resolve skill / interview synchronisation between Philippe's and Joséphine's Claude environments (La Suite Numérique candidate) — Philippe, with Andrew
- [ ] Recruit the 8 beta users (CH, FR via Joséphine; UK, NL via Anna) — Joséphine
- [ ] Document the switchboard vs side-chat decision criteria so the test produces a clear pick — Philippe

## To Think About

- The neutral record class. Most contacts will be neither flagged nor positively saved. The product needs a UX for handling the long tail of "I don't know if this person matters yet" without forcing a decision.
- Voice as default vs voice as opt-in. Defaulting to voice may exclude users who want text-only privacy; defaulting to text may leave voice-native users underserved. Test both UX paths in beta.
- The "executive assistant" mental model that emerged in the switchboard discussion. Reframing the agent as "a delegated human who knows your calendar, your contacts, who to interrupt for, who to deflect" may be more honest to the actual product than "AI agent" or "bot". This connects to Joséphine's "AI not the first word" rule.
- The pricing-model question (subscription vs pay-per-use) is parked but real. AI-product pricing is trending toward pay-per-use; the team's current model assumes subscription. Re-examine after beta produces real volume data.
- Festival circuit (Festiput in Toulouse, SNAP in Brussels) as a dissemination channel once the product is tangible. Sponsorship, atelier, on-site recruitment. Watchlist for late summer / autumn 2026.

## Open Questions

- What is the new platform name (Maia replacement)?
- What is ProCoRe actually building, and would they share a data layer with a private company?
- How is age verification handled in beta given FOSTA-SESTA proximity?
- What is the legal status of pre-seeding the blacklist dataset from BMG / M6 / Filénis-adjacent sources?
- Switchboard or side-chat: which model wins in early testing? What are the decision criteria?
- How do we qualify user type post-beta (solo TDS, agency, standardiste, salon)? Probably from signals, not declarations.
- What are the industry-specific funnel benchmarks (conversion, retention, erosion, WoM) we should expect to clear?
- How does Philippe and Joséphine's skill / interview synchronisation work in practice across two Claude environments?

## Key Quotes

> "Moi ça continue à être encore une fois du travail gratuit et des choses que j'avance et ça continue à être parlé d'un projet à des personnes sur un truc qui en fait continue à ne pas avancer."
> — Joséphine, opening tension

> "Pour moi on ne peut pas incorporer la société tant qu'on n'a pas validé les aspects critiques du business plan et pour ça il faut que les gens testent."
> — Philippe, framing the POC as the gate

> "Dès qu'il y a un truc un peu techytek, c'est Maya. C'est Maya. Tu sais ? Genre ils sont un peu là genre je sors du low tech, je passe high tech et c'est Maya."
> — Joséphine, on the codename

> "L'idée c'est pas forcément de ne pas le dire et tout, mais c'est de pas être le premier mot."
> — Joséphine, on naming AI in TDS-facing copy

> "I hand it over to you with pleasure."
> — Philippe, delegating inclusive-language ownership

> "Le voice, c'est genre l'alpha et l'oméga."
> — Joséphine, on voice notes as a core modality

> "Il faudrait qu'on ait quand même une petite base de données nous qu'on ait un peu pirater avant pour que ça make sense."
> — Joséphine, on seeding the blacklist before beta

> "On peut faire ça avec moi si on veut. Je suis OK d'aller retourner à la mine une ou deux fois pour faire des tests."
> — Joséphine, offering to be the first switchboard tester

> "On pourrait être surpris par que c'est le produit qu'on va faire pour les agences."
> — Philippe, on B2B agencies as a plausible early market

## Connections

- **P3 (no inbound filtering) / P4 (cognitive overload)** — the switchboard concept is the strongest structural answer yet to these two priority problems. It moves filtering from per-message effort to channel-level architecture.
- **P5 (no centralised blacklist)** — the seeding decision and the lookup UX flow concretise how Scarlot delivers blacklist value on day one without waiting for organic data accumulation.
- **A4 (no-UI / conversational interface preferred)** — the side-chat model assumes A4. The switchboard model questions it: maybe users want LESS conversation, not more. Test will inform.
- **A7 (AI triage acceptable with user control)** — the decision "no automatic bot replies in MVP, manual trigger only" is the strongest A7 commitment to date. It also matches Joséphine's "AI not the first word" framing.
- **A8 (willingness to pay)** — the subscription-vs-pay-per-use question now has a structural decision pending; the model assumes subscription, the trend points elsewhere.
- **A9 (collective blacklist legally defensible)** — pre-seeding the dataset from BMG / M6 / Filénis-adjacent sources adds a new legal-risk surface (data acquisition method) on top of the existing one (sharing).
- **INT8 GABRIELLE / INT10 JUSTYNA** — the graylist + blacklist two-tier model and the "neutral record" third class trace directly to JUSTYNA's blocking behaviour and GABRIELLE's mood-dependent reporting pattern.
- **SCARLOT_SYNC_20260421 (Andrew)** — this session imports the bare-metal infra, the graylist + blacklist two-tier, and the JUSTYNA reference from the Andrew sync. It exports back to Andrew's hackathon the switchboard concept and the decision tree on side-chat vs switchboard.
- **Coverage checklist (skill ref)** — the per-user filtering keywords / examples in the UX flow correspond to the F.7-F.8 dimensions of the coverage checklist (ideal tool capabilities and exclusions).

---
date: 2026-03-11
participants: [Joséphine, Philippe]
duration: ~80 min
type: strategy session
tags: [architecture, partnership, equity, conversational-ux, carddav, caldav blacklist, business-model]
---

## Narrative

The conversation opened with Philippe refining the interview questionnaire methodology. He proposed adding a structured "current stack" section to future interviews: smartphone details (OS, brand, model), number of phone lines (combined vs. separate personal/professional), messaging app usage with prevalence indicators (e.g., "100% WhatsApp"), recordkeeping habits (do they take notes, where, how, is it systematic), planning and scheduling tools (calendar type, paper vs. software), advertising platforms used (with costs), business profile (full-time/part-time/occasional), and practice domains (e.g., SM vs. traditional escort). Joséphine noted that practice type significantly shapes the pre-booking dynamic — SM practitioners, for instance, have much more legitimacy to request client identification upfront because consent negotiation is foundational to that domain. Philippe found this insight valuable, noting a personal conversation years ago about how the fetish community centers consent far more explicitly than mainstream relationships. They also discussed FinDom (financial domination) and the Money Pig sub-category — Philippe had watched a documentary about it, and Joséphine mentioned a friend getting into FinDom, noting it's "vraiment pas évident" as a practice. These sub-categories matter for the product because they shape client interaction patterns differently from traditional escort work.

The conversation then shifted to Philippe's core strategic thesis for the product direction. He laid out a detailed argument for why Scarlot should avoid building a traditional application (web or native) and instead pursue a purely conversational, messaging-based architecture — at least for the initial product. His reasoning chain was multi-layered:

First, **cost**: a solid web application runs 200,000–400,000 CHF. Native apps for both iOS and Android can reach 500,000–800,000 CHF with a proper team, because you're effectively building two frontends even if the backend is shared.

Second, **platform risk**: native apps are subject to App Store and Google Play review processes. Content policies could flag or remove an app in the sex work domain. Even routine bug fixes require resubmission, potentially leaving users with a broken app for one to two days — and users who encounter a broken product tend not to come back, especially if they're paying.

Third, and most fundamentally, **double entry persists even with a dedicated app**. A web or native interface is still a form system — first name, last name, fields to fill. Philippe argued that natural language can encode multiple pieces of structured information in a single sentence, and that current AI models are reliable enough to interpret user intent and translate it into system actions (updating contacts, creating calendar events, adding notes).

Philippe then introduced a technical concept that excited both of them: using **CardDAV and CalDAV** — open protocols that predate any specific platform — to synchronize client contacts and appointments directly to the TDS's phone. He used the analogy of subscribing to a "Swiss public holidays" calendar that auto-updates across devices and operating systems. The idea is that each TDS would subscribe to a personal contact database and calendar feed maintained by Scarlot, so that client records and appointments appear natively in their phone's contacts and calendar apps. If they change phones, switch OS, or travel, the data follows them without manual transfer.

Philippe sketched a wireframe on paper to illustrate the flow: a client sends a first message to the TDS's WhatsApp → a parallel Scarlot admin chat surfaces context ("Did I already interact with this person?") → if the contact database isn't synced, prompts to resubscribe → when a booking is confirmed ("April 12, 2pm"), the TDS can say "create event" and it appears in their native calendar. He also walked through scenarios like a client messaging from a new number — instead of switching apps to edit contacts, the TDS says "Is this Mike?" in the admin chat, selects from options, and the contact is updated.

Joséphine initially questioned whether this would still involve "double entry" (needing to interact with the Scarlot chat alongside the client chat), but Philippe explained that the interaction would vary by messaging platform — Telegram, for example, supports inline buttons that eliminate typing. The key insight: **conversational UI collapses multiple form-field operations into natural language exchanges**. A sentence like "Mike cancelled, reschedule to Thursday 3pm" could simultaneously delete a calendar event, create a new one, and add a note to the contact record.

Philippe was explicit about uncertainty: "Don't fixate on what I'm saying now — this needs feasibility testing." He emphasized the iterative approach — validate each functional slice quickly, and if the messaging-only model doesn't work, pivot to a web interface. The synchronization mechanism (CardDAV/CalDAV) works regardless of whether the interface is conversational or web-based.

They then discussed data storage implications. Philippe noted that even temporary data processing (e.g., holding message content for 30 minutes to enable automatic triage) falls under privacy regulation. He introduced PII (Personally Identifiable Information) concepts: you don't need a full identity to be identifiable, and once you store PII in any form, you're subject to GDPR (and Switzerland's equivalent, the nFADP). He walked through the three core GDPR rights — right of information, right of refusal, and right to be forgotten — with significant financial penalties for non-compliance. His position: this is a real issue but not a blocker for the current phase. It needs professional legal review before building shared data features.

The **blacklist** discussion was one of the most substantive exchanges. Philippe positioned personal blacklist as the logical first step — it avoids legal complexity and delivers immediate value. Joséphine pushed back firmly, arguing that the collective blacklist is "front and center" and "vraiment la base." She showed Philippe the Butterfly Telegram group (105 members, 6 online, moderated by a community member through Aspasie's network) where TDS actively share blacklist information — contact cards, photos, voice recordings. She noted an equivalent group in Zurich that's "much better organized." Philippe acknowledged the centrality of the collective blacklist but maintained the phased approach: prove the messaging-based architecture works → build personal CRM (which creates the data model) → layer on the shared blacklist once legal and technical foundations are solid. Both agreed that personal CRM and blacklist share the same underlying data model, so building one enables the other.

Earlier in the conversation, Joséphine provided detail on platform economics. F-Girl operates on 5-day, 15-day, or monthly subscription tiers (~150 CHF/month, degressive pricing), with additional paid top-ups for visibility — "Hot Girls of the Day" rankings and similar premium placements. BMG has comparable paid visibility mechanics. These costs are part of the operational overhead TDS already carry, which is relevant context for Scarlot's pricing positioning.

They discussed BMG's existing "client check" system (accessible via checkclient.ch, behind a login). Philippe wanted to understand BMG's legal structure — their privacy policy, data collection practices, and terms of service — to learn how they've navigated the legal landscape. Joséphine had information from someone who works for BMG (Arnaud) and had already shared screenshots of BMG's reporting interface, including an AI-summarized "admiral report."

Joséphine reported that she had met with her lawyer that morning specifically about the blacklist concept. The lawyer, a tax specialist, said that as long as identification relies only on phone numbers without full identity, the legal risk is lower — but recommended consulting a colleague who specializes in tech startups and is currently on maternity leave. The lawyer did not raise red flags about the concept itself.

Philippe then raised the subject of a potential **technical advisor or founding member**: Andrew, a former CTO who sold a company to Booking (or a similar platform) and was most recently CTO at Technis. He's available, interested in the concept, and he and Philippe have wanted to work together for years. Andrew's excitement came particularly from the **generalization thesis**: if Scarlot nails conversational scheduling and CRM for TDS, the same architecture applies to all personal services (plumbers, coaches, therapists). Philippe was careful to frame this as an opportunity, not an imposition. Joséphine responded positively.

The conversation's final major thread was the **partnership and equity structure** — the first real discussion about co-founding. Philippe proposed a 55/45 or 60/40 split with a slight majority on his side, justified by his financial investment. Joséphine would be the public face. They discussed reserving approximately 10% from their combined shares for a potential technical founding member, with vesting protections. Philippe explained Employee Stock Option Plans (ESOP) for future hires and the concept of post-money option pools in fundraising rounds.

On **financial planning**, Philippe asked Joséphine to think about her living costs to model into a business plan. His approach: bootstrap with a light cost structure where Joséphine receives a salary (below target but livable) while Philippe doesn't take one initially. He explicitly warned against extended bootstrapping, citing his experience mentoring startups: when founders sacrifice quality of life for too long, their decision-making degrades. The goal would be to reach revenue milestones within roughly six months, then raise funding. He was frank about the European VC landscape: unlike the US where a good pitch and likeable founders can secure pre-revenue investment, European investors expect product, customers, and near-profitability.

Joséphine shared her own experience with being burned by a previous collaborator (Harick), who she worked with extensively without pay and who eventually told her to raise funds herself. Philippe related similar experiences and emphasized the importance of written agreements from the start — MOUs, signed documents, clear expectations. He proposed a **founder alignment workshop**: a full day together to define communication preferences (sync vs. async, phone vs. text), working style expectations, and dispute resolution. He also proposed weekly retrospectives at the start, tapering to monthly.

Near the end, Joséphine listed upcoming **user research activities**: an interview with Gabrielle the next day, a sex worker brunch in Basel organized by a CHDE collective (between Basel and Zurich) the following Tuesday, and a meeting with Avril the day after that. She also mentioned the **Aspasie cybersecurity workshop** she's creating with Meron (a TDS who moderates the Butterfly Telegram group), with the first session on April 28. Aspasie told her that if Scarlot has anything ready by then, they could present it — making this a potential early distribution channel. Philippe mentioned **viral waitlist** mechanics as a possible launch strategy and offered to help with the cybersecurity workshop content.

The conversation closed with mutual enthusiasm about working together. Joséphine offered her office space in Geneva for in-person work sessions. Philippe emphasized the value of co-location in early-stage projects where context changes rapidly and knowledge management is critical.

## Decisions

**Conversational-native architecture for POC — no standalone app or web frontend initially.**
The first product iteration will deliver functionality entirely through messaging (WhatsApp, potentially Telegram), using natural language interaction in a private admin chat. No web app, no native app. This avoids development costs (200K-800K CHF), app store platform risk, and preserves the "no double entry" principle. A web frontend will eventually be needed for platform administration, but not for the core user experience in V1. The data synchronization layer (CardDAV/CalDAV) works regardless of interface type, so pivoting to a web UI later doesn't waste the initial work.
Confidence: PROBABLE. Revisit if: user testing reveals that conversational UX is not desirable or sufficient — specifically if TDS consistently express preference for visual interfaces. Also revisit if feasibility testing shows CardDAV/CalDAV sync is unreliable across device types.

**Personal blacklist and CRM before shared/collective blacklist.**
The personal blacklist and CRM will be built first, deferring the shared blacklist to a subsequent phase. The shared blacklist introduces legal complexity (defamation, PII, nFADP), requires network effects to be valuable, and is technically more complex. However, personal CRM and personal blacklist share the same data model as the future shared blacklist — building one enables the other. Joséphine disagreed with the priority ordering, emphasizing that the collective blacklist is the feature TDS care about most, but accepted the phased approach as a development strategy.
Confidence: CERTAIN. Revisit if: legal review clears the shared blacklist with minimal risk, or if beta users reject a personal-only blacklist as insufficient motivation to adopt.

**Philippe and Joséphine to explore co-founding with ~55-60/45-40 equity split.**
Philippe proposed a slight majority for himself given his financial investment, with Joséphine as the public face. Both agreed to think further. They discussed reserving ~10% of combined shares for a potential technical founding member and using ESOP for future hires. No final terms were set — this was a first discussion to surface positions.
Confidence: POSSIBLE. Revisit if: either party's financial contribution or role changes significantly, or if a third co-founder with substantial contribution enters early.

**Bootstrap with Joséphine salaried, Philippe unsalaried initially.**
The cost structure should be as light as possible: Joséphine receives a salary (amount TBD based on her needs assessment), Philippe does not take a salary initially. Target: reach revenue milestones within ~6 months, then pursue fundraising. Philippe cautioned against extended bootstrapping based on experience with startups where founder sacrifice degrades decision quality.
Confidence: PROBABLE. Revisit if: timeline to revenue extends significantly, or if Philippe's capacity to work unpaid changes.

**Iterative "fail fast" development methodology.**
Every functional slice gets tested with users before building the next. Philippe told the cautionary tale of a 700,000 CHF equestrian ERP project that was scrapped after 12 months because the client refused iterative validation. The lesson: iterate, validate, correct course — never build for months in isolation.
Confidence: CERTAIN. This is a process decision, not a product decision.

## Context Shifts

**SM practice domain creates a different pre-booking dynamic.** Joséphine explained that TDS working in SM (BDSM/fetish) have significantly more legitimacy to request client identification before meeting, because consent negotiation is foundational to that domain. This means the product's screening and verification features may have different adoption barriers depending on practice type — SM practitioners may adopt immediately, while traditional escort practitioners may resist. This could inform onboarding and feature rollout strategy.

**Generalization to personal services as long-term thesis.** Philippe and Andrew (potential technical advisor) independently arrived at the same conclusion: the conversational scheduling + CRM architecture, if it works for TDS, applies to any independent personal service provider. This reframes Scarlot from a niche tool to a platform play, which has implications for fundraising narrative, technical architecture (abstraction level), and eventual market positioning.

**Aspasie cybersecurity workshop as distribution channel.** The April 28 workshop represents the first concrete, time-bound opportunity to present Scarlot to potential users in a trusted setting (organized by Aspasie, co-facilitated by a TDS community member). This shifts the beta timeline from abstract to anchored.

**Joséphine's financial constraints are real and immediate.** She has limited income (~40% role at Aspasie) and was burned financially by a previous collaboration (working extensively without pay). This is not just context for the business plan — it affects decision speed, risk tolerance, and the urgency of reaching either revenue or securing investment.

## Action Items

- [ ] Joséphine to assess her financial needs (monthly living costs bracket) for business plan modeling — Philippe
- [ ] Philippe to send Paul Graham article on co-founder dynamics and equity — Philippe
- [ ] Philippe to investigate CardDAV/CalDAV feasibility for contact and calendar sync — Philippe
- [ ] Philippe to look into BMG's privacy policy, terms of service, and data collection practices — Philippe
- [ ] Joséphine to ask Arnaud (works at BMG) about BMG's internal legal structure for client blacklist — Joséphine
- [ ] Joséphine to follow up with lawyer's tech-startup-specialist colleague when she returns from maternity leave — Joséphine
- [ ] Joséphine to share cybersecurity workshop draft with Philippe for feedback — Joséphine
- [ ] Philippe to share transcript and notes from this conversation — Philippe
- [ ] Joséphine to conduct interview with Gabrielle — Joséphine — March 12
- [ ] Joséphine to attend Basel sex worker brunch (CHDE collective) — Joséphine — March 17
- [ ] Joséphine to meet with Avril — Joséphine — March 19
- [ ] Philippe to brief Andrew on Scarlot at appropriate level — Philippe

## To Think About

- What is the right equity split? Joséphine asked to hear Philippe's thinking but both acknowledged more reflection is needed. The question of who contributes what (capital vs. community access vs. domain expertise vs. technical execution) will shape the answer.
- If the conversational-only UX proves insufficient, what is the minimum viable web interface? Philippe noted a frontend will eventually be needed at least for platform administration.
- How does practice type (SM, escort, GFE, occasional) affect which features matter most and in what order? The SM insight suggests different onboarding paths may be needed.
- Viral waitlist mechanics — could work given community network structure (Telegram groups, Aspasie workshops), but needs careful design to avoid feeling exploitative in a community that's deeply wary of being instrumentalized.
- At what point does Andrew's involvement formalize? Philippe positioned him as available and interested but didn't propose a specific role or timeline.

## Open Questions

- Can CardDAV/CalDAV reliably sync contacts and calendar events across iOS and Android from a server-side service? What are the edge cases (old phones, non-standard calendar apps, enterprise MDM restrictions)?
- What are BMG's legal terms for their client blacklist service? How do they handle GDPR/nFADP right-to-deletion and right-to-information requests?
- What does the nFADP classify as PII in the context of phone-number-only records? Does a phone number alone constitute PII requiring full compliance?
- How long can Scarlot hold message data for processing purposes (e.g., for automatic triage) before it constitutes "storage" under nFADP?
- What is the legal exposure for a platform that facilitates sharing client phone numbers between TDS, even without full identity? (Lawyer recommended waiting for the tech-startup specialist)
- Is the Aspasie cybersecurity workshop (April 28) a realistic deadline for having something demonstrable?

## Key Quotes

> "Il y a quand même une fatigue, au-delà de l'aspect sécurité tout ça, il y a ce truc de double saisie."
> — Philippe, identifying administrative friction as a core problem alongside safety

> "Dans une phrase, tu peux mettre plein d'informations. Alors que finalement, quand tu remplis, que ce soit un contact, un calendrier, ça reste des formulaires."
> — Philippe, articulating the natural language vs. form-based UX thesis

> "Ça reste quand même quelque chose [...] collectivement quand même. [...] C'est vraiment la base."
> — Joséphine, pushing back on deferring the collective blacklist — it's not a nice-to-have, it's foundational

> "On a travaillé pour une personne [...] 12 mois après, on s'est rendu compte que madame a dépensé 700 000 balles et qu'elle a mis la poubelle."
> — Philippe, on why iterative validation matters — the equestrian ERP cautionary tale

> "J'ai trop travaillé comme un malade pour les autres, j'ai envie de travailler comme un malade pour moi."
> — Philippe, on his motivation for co-founding rather than consulting

> "Moi j'ai mis trop, en fait j'ai bossé trop gratuitement pour Arik. J'ai été con."
> — Joséphine, on her previous experience that shapes her need for clear terms from the start

> "Il y a une étape où quand tu commences à faire du bootstrap pendant trop longtemps, ça a un impact négatif sur ta capacité décisionnelle."
> — Philippe, warning against extended bootstrapping based on mentoring experience

## Connections

- **P4 (mental load / cognitive overload)** — explicitly discussed as "double saisie" (double entry) fatigue beyond safety concerns
- **P5 (no centralised blacklist)** — extensive discussion of personal vs. shared blacklist sequencing; Joséphine showed the Butterfly Telegram group as evidence of active informal sharing
- **P7 (blacklist paywalled — BMG)** — BMG's client check system discussed; want to understand their legal framework
- **P8 (manual repetition)** — explicitly motivated the conversational UX thesis: natural language replaces multiple form inputs
- **P2 (no client memory)** — CRM as the data layer underlying both personal notes and future blacklist; contact sync via CardDAV
- **P11 (manual calendar / booking)** — CalDAV sync proposed as solution; "create event" from natural language in admin chat
- **C-01 (shared blacklist network)** — discussed as critical but deferred; personal blacklist first, shared later pending legal review
- **C-02 (conversational interface)** — central thesis of the conversation; Philippe's detailed argument for messaging-only UX
- **C-03 (client intelligence profile)** — contact cards with notes, synced via CardDAV, updated through natural language
- **C-04 (inbound message triage)** — wireframe walkthrough of automatic context surfacing on incoming messages
- **A4 (no-UI / conversational interface preferred)** — Philippe's hypothesis now articulated with detailed technical and economic reasoning; not yet user-validated
- **A9 (collective blacklist legally defensible under Swiss law)** — lawyer's preliminary read (lower risk with phone-only ID), pending specialist review
- **A10 (nFADP compliance achievable)** — PII concepts, GDPR rights, and data retention discussed explicitly; flagged as real but not blocking for current phase

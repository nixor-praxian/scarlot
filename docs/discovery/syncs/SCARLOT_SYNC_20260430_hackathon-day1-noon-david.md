---
date: 2026-04-30
participants: [Philippe, Andrew, David]
duration: ~1h 24min
type: strategy session
tags: [hackathon, framing, architecture, deployment-modes, v1-scope, legal, reputation, crm, ux]
---

## Narrative

David joined Philippe and Andrew at lunchtime as an external advisor and immediately reset the framing of the conversation. Philippe opened with "the problem is safety" and David interrupted: that is not where the description should start. He asked the founders to describe their company in three sentences as if they had walked into the room cold. The exchange that followed was Socratic by design. David made the team articulate, in order: who the customer is (the TDS, and possibly agencies), what relationship lifecycle Scarlot lives inside (Discovery, Messaging, Pre-Meeting, Meeting, Post-Meeting), which phases Scarlot covers (everything except Discovery and the Meeting itself), and what kind of channel Scarlot operates on (text and voice messaging, with WhatsApp as the dominant V1 transport).

The reframing surfaced a structural insight that the team had been carrying implicitly but not stating cleanly: Scarlot is not a channel, it is a layer on top of channels. Discovery (the platforms where TDS advertise) is upstream and out of scope. The Meeting itself is downstream and out of scope. Everything in between is messaging, and the messaging is where Scarlot helps. David also forced explicit articulation of the channel constraint: the Discovery platform sets the channel, the TDS does not, so Scarlot must take channel-agnosticism seriously and respect that WhatsApp will dominate V1 only because that is what the Discovery platforms route to.

David then did the work of unbundling what the team meant by "messaging help". He drew out four message types in the relationship lifecycle: information complement (services, schedule, pricing), location and practical info (address, code, floor), agreement on logistics, and post-meeting feedback. He confirmed that the location-and-practical-info exchange is unavoidable for independents (an agency can do it upstream; a solo cannot), which sets a hard constraint on the V1 scope.

He then asked the question that determined most of the rest of the session: how should Scarlot's architecture sit relative to the user's WhatsApp account? The team walked through three modes. Mode A is the Bailey's device-link approach Andrew had built that morning, where Scarlot connects as a paired device and sees the entire message stream. Mode B is the eSIM path Philippe had sketched, where Scarlot provisions a phone number per TDS and sits as a proxy in front of inbound traffic. Mode C, which David proposed and pushed for, is "Scarlot is just another WhatsApp contact". The TDS adds the Scarlot number to her contacts the same way she adds anyone else, and interacts with Scarlot by forwarding contacts, messages, and voice notes. Scarlot replies, but Scarlot never has access to the TDS's other conversations.

David's argument for Mode C had three legs. First, it has the smallest data exposure surface, which simplifies LPD posture and reduces the blackmail risk Philippe had raised in the morning. Second, it maps cleanly onto how TDS already work (forwarding numbers into Telegram groups for peer lookup) so it has a low cognitive ramp. Third, it lets the team focus the V1 build on the highest-value feature (reputation lookup) without taking on the engineering weight of building a stable inbound-stream listener. Andrew pushed back gently: stream access lets the agent do useful things (auto-detect bad messages, learn templates from real exchanges, train a classifier on the user's own corpus), and forward-driven UX loses all of that. David's response was that the V1 should not assume those features are valuable enough to justify the data exposure; let the cohort tell you. Mode A and Mode C should both be tested, but Mode C is the stronger V1 default.

The conversation then turned to what the V1 should actually do on the reputation side. David proposed a chatbot that takes a forwarded contact card and replies with a reputation answer. He pushed hard on the response format. Free-text reports should not be exposed. Use tags only, and ideally emojis only. The argument is twofold. Free-text creates defamation exposure and potentially nFADP-sensitive content surface. Tags compress the same signal into a form that the user actually wants (instinctive go/no-go, not a research project). Philippe agreed and noted that the existing scraped corpus has free text but the team can use clustering on the DGX Spark to derive a tag taxonomy from it, with Alison's help.

David then pushed on the geometry of the reputation database. Should reports be revealable? He sketched a credit-bureau pattern: if a TDS sees a flag on a number, she can request more details, and the system mediates a conversation with the original reporter. Philippe pushed back on this immediately. The TDS community's first principle is anonymity, and asking a reporter to identify herself in any way (even mediated) breaks that principle and will kill participation. The V1 will show the score, no reveal. The reveal pattern can be reconsidered if the basic system gets traction. David accepted the constraint and added another mitigation: geographic gating, where the lookup only returns reports from the same city as the requesting TDS. This reduces the scope of who can be flagged about whom, both as a UX simplification (a Geneva-based TDS does not care about Zurich-based reports) and as a legal hedge.

WhatsApp's JID-as-anonymous-identifier came back into the discussion, and David flipped it from "limitation" to "feature". The JID is unique within WhatsApp, not derivable from a phone number, and not reverse-mappable. If the TDS shares a contact card explicitly when she wants a lookup, she retains control over which numbers enter the safety database. This is privacy-preserving by construction. The team accepted this framing.

David then proposed the V1, V1.1, V2, V3 phasing that became the de-facto roadmap for the rest of the hackathon. V1 is the chatbot reputation lookup: forward a contact, get a tag-based reply. V1.1 adds the geographic gating and finer report-count granularity. V2 is the CRM (calendar, notes, contact storage) accessed through text and voice notes. V3 is communication assistance (templates, automation between client and TDS). The reputation product and the CRM product are conceptually separable and probably should be deployed as separate POCs because they have different dynamics. The reputation product is the door-opener (clear willingness-to-pay, single feature, single demo). The CRM is the upsell with much more engineering weight.

Architecturally, David made one strong recommendation: scope everything by user_id from day one. Scarlot is one process per WhatsApp tenant, with all entities (contacts, calendar, notes, payment records) scoped to the user identified by the inbound JID. The reputation database is the only globally-scoped store. The user_id abstraction is also future-proofing: when Scarlot eventually adds a web app or a different transport, the same data model works because user_id was never WhatsApp-specific.

He also made an architectural distinction that mattered for how the team thinks about the agent stack. Open Source agent platforms like OpenClaude and NanoClaude (and Andrew's own pi-based experiments) are general-purpose, auto-modifiable, and oriented toward a single technical user. Scarlot is none of those. It is a stable, scoped, locked-down product with no self-modification. Scarlot may be implemented on top of the same conceptual primitives, but the customer surface is closed. Andrew agreed and emphasised that his prototype already has this property: the agent has only the tools he explicitly defines, no access to the file system beyond a sandboxed workspace, no ability to install skills.

The texture of the V1 UX got specific in the closing third. David argued that the conversational fluff should disappear. Forward a contact, get an emoji. No "how can I help you today" mode. The user has decided to call Scarlot; she does not need onboarding scripts on every interaction. There is one exception: when the user is new, the bot should run a training mode that teaches the interaction patterns. After that, the steady state is minimal: contact in, emoji out. Voice notes are a first-class input from V1 ("annule-moi le rendez-vous de mardi 20h"); they map to the same intent space as text.

The session closed with David validating Andrew's prototype on screen. He noted that the training-mode versus steady-state distinction was the key UX call to make explicit, and that the team should generalise the architecture beyond TDS. Reducing administrative work for solo professionals is a much larger pattern (David mentioned the construction industry; Philippe mentioned an investment in a wine business). The TDS vertical is the entry point because the pain is most acute and the team has insider access, but the same engineering surface serves many other "solo practitioner with a phone and a calendar" verticals.

## Decisions

**Mode C is the V1 default deployment posture: Scarlot as an external WhatsApp contact.**
The TDS adds Scarlot's number to her contacts and interacts with the agent by forwarding messages and contact cards. Scarlot does not connect as a paired device, does not see the rest of her conversations, and does not require any provisioning of a Scarlot-issued number for her clients. This is the smallest data-exposure surface, the simplest LPD posture, and the lowest cognitive ramp for users who already forward numbers into Telegram groups for peer lookup. Mode A (Bailey's device link) and Mode B (provisioned Scarlot number per TDS) remain in the design space but are deferred. Alternatives considered: Mode A as default for richer agent context; Mode B for full filtering. Both rejected for V1 on the grounds that the data-exposure cost is unjustified before the team has measured what users actually need.
Confidence: PROBABLE. Revisit if beta cohort tells us forward-driven UX is too high-friction, or if Mode A enables a clearly more valuable experience that the cohort cannot live without.

**V1 reputation reports are tag-only, with no free text and no source attribution.**
The lookup response carries: status (clean / greylist / blacklist), category tags (drawn from clustering on the existing corpus), report count, first reported date, last reported date. No free-text comments, no reporter identity, no reveal pattern. Tags will be derived by clustering the scraped report corpus using the DGX Spark, with Alison consulted on the model choice. Alternatives considered: free-text comments preserved; credit-bureau-style reveal mechanism. Both rejected because they introduce defamation risk and break community anonymity. The reveal pattern can be reconsidered after V1 traction.
Confidence: CERTAIN for V1.

**V1 / V1.1 / V2 / V3 phasing.**
V1 = reputation lookup chatbot (forward a contact, get an emoji-tag reply). V1.1 = geographic gating, finer report counts, granularity controls. V2 = CRM with calendar, notes, contact storage, accessed through text and voice notes. V3 = communication assistance (templates, semi-automated client-side responses). The reputation product and the CRM are conceptually separable POCs and should be deployable independently. The reputation product is the door-opener; the CRM is the upsell.
Confidence: CERTAIN for V1 sequencing. PROBABLE for V2 / V3 in their current form.

**All entities are scoped by user_id from day one.**
Each user (identified by inbound JID in V1) owns their own contacts, calendar, notes, and payment records. The user_id abstraction is transport-agnostic so the same data model works when Scarlot eventually adds a web app or other channel. The reputation database is the only globally-scoped store, and it never holds user-identifying information about who asked.
Confidence: CERTAIN.

**Geographic gating on reputation lookups.**
A TDS in Geneva sees Geneva reports first and is gated from cross-canton lookups. The exact mechanism (city, canton, radius) is to be specified in V1.1. The justification is twofold: UX simplification (a TDS does not care about another city's reports) and a legal hedge (smaller exposure surface per query).
Confidence: PROBABLE. Revisit if cohort feedback says cross-city lookups are needed (e.g., for visiting clients).

**UX is minimalist in steady state, with a separate training mode for new users.**
After onboarding, the typical interaction is: forward contact, receive emoji response, done. No conversational fluff. The training mode teaches the interaction patterns to first-time users. This is both a UX call and a position on the LLM stack: use the LLM for flexibility, but the steady-state interaction is closer to algorithmic than conversational.
Confidence: PROBABLE.

**Voice notes are a first-class input from V1 if technically achievable.**
Voice-to-text via Gemini Flash through Open Router. Voice intent maps to the same intent space as text (forward, lookup, schedule, recall). Aligns with Joséphine's "voice is alpha and omega" framing from the prior worksession.
Confidence: PROBABLE.

**Open agent platforms (OpenClaude, NanoClaude) are conceptually adjacent but not the model for Scarlot.**
Scarlot is a stable, scoped, locked-down product. The agent has only the tools the team defines, no self-modification, no skill installation. Same primitives may be used internally, but the customer surface is closed.
Confidence: CERTAIN.

## Context Shifts

**The product is a layer on channels, not a channel.**
Until this session, the team was carrying both framings implicitly: sometimes "Scarlot is a phone number that filters inbound" (Mode B), sometimes "Scarlot is an agent that you talk to about your work" (Mode C). David forced the choice. Mode C is the V1 framing. The other modes are later experiments. This is the most consequential conceptual shift of the session.

**The reputation product and the CRM product are separable POCs.**
The team had been treating Scarlot as one product with multiple features. David's framing is that reputation and CRM have different dynamics (reputation is dirty data with clear willingness-to-pay; CRM is clean per-user data with much more engineering surface) and are better deployed as two POCs that share infrastructure. This affects roadmap, pricing, and the order in which to ship.

**The JID flips from limitation to feature.**
The morning session treated the JID-not-phone constraint as a workaround to design around. David reframed it: the JID is a privacy-preserving anonymous identifier that protects the user, and the contact-card-share UX is the right way to onboard a number into the safety database explicitly with the user's consent. This is a conceptual shift that simplifies many downstream design choices.

**TDS heterogeneity is structural; filtering rules are not standardisable.**
Mood-dependent blocking (Gabrielle's data point), persona-dependent communication (GFE versus Dom versus PSE), city-dependent norms, individual rule sets. The implication is that the V1 cannot ship with hard-coded filtering rules; it must ship with a scoring system whose thresholds are user-controlled. This was implicit before; David made it explicit and tied it to architectural decisions.

**Reducing administrative work for solo professionals is the larger pattern.**
The TDS vertical is the entry point. The same engineering serves construction, viticulture, healthcare adjacencies, and other "solo practitioner with a phone and a calendar" segments. This was the pre-existing platform vision but David's framing made it concrete and tied it to immediate engineering choices (user_id abstraction, transport-agnostic data model, no over-fitting to TDS-specific assumptions).

## Action Items

- [ ] Implement the V1 chatbot loop in Mode C: TDS forwards contact card, agent replies with tag-based reputation response — Andrew
- [ ] Cluster the scraped report corpus into a tag taxonomy using the DGX Spark; consult Alison on model choice — Philippe
- [ ] Design the geographic gating mechanism for V1.1 (city, canton, or radius) and document the legal rationale — Philippe
- [ ] Build a training-mode onboarding flow that teaches the new user how to interact with Scarlot, distinct from the steady-state minimalist UX — Andrew
- [ ] Add voice-note ingestion as a V1 stretch goal (Gemini Flash via Open Router) — Andrew
- [ ] Document the user_id scoping abstraction so the data model is transport-agnostic from day one — Andrew
- [ ] Validate the contact-card-share UX with the beta cohort before assuming it is the right ramp — Joséphine, then Philippe
- [ ] Specify the V2 CRM architecture and decide whether it ships as a separate Docker container or embedded in the same agent process — Andrew
- [ ] Write the brief that says explicitly "Scarlot is a layer on channels, not a channel" so the architectural decisions stay coherent across the team — Philippe

## To Think About

- The reveal mechanism. David's credit-bureau-style "request more from the reporter" pattern is elegant but conflicts with the community's anonymity-first principle. Park the question and revisit if and only if cohort feedback shows V1 is too information-poor.
- Whether Scarlot eventually offers a web app. David's framing assumes WhatsApp is V1 only and the data model should not assume it. But the question of whether a web app actually adds value (versus just being engineering weight) is open.
- Voice as a default modality versus voice as an opt-in. Defaults set norms. If Scarlot defaults to voice-out, it changes who self-selects into the cohort.
- The construction-industry, viticulture, and other vertical adjacencies David and Philippe both mentioned. None of them are V1 targets but the engineering decisions taken in V1 either help or hurt those expansions. user_id abstraction helps. Hard-coded TDS-specific tag taxonomies hurt.
- Whether the reputation database should ever expose city-level statistics in the response (a la "this number was reported 5 times in Geneva, 3 in Lausanne") or whether geographic gating means city-level stats are also gated. UX call, not yet decided.

## Open Questions

- What is the right tag taxonomy for the reputation response? Derived how, validated by whom?
- How does the geographic gating handle TDS who travel (touring across cities)? UX must accommodate this without forcing a configuration burden.
- Where is the boundary between V1.1 (geographic gating, granularity) and V2 (CRM)? Some users may want CRM features before they have enough lookup volume to need granularity.
- Should the training-mode onboarding be one-shot or revisitable? Cohort answer.
- What are the LPD obligations if the safety database holds tag-only structured data versus free-text reports? The tag-only design narrows the surface but does not eliminate it.
- How do we handle a TDS who herself becomes the subject of a report (e.g., the rare reports that flow in the other direction)? Out of V1 scope but the data model should not preclude it.

## Key Quotes

> "On va dire plus large, communication."
> — David, expanding the framing from messaging to communication

> "Le recurring n'est jamais un problème. On ne parle que des nouveaux."
> — David, narrowing the inbound-filtering problem to the actually-painful subset

> "Vous n'êtes pas un canal. Vous êtes un module qui s'insère dans des canaux."
> — David, naming the conceptual shift

> "Crée un tag avec carrément des emojis. Zéro texte. Zéro texte. Ça vous protège aussi légalement."
> — David, on the report response format

> "À un moment donné, tout ce que vous me dites, là, il y a zéro innovation. Si vous voulez faire une innovation, c'est sur l'interaction."
> — David, on where the engineering effort actually pays off

> "Le réputationnel, c'est le produit d'appel. Le CRM, c'est l'upsell."
> — David, on the two-POC split

> "Vous prenez l'approche Claude, et c'est tout à fait vrai, mais derrière vous allez la configurer d'une certaine façon pour qu'elle apporte un max de valeur, et surtout vous allez complètement déconnecter le 'mon gars rajoute un skill', parce que c'est surtout pas ce que vous voulez."
> — David, on the boundary between Open Claude / Nano Claude and Scarlot

## Connections

- **P5 (no centralised blacklist)** — V1 is the reputation lookup. Mode C plus the contact-card-share UX is the concrete delivery vehicle.
- **P3 (no inbound filtering) / P4 (cognitive overload)** — Mode C does not directly solve P3 (the TDS still receives all her inbound). It addresses the highest-cost slice of P3, the lookup decision, and defers the rest.
- **P2 (no client memory)** — V2 CRM is the answer; V1 does not address it.
- **A1 (collective blacklist contribution)** — V1 is consumption-only; V2 adds contribution. David's tag-only response constraint shapes the contribution UX too.
- **A4 (no-UI / conversational interface preferred)** — Mode C plus the minimalist steady-state UX is the strongest A4 commitment so far. The training mode is an interesting wrinkle: it implicitly assumes A4 is not natural, which is honest.
- **A7 (AI triage acceptable with user control)** — V1 has zero automatic agent action on the user's behalf. The agent only responds to explicit requests.
- **A9 (collective blacklist legally defensible)** — tag-only-no-free-text plus geographic gating plus no-reveal narrows the legal surface materially. Counsel still required pre-launch.
- **A10 (nFADP compliance achievable)** — user_id scoping is a structural data-minimisation move. Mode C means less data ingest. Both directionally help A10.
- **C-04 (inbound message triage)** — explicitly deferred to V3.
- **Coverage checklist (skill ref)** — V1 covers the F.5 (lookup) dimension. F.6 (memory), F.7 (templates), F.8 (filtering) move to V2 / V3.
- **Morning sync (this date)** — this session changes the morning session's primary architecture call (Bailey's device-link as default) into the secondary one. The contract between the two services and the Trojan Horse remain unchanged.

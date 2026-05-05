---
date: 2026-05-05
participants: [co-founder (Joséphine)]
duration: ~10 min (two consecutive voice notes, 20:07 and 20:10)
type: debrief
tags: [poc-testing, whatsapp-doubt, conversational-ux, ux-feedback, schema, control]
---

## 1. Narrative

These two voice notes (20:07 and 20:10 on 5 May) are Joséphine's first hands-on debrief after spending real time inside the integrated-mode Scarlot agent. They came hours after her email reply earlier the same day (recorded in `SCARLOT_SYNC_20260505_0930_post-hackathon-feedback.md`), and they are deeper, sharper, and more concrete than that email. The earlier exchange surfaced a doubt about WhatsApp from two non-tester contacts (Maryline and Yumie) and a generic "the conversational experience could be more fluid" remark from Joséphine. The voice notes turn that doubt into a specific, reasoned position from her own usage, and they spell out for the first time what an acceptable shape would look like.

She opens with the question Philippe had asked her — does WhatsApp feel safe? — and answers no, with reasons. Her first reason is meta: WhatsApp is Meta. The second, which she returns to several times, is that a chat-only surface gives the user no visibility and no manual access to her own data. She cannot see her agenda, she cannot see her client list, she cannot pull up a record and edit it. Everything must be obtained by conversing with the bot, which means every read or correction is itself a conversation turn that produces more text in the same thread. From a TDS point of view this is not a usability nit. She frames it explicitly as a control issue: TDS are a population that is "marginalisée, contrôlée, surveillée, fichée, pas fichée — elles exercent une profession libérale qui n'est pas en fait libre". A tool that holds their data and only lets them retrieve it through a conversation reproduces, at the level of the interface, the same lack of agency the community already lives with elsewhere. This is the deepest line of her feedback and it shows up in both voice notes.

From there she moves to the texture of the experience. Typing into WhatsApp is heavy ("chronophage", "ça me gonfle" — a phrase she uses about communication with clients on the same channel), and she makes a precise comparison: the impatience she felt while feeding the bot is the exact same impatience she feels while managing client conversations. The channel is the same, the typing posture is the same, and as a result the agent does not dissociate itself from the burden of conversation management it is supposed to relieve. She names this as the failure of the value proposition: the bot, as currently delivered through WhatsApp, blends into the very pain it is meant to solve. She compares it with how ChatGPT, Claude or Monday work — each produces an artefact (a document, a board, a CRM view) that lives outside the chat and that the user can open, scan, and edit. WhatsApp gives her none of that. Her formulation: "L'interface WhatsApp ne doit pas être l'interface principale d'échange. Ça ne change pas du tout, ça ne fait pas de différence."

She then moves to specific UX failures. The bot answers too fast, in a way that feels like a chatbot replying to an unfinished prompt — she compares it to a Claude or ChatGPT instance starting to respond before you have stated what you want. The pacing creates pressure rather than relief. The visual layout in WhatsApp puts her own messages and the bot's messages on different sides, but as the thread grows she finds she cannot reliably tell them apart, and scrolling back to find a piece of information is the same act of "remonter le fil" she resents in client threads. This is the second front on which the agent reproduces the pain it should be removing.

She then opens up the schema question for the first time with operational specificity. The bot today asks generic, technical-sounding questions and does not ask for the things that actually matter to a TDS. She lists the canonical fields for a client/appointment record as she sees them: for the client profile, name or pseudonym and/or phone number; for the booking, duration, location (address, city, hotel, code name), service, price, and notes. Notes break down further: information about the place, about the service, about the price, plus dress code, intercom code, things-not-to-forget. She gives a concrete failure example — if she types "rendez-vous Philippe 3h demain", the bot accepts it and does not flag that the address is missing, which means she may also have failed to ask the client for the address. The bot is not domain-aware enough to act as a safety net for the things she is supposed to capture. She also flags the absence of natural shortcuts: she would write "GVA" for Geneva and the system should accept it, today she has to write the full word, which adds friction in exactly the moments she is trying to be fast.

The duplicate-handling failure is her clearest concrete bug. When she enters an appointment, then later modifies a field (location, time), the bot does not update the existing record — it creates a new one and reports the conflict back to her using internal IDs ("ID 6", "ID 3", "ID 63 machin"). Two failures stacked: the bot exposes implementation-level identifiers in user-facing language, and the only resolution it offers is that she remove the duplicates manually, which she cannot do because there is no manual access to the data. Her workaround is to ask the bot to wipe everything and re-enter from scratch. She names this twice: once as a UX failure ("confusion totale"), once as the political point ("je n'ai pas accès manuellement, je ne peux pas contrôler mes choses"). The second framing matters more — the duplicate bug is a symptom, the lack of agency is the structural complaint.

She closes the first voice note on the seeding problem (already raised in the email): even if the conversational entry worked, the per-message data entry is incompatible with a real onboarding — they will lose Adèle if she has to dictate her client base one field at a time into a WhatsApp self-chat. The team needs another way in.

The second voice note is a tightened recap of the first, delivered in the format she announced ("limites et améliorations"), and it adds two things. First, a comparison to Telegram: Telegram supports plugins / mini-apps, and a plugin layer alongside the messaging interface is closer to the shape she thinks the product needs. She immediately notes the constraint that kills this as a primary path — the Swiss TDS clientele is overwhelmingly on WhatsApp, with Telegram and Signal as minorities, so Scarlot cannot test on Telegram and reach a representative sample. The implication she does not state but that hangs over the comment is that the product needs both: WhatsApp as the channel for client interaction and a separate visual/management layer for the TDS herself. Second, she returns one more time to the control / agency point and explicitly acknowledges that this kind of framing may sound heavy or exaggerated to Philippe, but asks him to take it seriously: "c'est vrai en fait et c'est ça qu'on doit aussi comprendre."

She closes both notes on enthusiasm — she calls the project "trop bien", says "ça m'excite trop", and apologises for the volume of negative material. She is explicit that the negativity is "des pistes d'amélioration et de réflexion", not a verdict. She will continue testing the next day, in chunks between her own work, and asks Philippe to flag anything unclear so she can come back to it.

The conversation lands the team in a different place than the morning's email exchange. The earlier exchange left "WhatsApp may not be the right channel" as a flag from third parties. The voice notes turn it into a first-person, reasoned position from the co-founder, with two distinct components: WhatsApp as the only interface fails, and the failure is structural (no visibility, no manual agency, no separation from client-conversation pain). The corollary — which Joséphine does not yet propose but which the framing implies — is that the product needs a complementary visual/management surface even in V1, not as a future ergonomic upgrade.

## 2. Decisions

**WhatsApp cannot be the sole interface for the TDS-side experience — a complementary visual/management surface is required.**
Joséphine's testing produced a first-person, reasoned position (not just a third-party flag, as in the email earlier the same day) that a chat-only surface fails on three grounds: no visibility into one's own data, no manual editing path, and no dissociation from the client-conversation burden. Alternatives discussed: stay WhatsApp-only and improve conversational UX (her test shows this does not solve the structural problem); switch primary channel to Telegram (rejected because the TDS clientele is overwhelmingly on WhatsApp). Reasoning: the value proposition of relieving cognitive load collapses when the agent reproduces the same typing posture, the same scroll-back behaviour, and the same channel as client conversations; on top of that, the absence of a manual data view recreates, at the interface level, the lack of agency this community already faces structurally. Confidence: PROBABLE — strong reasoning from one co-founder test; needs validation with first beta users (Adèle, Meron) and on the broader assumption A4 across more TDS. Revisit if: the first beta testers report that a well-tuned WhatsApp-only experience is sufficient.

**Client and appointment records have a defined domain schema, owned by Joséphine.**
Joséphine spelled out the canonical fields the agent must capture: for the client, name or pseudonym and/or phone number; for the booking, duration, location (address / city / hotel / code name), service, price, and notes; with notes covering place-info, service-info, price-info, dress code, intercom code, things-not-to-forget. Alternatives implicit: continue with the bot's current generic questions. Reasoning: the current bot asks technical-feeling questions that miss the domain, fails to flag missing required fields (e.g. accepting "rendez-vous Philippe 3h demain" without noticing the address is absent), and so cannot act as a safety net for what the TDS is supposed to remember to ask the client. Confidence: CERTAIN on the field list (it comes from her direct knowledge of the work). Revisit if: beta testers reveal additional required fields or different defaults.

## 3. Context Shifts

**A4 (No-UI / conversational interface preferred) is now under direct pressure.**
Until today A4 was Philippe's hypothesis with `OPEN` status, untested by users. Joséphine's hands-on test does not refute the conversational-native bet entirely — she still wants conversational entry to work — but she explicitly rejects the version of A4 where *everything* lives in a messaging thread. The shift: the operative question is no longer "conversational vs. UI", it is "what minimum visual/management surface does the TDS need alongside the conversational layer". This reframing should be carried into the next planning conversation and into the first beta tests. The status of A4 is not yet `CONTRADICTED` (she tested alone, conversational entry was not deeply tested across multiple realistic flows), but it is no longer cleanly `OPEN` either — the strongest insider voice in the team has flagged it.

**Control / agency is a first-class product requirement, not a nice-to-have.**
The conversation surfaced — twice and explicitly — that for this user community, the absence of manual access to one's own data reproduces a pattern of structural disempowerment they live with elsewhere. This frames any feature that "owns" data behind the bot (records, calendar, blacklist, payments) as politically loaded. The implication: even a small "view / edit / delete" surface, however basic, has disproportionate weight as a trust signal.

## 4. Action Items

- [ ] Reply to Joséphine confirming what is clear and what needs follow-up before Thursday's meeting — Philippe
- [ ] Bring a concrete proposal for the complementary visual/management surface (web dashboard? local view? read-only first?) to Thursday's meeting — Philippe
- [ ] Define the client/appointment record schema in code from Joséphine's field list (name/pseudo + phone, duration, location, service, price, notes with sub-fields) — Philippe
- [ ] Make the bot domain-aware: detect missing required fields after an entry and prompt for them (e.g. flag missing address on "rendez-vous Philippe 3h demain") — Philippe
- [ ] Replace user-facing internal IDs ("ID 6", "ID 3") with natural references (date, name, time) in agent replies — Philippe
- [ ] Fix duplicate-on-update behaviour: a follow-up turn that adds or modifies a field should update the existing record, not create a new one — Philippe
- [ ] Support natural shortcuts in user input (e.g. "GVA" → Geneva) — Philippe
- [ ] Improve agent pacing: do not reply instantly with multi-part responses; wait for the user to be done — Philippe
- [ ] Find a non-WhatsApp seeding path for initial client data so onboarding does not require message-by-message entry — Philippe + Joséphine
- [ ] Validate timezone handling: simplify defaults to common ones (Berlin / Rome / Paris / Geneva) and make timezone inference clearer — Philippe

## 5. To Think About

- The right minimum shape for the management surface in V1. Read-only might be enough as a first step (visibility is the first complaint, manual editing is the second). A web dashboard tied to the same backend, behind login, may be the lowest-cost answer that addresses both visibility and the agency point.
- How to surface "what the bot still needs from you" without re-creating the conversation drag. A daily summary message? A persistent "things to fill" list visible in the management surface?
- Whether the bot should use a different visual register (different prefix, different layout, even a different chat) so the user can distinguish her own typing from the agent's replies in the WhatsApp thread.
- Whether the duplicate bug is downstream of a fundamental modelling gap (no clear "current draft appointment" notion in the agent's working memory) rather than a fix-in-place.
- The political dimension Joséphine raised may also imply something about defaults and reversibility — e.g. delete-locally-on-request, exportability, and being explicit about what the bot does and does not retain.

## 6. Open Questions

- What is the cheapest acceptable form of the complementary visual/management surface? (Web dashboard? Read-only PDF/email digest? Native app?)
- Is there a path to seed initial client data outside of WhatsApp message-by-message entry that respects the privacy posture (no PII transit through unprotected channels)? CSV upload via the management surface? Voice transcription of a dictated list?
- For the management surface, where does it live and how is access secured? (Same Swiss hosting story? Same per-TDS isolation as the bot containers?)
- What is the right way to present a duplicate-detected case in a TDS-facing way — not "ID 6 vs ID 3", but something like "I already have an appointment with Philippe tomorrow at 3pm — should I update the address or create a second one?"

## 7. Key Quotes

> "L'interface WhatsApp ne doit pas être l'interface principale d'échange. Ça ne change pas du tout, ça ne fait pas de différence. Du coup, tu ne dissocies pas en fait cette interface avec la discussion client et du coup je pense que tu perds complètement la valeur ajoutée."
> — co-founder

> "Je n'ai pas accès manuellement, je ne peux pas contrôler mes choses. De nouveau, conflit avec le travail du sexe, qui est quelque chose qui est toujours contrôlé par les autres."
> — co-founder

> "Elles ne sont pas juste stigmatisées, elles sont marginalisées, contrôlées, surveillées, fichées, pas fichées — les lois font qu'elles exercent une profession libérale qui n'est pas en fait libre. Donc ça te donne encore un sentiment de ne pas pouvoir contrôler les choses."
> — co-founder

> "Genre moi ça me fait un truc d'impatience assez fort qui me fait penser à la exacte même impatience que quand je discute avec un client sur WhatsApp."
> — co-founder

> "Si moi je mets par exemple rendez-vous Philippe 3h demain, ça ne va pas me faire me rendre compte que j'ai oublié de noter l'adresse. Donc peut-être que j'ai oublié de demander l'adresse au client."
> — co-founder

> "Je ne veux pas sonner pour quelqu'un qui est en train de pointer tout le négatif, mais j'essaie de pointer des pistes d'amélioration et de réflexion."
> — co-founder

## 8. Connections

- **A4 (No-UI / conversational interface preferred)** — first hands-on insider test puts the strongest version of A4 under pressure ; reframes the question from "conversational vs. UI" to "what minimum management surface alongside the conversational layer". Status should be reviewed at the next planning conversation.
- **A7 (AI triage acceptable with user control)** — the duplicate-handling failure is also an A7 signal: the bot acted (created records, flagged conflicts) without giving the user a way to correct outcomes manually. Control is the missing half.
- **P2 (no client memory)** — Joséphine spelled out the canonical client/appointment schema (name/pseudo + phone, duration, location, service, price, structured notes) as the operational shape of the memory layer.
- **P4 (mental load / cognitive overload)** — the central failure mode she described is that the WhatsApp-only experience reproduces, rather than relieves, P4. The agent must dissociate itself from the channel pattern to deliver against P4.
- **P8 (manual repetition)** — natural shortcut handling (e.g. "GVA" → Geneva) and missing-field detection both belong to the P8 surface.

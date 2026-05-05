---
date: 2026-05-05
participants: [advisor (Philippe), co-founder (Joséphine), Andrew (collaborator)]
duration: ~3 days (asynchronous email exchange, 2–5 May 2026)
type: post-hackathon debrief / first user-side feedback
tags: [hackathon, beta-prep, whatsapp-doubt, conversational-ux, taxonomy, onboarding]
---

## 1. Narrative

This record covers the email exchange between Philippe and Joséphine (cc Andrew) following the 30 April hackathon. Philippe sent a long recap on 2 May describing what was built, what works, what doesn't, and what he needed from Joséphine before the pilot. Joséphine replied on 5 May after she had personally tested the integrated mode and had two informal conversations with TDS contacts (Maryline and Yumie). The exchange is the first instance where the working system met the founder side of the team in a hands-on way and where a TDS-side reaction (even non-tester) was elicited about the channel itself.

Philippe's recap framed the system as "end-to-end complete" : signup on the site, QR code, WhatsApp pairing, first message to Scarlot inside the WhatsApp self-chat. He distinguished two onboarding paths — "Mode contact" (writing to Scarlot as a third-party contact, not yet functional) and "Mode Intégré" (writing to oneself in WhatsApp with Scarlot answering inline, functional). He explicitly asked Joséphine to only test Mode Intégré at this stage. He clarified that the safety lookup was not yet wired to Scarlot — users can blacklist numbers for themselves, but the call into the reputation base (safety-data) is not yet implemented. He walked through the three-service split (site, assistant, reputation base) and the rationale (each service only knows what it needs ; if one is compromised, the others stay safe).

He listed what works today (verified during the hackathon) : end-to-end signup, multi-tenant deployment with one container per TDS, the 17 Scarlot tools (client records, appointments, payments, blacklist, settings), the "cheap door" routing simple commands away from AI calls, the phone masking layer (the AI sees PHONE_1, PHONE_2 — never raw numbers, which protects against prompt injection through messages), the appointment reminder (60-second polling), the event log, and the full And6 ingest with local AI enrichment (10,228 reports across 7,876 phones, 96.8% classification coverage from the AI+rules combination, with health_risk identified independently by the model as 2.8% of the corpus). He listed what is not yet done : Scarlot ↔ reputation base wiring (Phase 5 of the safety-data plan), encryption at rest (SQLCipher per-TDS keys designed but not coded), no real users yet, no voice transcription wired (Gemini Flash validated but not connected), no SMS/OTP on the site, no rate limiting on signup, no soft-delete or backups or SLA, the DPIA still in draft, only one signal source (And6), and hosting in Germany rather than Switzerland (chosen for cost and speed, to revisit before production).

He then named what he needed from Joséphine before the pilot : (1) the value proposition, tone of voice, branding, and assistant experience ; (2) a decision on whether health_risk should be exposed publicly as a separate category or folded into "dangerous" ; (3) a beta user list. He proposed a two-week trajectory : finish the Scarlot ↔ reputation base wiring, expand to more report sources, automate the safety data freshness, wire voice, run a self-testing phase, validate a pilot information document, then onboard the first n users individually with a shared backend WhatsApp chat for feedback, and keep larger deployment behind further hardening.

Joséphine's reply opened with strong enthusiasm (the system going live "in one weekend" being a moment she had waited a long time for). She then split her response into two parts : answers to Philippe's questions, and her first usage feedback.

On the questions : she said health_risk **should** be a distinct category, but flagged that she is not herself a user of online blacklists and wanted that choice validated by experienced users during the first tests — i.e. her answer is a strong default but not final. On beta users : she said 1 to 3 maximum, drawing on her past experience with Choice — at distance, interest and trust evaporate fast ; she needs to go meet them and onboard them in person. The first names she had in mind were Adèle and Meron. On the value proposition / tone of voice / branding ask : she explicitly asked Philippe to clarify what he wanted from her, to make sure they were aligned before she invested effort.

On her actual usage of the system : she opened with genuine surprise at seeing it actually respond. Four concrete observations followed.

First, the QR code did not work with her professional number because that number is not paired with a second device — and she flagged this as probably a frequent case among TDS, requesting an alternative onboarding path. (Philippe's draft response notes there was also a small bug : after the number is validated and the QR appears, the page sometimes crashes and needs a refresh. He has cleaned the links tied to her secondary number and asked her to retest.)

Second, on the conversational experience : she said this is where the most potential lies. Her concrete observation : feeding Scarlot today requires entering many bits of information one at a time through message turns — and that pattern is exactly what TDS already find heavy in their daily life. She added that the WhatsApp side does not give a feeling of safety, and that this matters a lot for TDS. The challenge she named : how to make it fluid enough that the project clearly distinguishes itself from the burden of conversation management TDS already carry. She also said the language could be warmer ("chaleureux" — she wasn't sure that was the right word), and that the first questions (language, timezone) could be simplified or moved earlier in the flow. She asked to defer her concrete proposals on this to their Thursday meeting rather than draft them in email.

Third, on onboarding : she reaffirmed that the first testers must be met in person — she proposed a test with Adèle on Friday if Adèle agrees. She also flagged a related need : a way to seed the initial client data outside of WhatsApp message-by-message entry. She didn't have a solution but called it out as something to figure out together.

Fourth, on data and legal : she asked for a simple document explaining how user data is protected and what participation as a beta user implies — she sees this as essential for trust, suggesting something short, lawyer-validated, and signed by users at onboarding.

Finally, she closed with the most strategically loaded note in the email : Maryline and Yumie (with whom she spoke Sunday but did not have test) remain enthusiastic about the project, **but they expressed reluctance about WhatsApp as the main channel for this kind of service**. Joséphine explicitly flagged this as an important signal to keep in mind. Her closing summary captures the ambivalence : "very exciting and encouraging to see something take life... however, the interaction with WhatsApp may not be the key platform for the finality of the project. I don't know if it's the conversational agent or if it's WhatsApp, the beta users will tell us."

The exchange landed with the team aligned on the mood (excited, the system is real) but with one substantive new tension : the channel itself may be in question among the very community Joséphine has insider access to. This is not a refutation of the conversational-native bet — it leaves open whether the friction is the channel or the agent — but it is the first time WhatsApp specifically is named as a possible problem rather than simply assumed. Philippe's draft response asks her to elaborate on the "WhatsApp doesn't feel safe" point so they can understand the underlying concern before the Thursday meeting.

## 2. Decisions

**health_risk to be exposed as a distinct public category — provisional, to be confirmed with first beta users.**
Joséphine's default position is yes, separate category. She is not a personal user of online blacklists, so she explicitly conditioned her vote on validation by experienced users during the first tests. Alternatives discussed : fold health_risk into "dangerous" (Philippe's question framing). Reasoning to keep it separate : the AI independently identified health_risk as a meaningful signal (2.8% of corpus), and folding it into "dangerous" loses information that may matter operationally to a TDS deciding whether to engage. Confidence : PROBABLE. Revisit if : first beta users say a 6-category surface is too much, or if they consistently treat health_risk and dangerous as the same thing.

**Beta cohort starts at 1–3 users, not more.**
Joséphine's hard-won lesson from her time at Choice is that remote onboarding loses interest and trust fast in this community. Starting small means each tester gets close, in-person accompaniment from her. Alternatives implicitly considered : a wider opening to capture more signal early. Reasoning : the cost of one bad early experience in this community is high (word travels, trust does not return), so the priority is depth over breadth at the start. Confidence : CERTAIN (she has direct prior experience supporting this). Revisit if : the first 1–3 testers stabilize and ask to bring others in themselves.

**In-person onboarding for the first beta testers.**
The first tester onboarding will be in person, not remote. First proposed test : with Adèle, Friday (week of 5 May), pending her confirmation. Alternatives considered : remote onboarding to scale faster. Reasoning : same as above — distance erodes trust and engagement in this population. Confidence : CERTAIN. Revisit if : a particular tester actively prefers remote (unlikely given the population profile).

**A simple data-protection / pilot-participation document will be drafted, lawyer-validated, and signed by beta users at onboarding.**
Joséphine flagged this as essential for trust ; Philippe confirmed it was already planned. Format : short, lawyer-validated, signed at onboarding. Confidence : CERTAIN. Revisit : not anticipated — this is a baseline requirement.

**The first n users will receive feedback via a shared WhatsApp chat with Philippe and Joséphine in the backend.**
From Philippe's proposed trajectory, not contested by Joséphine. This becomes the operational feedback loop during pilot. Confidence : PROBABLE. Revisit if : the channel itself becomes a problem (see context shift below) — the feedback channel may need to be different from the product channel.

## 3. Context Shifts

**WhatsApp specifically is now under question, not just abstractly.**
Until now, the team has operated on Philippe's hypothesis (A4) that a conversational interface inside an existing messaging app is preferred over a dedicated app. WhatsApp was the working assumption because it is dominant. Joséphine reports that two TDS in her network (Maryline and Yumie) — who remain enthusiastic about the project — explicitly named WhatsApp as a problem for "this kind of service." Joséphine extends the doubt herself : "le côté WhatsApp ne donne pas de sentiment de sécurité, et ça c'est très important pour les TDS." This does not collapse A4 (the conversational pattern itself may still be right) but it weakens the conviction that WhatsApp is the right channel. The open question becomes : is the friction in the channel, in the conversational pattern, or in the current implementation of the assistant ? The first beta users are now expected to be the source of signal on this.

**The data-entry burden is the same friction TDS already complain about — and it shows up at the wrong moment.**
Joséphine's observation that bootstrapping the agent requires many message-turns of one-info-at-a-time entry is more than UX friction. She names it as the exact pattern TDS find heavy in their daily life. This means the current onboarding flow may be performing a regression of the very problem the product is supposed to relieve, at the moment of first contact. This shifts the priority of seeding mechanisms (some way to import or batch-enter initial client data) from "nice to have" to "blocking for first impressions." Philippe's draft response acknowledges this and frames it as something to improve over time, not solve immediately.

## 4. Action Items

- [ ] Philippe to fix the post-validation page crash in the QR onboarding flow ; he has already deleted the links tied to Joséphine's secondary number so she can retest. Joséphine to retest and report back. — Owner : Philippe (fix), Joséphine (retest)
- [ ] Joséphine to elaborate on what specifically about WhatsApp does not feel safe — Philippe asked for this explicitly so they can understand the underlying concern. — Owner : Joséphine
- [ ] Philippe to clarify what he wants from Joséphine on "valeur, tone of voice, branding, expérience" : concrete deliverables, format, scope. He commits to defining this concretely and pointing to online resources that help produce the assets. — Owner : Philippe
- [ ] Move the language and timezone questions out of the post-pairing conversational flow and into the pre-WhatsApp flow (right after number verification). Consider collapsing timezone into a single question, optionally inferred from geolocation (without tracking, without storing exact coordinates). Note : timezone matters because of agenda/booking and because a TDS may be in one city preparing a stay in another (e.g. Budapest preparing London). — Owner : Philippe (implementation), Joséphine (UX validation)
- [ ] Confirm Friday (week of 5 May) in-person onboarding test with Adèle (pending Adèle's agreement). — Owner : Joséphine
- [ ] Find a way to seed initial client data outside of WhatsApp message-by-message entry. — Owner : both, no solution proposed yet
- [ ] Draft the short pilot information document (data protection, participation terms), get lawyer validation, prepare for signature at onboarding. — Owner : Philippe (already planned)
- [ ] Joséphine to bring concrete proposals on conversational experience, language, warmth, and onboarding question flow to the Thursday in-person meeting. — Owner : Joséphine — Deadline : Thursday meeting
- [ ] Philippe to finish the Scarlot ↔ reputation base wiring (Phase 5 of safety-data) — the priority-one item before the pilot opens. — Owner : Philippe

## 5. To Think About

**Is the channel friction WhatsApp specifically, or messaging-as-channel more broadly ?** If Maryline, Yumie, and Joséphine's discomfort with WhatsApp also extends to Telegram or other messaging apps, the conversational-native bet (A4) is in trouble. If it is WhatsApp specifically (Meta, US-owned, perceived as not private), the answer is to support alternative channels — which the architecture already anticipates ("Channel-agnostic architecture. No single platform is existential" — CLAUDE.md). The first beta users are expected to surface this.

**The data-entry pattern at onboarding repeats the daily-life pain.** Worth thinking about whether the first session should be guided ingest (Joséphine seeds data with the user in person) rather than the user typing it in turn by turn. This connects to the in-person onboarding decision.

**The Thursday meeting should consolidate Joséphine's concrete UX proposals before any tone-of-voice / branding work starts.** Philippe's draft asks Joséphine for concrete deliverables on this ; alignment first, then production.

## 6. Open Questions

- What specifically about WhatsApp does not feel safe to TDS ? (Asked of Joséphine in Philippe's draft response.)
- Should language and timezone collection happen before WhatsApp pairing, after, or both ? Single question or two ? Geolocation-inferred timezone vs explicit user input — what is acceptable to TDS ?
- What is the right alternative onboarding path for users whose professional number is not on a second device ? (Different QR flow ? SMS code ? Manual pairing assistance during in-person onboarding ?)
- What does "valeur de marque, tone of voice, branding, expérience" look like as concrete deliverables ? (Philippe to define.)
- For the data-entry seed problem : import from contacts ? Voice-driven batch entry with the user in person ? Pre-filled template the user reviews ?

## 7. Key Quotes

> "Le côté WhatsApp ne donne pas de sentiment de sécurité. Et ça, c'est très important pour les TDS."
> — Joséphine

> "Maryline et Yumie sont toujours aussi partantes pour le projet. En revanche, elles émettent une réticence sur WhatsApp comme canal principal pour ce type de service. C'est un signal important à garder en tête."
> — Joséphine

> "Aujourd'hui, pour alimenter le bot, il faut rentrer beaucoup d'infos une par une et via des échanges de messages : et c'est justement ce que les TDS trouvent lourd dans leur quotidien."
> — Joséphine

> "À distance, on perd vite leur intérêt et leur confiance. Je dois aller les rencontrer, les onboarder moi-même."
> — Joséphine (drawing on Choice experience to argue for in-person, small-cohort beta start)

> "Pour moi-même, je n'ai pas encore la réponse. On avance, et ça c'est sexy !"
> — Joséphine (closing — captures the genuine excitement coexisting with the channel doubt)

> "C'est là à mon sens où se trouve la véritable innovation de ce qu'on est en train de réaliser. Faut que ce soit super fluide et pas comme un 'bête' chatbot."
> — Philippe (on the conversational experience — both agree this is the critical know-how to develop)

## 8. Connections

- **P5 (no centralised blacklist)** — Philippe identified the Scarlot ↔ reputation base wiring as priority one before pilot ; the safety lookup is what unlocks the highest-value feature, which is exactly the gold-tier P5 problem.
- **P4 (mental load / cognitive overload)** — Joséphine's observation that the data-entry pattern repeats the daily-life pain is a direct callback to P4 ; the product currently regresses the very problem it aims to relieve at the first-impression moment.
- **A4 (no-UI / conversational interface preferred)** — weakened, not refuted. The signal from Maryline, Yumie, and Joséphine about WhatsApp specifically does not yet collapse the conversational bet, but it removes WhatsApp's status as a safe default. First beta users become the test.
- **A1 (collective blacklist contribution)** — implicit : the health_risk taxonomy decision and the priority of wiring the reputation base into Scarlot both depend on A1 holding.
- **A8 (willingness to pay at meaningful levels)** — not directly discussed but the in-person, small-cohort beta start is the cheapest test that doesn't require pricing yet. Pricing signal will come later.
- **A10 (nFADP compliance achievable)** — the pilot information document and the still-unfinalised DPIA are both gating items for A10 ; Joséphine's request for a signed user document fits inside this assumption's resolution path.

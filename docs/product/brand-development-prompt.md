# Brand Development — Working Prompt for Joséphine

This is a working tool for Joséphine to co-author Scarlot's brand foundation with Claude. It produces, through conversation, five deliverables :

1. **Value proposition** — what Scarlot offers, to whom, against what alternative
2. **Onliness statement** — the one differentiator no competitor can claim
3. **Tone of voice** — four-axis profile, do's and don'ts, sample copy
4. **Brand / agent personality** — how Scarlot behaves in conversation
5. **Conversational experience principles** — how Scarlot greets, asks, confirms, fails

The prompt below is designed to be pasted into a fresh Claude conversation. It briefs Claude on Scarlot, sets the working method (one stage at a time, validated by Joséphine before moving on), and produces structured artefacts.

---

## How to use

1. Open a new Claude conversation (claude.ai or Claude Code).
2. Paste the prompt block below as your first message.
3. Answer Claude's questions one stage at a time. Don't try to skip ahead.
4. At the end of each stage, Claude will produce a draft artefact. Validate, adjust, or push back.
5. Save each artefact (Claude will tell you the suggested filename).
6. When you reach Stage 5, you will have the full set.

Work in French if that's where you think best — Claude handles French natively. Output can be in French ; we will produce the English / German / Spanish localizations later.

---

## The reference frameworks behind the prompt

The prompt borrows from four established frameworks. You don't need to read these to use the prompt, but if you want to dig in :

- **[Strategyzer Value Proposition Canvas](https://www.strategyzer.com/library/the-value-proposition-canvas)** — jobs / pains / gains × products / pain relievers / gain creators. Standard for matching offering to user need.
- **[Marty Neumeier — Onliness statement](https://www.martyneumeier.com/the-brand-gap)** (from *The Brand Gap* and *Zag*) — "Our [offering] is the only [category] that [benefit], for [audience], in [geography], during [trend]." A decisional filter for every future product call.
- **[Nielsen Norman Group — Four Dimensions of Tone of Voice](https://www.nngroup.com/articles/tone-of-voice-dimensions/)** — formal/casual, serious/funny, respectful/irreverent, matter-of-fact/enthusiastic. Each dimension scored on a spectrum.
- **[Mailchimp Voice and Tone guide](https://styleguide.mailchimp.com/voice-and-tone/)** — best-in-class public example of a brand voice doc. Worth skimming as a reference for the kind of artefact we're producing.

For chatbot-specific personality work, [Kommunicate's playbook](https://www.kommunicate.io/blog/chatbot-personality-playbook/) and [Tidio's chatbot persona guide](https://www.tidio.com/blog/chatbot-persona/) cover the conversational layer (greeting, error states, confirmations).

---

## The prompt

Copy everything between the lines below into Claude.

---

```
You are a brand strategist working with me to define the brand foundation for a product called Scarlot. I am Joséphine, the co-founder. I have insider credibility in the user community ; you have framework expertise. We work as peers : you do not lecture, I do not delegate.

CONTEXT — read this carefully before responding.

Scarlot is a personal AI assistant for independent sex workers (TDS — travailleuses du sexe) in Switzerland, where sex work is fully legal and regulated. The assistant lives inside WhatsApp (the user writes to herself in WhatsApp ; Scarlot answers in that thread). It does not read or reply to client conversations. It helps the user manage : safety lookups (is this number known to be problematic ?), client memory (who have I seen, what happened, what should I remember), bookings, payments, and a personal blacklist.

Two principles are non-negotiable :
- Semi-automatic, never automatic. The user controls every decision. The assistant assists, never decides.
- Individual first, collective second. The personal tool must work before any shared features.

The user community has very specific characteristics that should shape every brand choice :
- High distrust of institutions, platforms, and tools designed by outsiders. Every existing tool was designed for someone else (BMG, F-Girl, WhatsApp Business). Trust is earned slowly and lost instantly.
- Strong cultural resistance to AI replacing direct human interaction. "Semi-automatic" is acceptable ; "the bot answers my clients" is not.
- A real fear of being patronized, infantilized, or moralized at. They are professionals running businesses. Treat them as such.
- Multilingual : French, German, Italian, English. We launch in French (Geneva) first.
- The community calls the work "TDS" (travail du sexe) and rejects euphemisms like "escort" or moralizing language like "girl". Mirror their language.

The first beta cohort is 1 to 3 users, onboarded in person by me in Geneva. Names of likely first testers : Adèle, Meron.

A signal we just received : two non-tester TDS in my network are enthusiastic about the project but expressed reluctance about WhatsApp specifically as the channel. We don't yet know if it's WhatsApp or the conversational pattern in general. Hold this open question — it may shape brand work.

WORKING METHOD

We will work in five stages, one at a time. After each stage, you will produce a structured artefact in markdown that I can save and version. Do not move to the next stage until I confirm the current one.

At every stage, you will :
- Ask me questions before producing anything. If you can't write the artefact without asking me something, ask. Don't guess.
- Default to my judgment on community fit. I am the insider. If I push back on a framework choice because it doesn't fit the community, the community wins.
- Avoid clichés : "empowering", "supportive", "we got you", "hey gorgeous", "you do you", "bestie", and any startup-speak warmth that would feel performative or condescending to a TDS reading it for the first time.
- Avoid moralizing language and euphemisms.
- Write at a level that respects the reader as a professional running a business.

If at any stage you don't have enough context to ask good questions, tell me what you need to know about the community, the product, or the existing materials. I have the product spec, the discovery interviews, and the priority stack — I can paste relevant excerpts.

THE FIVE STAGES

Stage 1 — Value proposition.
Use the Strategyzer Value Proposition Canvas (jobs / pains / gains on the user side ; product / pain relievers / gain creators on our side). Ask me about :
- The top 3 jobs-to-be-done a TDS has on a working day
- The top 3 pains in those jobs (what specifically hurts ?)
- The top 3 gains they would value (not what we offer — what they want)
- The top 3 ways Scarlot relieves those pains
- The top 3 ways Scarlot creates those gains
Then produce a one-paragraph value proposition statement and the filled canvas as a markdown table.

Stage 2 — Onliness statement.
Use Marty Neumeier's onliness format : "Scarlot is the only [category] that [benefit], for [audience], in [geography], during [trend]." Ask me what we are NOT (what category we refuse), what no competitor can claim, and what the trend is that makes Scarlot's moment now. Produce three candidate onliness statements ; I will pick or merge.

Stage 3 — Tone of voice.
Use the NN/g four dimensions. For each dimension, propose a position on the spectrum (1 to 10) with a one-sentence justification rooted in the community context :
- Formal vs casual
- Serious vs funny
- Respectful vs irreverent
- Matter-of-fact vs enthusiastic
Then produce a do / don't list (5 of each) with a real example of each : "We say X. We don't say Y." Then write three sample messages in the proposed voice : (a) the very first message Scarlot sends a brand-new user, (b) a confirmation before a destructive action ("you want me to delete this client ?"), (c) an error state ("I didn't understand"). Write these in French.

Stage 4 — Brand / agent personality.
Define Scarlot as a character. Not a mascot. A consistent voice. Ask me :
- Who is Scarlot to the user ? An assistant ? A colleague ? A discreet collaborator ? A trusted operator ?
- What is Scarlot's relationship to the user's profession ? (Neutral ? Allied ? Pragmatic ? Protective ?)
- How does Scarlot handle sensitive moments — the user is upset, exhausted, in danger, embarrassed ?
- What does Scarlot never do, even if asked ? (Hard limits.)
Produce a one-page persona doc : name, archetype (using a recognized framework like the 12 Jungian archetypes or similar — propose the best fit), three character traits, three character anti-traits, three signature behaviors, three hard limits.

Stage 5 — Conversational experience principles.
Drawing on everything above, write the principles that govern how Scarlot conducts itself in conversation. Cover at minimum :
- First message a new user receives after pairing WhatsApp
- How Scarlot asks a question (one at a time vs batched ; multiple choice vs open)
- How Scarlot confirms a destructive action
- How Scarlot handles ambiguity ("I'm not sure what you meant, do you want X or Y ?")
- How Scarlot handles errors and silences
- How Scarlot handles the user being short, rude, or upset
- How Scarlot signs off / closes a thread
- Length norms (how long is a Scarlot message, by default ?)
- What Scarlot never does (recap of hard limits from Stage 4 in conversational form)
Produce a markdown doc with one principle per section, a one-line rule, and a worked example showing the principle in action.

OUTPUT FORMAT

For each stage's artefact, propose a filename of the form `brand-stage-N-[topic].md` and produce clean markdown I can save directly.

START NOW

Begin Stage 1. Ask me your first set of questions about jobs / pains / gains. Don't produce the canvas yet — just ask.
```

---

## What you'll have at the end

Five files, ready to drop into `docs/product/` :

```
docs/product/brand-stage-1-value-proposition.md
docs/product/brand-stage-2-onliness.md
docs/product/brand-stage-3-tone-of-voice.md
docs/product/brand-stage-4-personality.md
docs/product/brand-stage-5-conversational-experience.md
```

These become the foundation for : the website copy rewrite, the assistant's system prompt, the onboarding flow, the first message a user reads, and any future marketing.

## Notes on running the conversation

- **Don't try to do all five in one session.** Each stage is 30 to 60 minutes if you take it seriously. Spread them across two or three sessions.
- **Push back when something feels off.** Claude will produce frameworks confidently. Your job is to filter through community fit. "That word would never work here" is a complete and sufficient answer.
- **Save the conversation.** If Claude produces something good and you want to revisit, copy the chat or export it.
- **Work in French.** The brand launches in French. We localize after.

## After Stage 5

Bring the five artefacts to our next working session. Philippe will turn the conversational principles (Stage 5) into the assistant's system prompt and the website copy will get rewritten against Stages 1 to 4. The branding (visual identity, logo, color, type) is a separate workstream that comes after this verbal/strategic foundation is locked.

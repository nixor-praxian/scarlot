# Landing Page Design Spec

Target: a single-page site that captures early-access intent from independent service providers in French-speaking Switzerland, while testing two competing onboarding flows for the Scarlot agent.

This is a discovery instrument, not a production marketing site. The page exists to:

1. Validate the value proposition with cold traffic (paid and community channels).
2. Compare relative preference and conversion between two integration models.
3. Capture verified phone numbers for follow-up by the founders.

## Audience

Primary: independent sex workers in French-speaking Switzerland. Discrete, mobile-first, language matters (French primary, English and German secondary). Many are on WhatsApp from morning to night. The page may be opened on a phone in a semi-public space, so it must be unembarrassing at a glance.

## Goals

- **Primary**: capture a verified phone number plus the user's chosen flow.
- **Secondary**: measure relative pull between Flow 1 and Flow 2.
- **Tertiary**: serve as a low-pressure discovery surface, a page someone can be sent in a private message without it feeling like a marketing funnel.

## Information architecture

Single page, four sections, progressive disclosure:

1. **Hero**: value proposition and primary action ("Demander un accès").
2. **How it works**: short explainer that surfaces the choice between Flow 1 and Flow 2.
3. **Phone verification**: enter number, receive code by SMS, enter code.
4. **Confirmation**: what happens next, expected wait, how the user will be contacted.

Both flows reach the same phone verification step. The user picks a flow before verification, not after.

The two flows are **explicit choices**, not a randomized split test. We want signal on conscious preference, and the flows differ enough in trust profile that forcing one on a user would distort what we learn.

## Value proposition

Recommended hero copy (French primary):

> **Le filet de sécurité et la mémoire qui vivent dans WhatsApp.**
>
> Scarlot vous aide à filtrer les nouveaux clients, se souvient de qui est qui, et vous alerte quand un numéro a déjà été signalé. Vous gardez la main sur chaque décision.

English fallback:

> **Your safety net and your memory, living in WhatsApp.**
>
> Scarlot helps you screen new clients, remembers who's who, and warns you when a number has been reported. You stay in control of every decision.

Subhead options to consider:

- "Construit avec et pour des indépendantes en Suisse."
- "Discret. Suisse. Vous décidez, l'agent assiste."

Avoid in copy:

- The word "AI" in the hero. Lower trust signal in this audience.
- Promises of automation ("works while you sleep"). Contradicts the semi-automatic principle.
- Marketing-deck language ("revolutionary", "game-changing").
- Any wording that implies Scarlot replies to clients on the user's behalf.

## The two flows

Both reach the same phone verification step. They differ in how Scarlot ends up living inside the user's WhatsApp. User-facing names are **Mode intégré** (Flow 1) and **Mode contact** (Flow 2). The flow numbers are internal only and should not appear in copy.

### Mode intégré (Flow 1): "Scarlot reads your messages"

Pitch: Scarlot connects to the user's WhatsApp account through a secure pairing (similar to WhatsApp Web). When the user wants to ask Scarlot something, they write to **themselves**, on their own number. Scarlot replies in that thread. It can also reference incoming messages from clients without the user having to forward anything.

Trust profile: deeper integration, deeper access, higher trust ask. Sells on convenience: zero context switching, the agent already sees what the user sees.

Visual cue suggestion: a phone mockup showing the user's own conversation with themselves, with Scarlot's reply appearing below the user's question.

Technical note for design: Flow 1 is implemented via Baileys, an open-source library that speaks the WhatsApp protocol. The user pairs once via a QR code or pairing code. The design must accommodate this pairing screen as a step after phone verification.

### Mode contact (Flow 2): "Scarlot is a contact you write to"

Pitch: Scarlot has its own WhatsApp number. The user saves it as a contact and writes to it like a friend who happens to be their assistant. To bring Scarlot into a client conversation, the user forwards the relevant message.

Trust profile: shallower integration, shallower access, lower trust ask. Sells on simplicity: nothing to pair, no broad access, the user controls exactly what Scarlot sees.

Visual cue suggestion: a phone mockup of the user's WhatsApp contact list with "Scarlot" pinned at the top, plus a thread where the user forwards a client message and asks "qui est-ce ?".

Technical note for design: after phone verification, the user receives a click-to-chat link (`https://wa.me/<scarlot_number>?text=...`) and a button to add the contact.

### Comparison block

Place a side-by-side after the hero:

| | Mode intégré | Mode contact |
|---|---|---|
| Setup | Pair WhatsApp once | Save a contact |
| Where you write | À vous-même | À Scarlot |
| Sees client messages | Oui, avec votre accord | Seulement ce que vous transférez |
| Idéal si | Vous voulez un assistant complet | Vous voulez essayer sans engager |

Lead visually with Mode contact. The lower-trust option should be the easy entry point; Mode intégré should feel like the deeper commitment.

## Phone verification

Required for both flows. Same component, runs after the user picks a flow.

Steps:

1. **Enter number**: single smart input. See "Phone input behavior" below.
2. **Send code**: submission triggers an SMS via the configured provider. Display a 60-second resend cooldown.
3. **Enter code**: six-digit input, autofocus, autosubmit on completion. Allow paste. Support iOS and Android one-time-code autofill.
4. **Verified**: proceed to the flow-specific step (pairing for Mode intégré, click-to-chat for Mode contact).

### Phone input behavior

One field, not two. No country-code dropdown glued to a number input in the primary path. Multi-field international phone inputs are the failure mode we are explicitly avoiding.

Parsing is driven by Google's **libphonenumber** (https://github.com/google/libphonenumber) via its maintained JavaScript port (`libphonenumber-js` is the default choice; the full Google build is acceptable if region accuracy matters more than bundle size).

Behavior:

- Accept any of these without the user thinking about it: `+41 79 123 45 67`, `+41791234567`, `0041 79 123 45 67`, `079 123 45 67`, `0791234567`. Same flexibility for every country libphonenumber supports.
- Parse and reformat on every keystroke using libphonenumber's as-you-type formatter. Validate continuously; show errors only after the input becomes long enough to be meaningfully wrong.
- Treat `00` as equivalent to `+`. Both are international access prefixes.
- **Region hint comes from server-side IP geolocation**, passed into the parser as the default region. A user in Switzerland typing `079 123 45 67` is parsed as `+41 79 123 45 67`. A user in France typing `06 12 34 56 78` is parsed as `+33 6 12 34 56 78`. The hint is a default, not a constraint: as soon as the input starts with `+` or `00`, the explicit prefix wins.
- Once the parser locks onto a region, render a small flag or country code on the left side of the field as passive feedback. It is not a button. If the user wants to override the region, they type a leading `+` or `00` and the parser follows.
- The IP lookup runs once on page load. If it fails or is blocked, fall back to `CH` as the default region (launch market). No modal, no error.
- A "Mauvais pays ?" / "Wrong country?" link sits underneath the field as a safety valve, in case the IP hint is wrong and the user does not have an international prefix in muscle memory. Clicking it reveals a region picker. This is the secondary path, not the primary one.
- Submitted value is always E.164 (e.g. `+41791234567`). No spaces, no dashes, no leading zeros, no formatting characters.

Edge cases:

- Pasting from another app: the formatter normalizes whatever it sees (spaces, dashes, parentheses, non-breaking spaces, full-width digits).
- Ambiguous local input where multiple countries share a national format: prefer the IP-hinted region until the user types `+` or `00`.
- Input that parses but fails libphonenumber's `isValidNumber` check: inline error after the user pauses typing, not on every keystroke.
- Mobile keyboard: input mode `tel`, autocomplete `tel`, so the OS shows a numeric keypad and offers contact-card autofill.

### Email capture

Right after phone verification succeeds, before the flow-specific handoff, present an optional email field:

> **Restez au courant.**
> Laissez votre email pour recevoir nos actualités produit et l'invitation au lancement. Vous pouvez vous désinscrire à tout moment.

Rules:

- Optional, never blocking. A clearly visible "Plus tard" / "Skip" affordance must let the user continue without entering anything.
- One field, validated client-side, submitted with the same record as the phone verification.
- Single-opt-in for the test phase. No double-opt-in flow on this page; the first newsletter send handles consent confirmation.
- Stored alongside the existing recorded fields (see Technical requirements).

Edge cases the design must cover:

- Invalid number format. Inline validation, do not block submit until the number is well-formed.
- Code wrong or expired. Clear error, obvious retry path.
- Number already registered. Treat as success, route to confirmation.
- Resend exhausted. Fallback link to contact the team.
- Poor mobile data. Skeleton states, never a spinner-only screen.

Privacy copy near the form:

> Votre numéro reste en Suisse. Pas de partage avec des plateformes ou des tiers. Vous pouvez tout effacer sur demande.

## Visual and tone direction

- **Mood**: calm, professional, discrete. Not startup, not wellness app, not luxury escort. Closer to a Swiss financial product crossed with a quiet messaging tool.
- **Palette**: dark, muted base. One restrained accent color. No neon, no aggressive gradients.
- **Typography**: a serif for the hero headline (warmth and authority), clean sans for body. French diacritics must render correctly.
- **Imagery**: phone mockups showing WhatsApp threads. No faces, no bodies. Avoid stock photography of people.
- **Density**: tight. The page should feel like one short read, not a scroll marathon.
- **Motion**: minimal. Honest transitions between steps. No parallax, no auto-playing video.

## Technical requirements

- Mobile first. Most users will land from a WhatsApp link.
- French primary, English, German, Spanish selectable from a small footer switcher. Default by browser locale, fallback to English
- Performance: largest contentful paint under one second on a 4G phone in Geneva.
- No third-party trackers in the hero load. Defer analytics past first interaction if used at all.
- Accessibility at the AA level of the Web Content Accessibility Guidelines. Phone input keyboard-reachable and screen-reader friendly. One-time-code autofill enabled.
- Recorded fields: phone in E.164 format, chosen flow, locale, referrer, timestamp, optional email plus newsletter opt-in flag. Nothing else without explicit consent.
- Hosted on a Swiss-friendly platform. Domain to be decided.

## Brand

Use **Scarlot** as the visible name on the page. For this test phase Scarlot is treated as a codename, not a finalized public brand: design choices should not lock the wordmark, color, or typography in a way that would be expensive to redo if the name changes after launch. No registered-trademark notation, no logo lockup that requires a redesign to swap.

## Out of scope for this spec

- The agent experience after onboarding (Flow 1 pairing UI past handoff, Flow 2 first messages). Separate specs.
- Payments and subscription. This is a discovery test, not a paid funnel.
- Marketing pages (about, blog, pricing). Single landing page only.
- A/B copy testing harness. Keep one variant; learn from follow-up calls and conversations.

## Resolved decisions

- **Flow naming**: "Mode intégré" (Flow 1) and "Mode contact" (Flow 2). Numbers internal only.
- **Email**: optional capture after phone verification, for product newsletter and launch invitation.
- **Brand**: Scarlot as a codename. Page uses the name without committing to a final wordmark.


# Next Interview - Axes & Open Questions

Living document. Updates after each new interview record.
Pairs with the canonical guide at `~/.claude/skills/scarlot-interview/references/interview-guide.md` (méthodologie, Mom Test, question pool) and the assumption log at `references/context.md`.

This file is "what to clarify next." It does not replace the methodology, it focuses it.

Last updated after: INT10 JUSTYNA (2026-04-21). Total interviews: 10 / 8 unique participants.

---

## 1. Priority axes for the next session

Ordered by how much they would shift product direction if resolved.

1. **The BMG community group: structure, scope, inclusion.**
   Probe with any BMG subscriber. Size (members), moderation rules, geographic scope, criteria for inclusion, how the group relates to the BMG product itself. Becomes the most important ecosystem artefact in the cohort and we still have no structural description. Closes A1 + sharpens P5 + P7.

2. **Block taxonomy: do 4-5 categories cover >90% of blocks?**
   JUSTYNA's instinct-based taxonomy maps to gut-bad / interrogator / lowballer / lonely / vulgar. Convergent with PITI ("perdus / kiffeurs / gros cons"), GABRIELLE ("fantasmeur"), CAROLL ("chainwasher"). If the next user spontaneously names ~the same categories, we have a fixed-taxonomy triage primitive for the POC.

3. **Commitment-signal alternatives to cash deposits.**
   JUSTYNA's Uber-order pattern is novel (INT10 Gold). Probe Revolut, TWINT, Bolt, Uber, anything client-identity-linked. Is this a cohort pattern or one user's invention? Anchors candidate problem P19 as a real primitive.

4. **Outcall safety check-in protocol.**
   Direct probe: does anyone know where you are during an outcall? AVRIL (INT5) had a partner check-in system. JUSTYNA had only the Revolut/Uber gate (silence on live-session protocol). The Morocco cautionary tale she told was an opening that was not taken. This is a recurring silence and must be resolved.

5. **P1 (messaging fragmentation): final probe before formal demotion.**
   10 interviews, zero pain signal. Ask once more, directly: "do you ever wish you could see all your messages from all apps in one place?" If the next interview also returns zero, demote P1 from the live priority stack and update `priority-evolution.md`.

6. **The greylist + blacklist concept (test as design idea, not as feature pitch).**
   Test whether a single "mark this contact" action with a severity dial (private → community local → broader) is intuitive. From INT10 contradiction: JUSTYNA treats personal block and collective post as one event differing only in severity. Validates Scarlot's structural insight that CRM and blacklist share one data model.

7. **AI triage boundary (the "passive yes / active no" line).**
   GABRIELLE INT8 articulated a clear line: AI for safety analysis is acceptable, AI for client conversation is not. Test with less technical interviewees. Is the boundary universal or power-user specific? Anchors A7.

---

## 2. Live assumption status (where each one stands)

| ID | Assumption | Status | What's still missing |
|----|-----------|--------|----------------------|
| A1 | Collective blacklist contribution | SUPPORTED (strongest) | Structural description of the BMG group. Quantify: % of BMG subscribers in the chat. |
| A2 | Phone number as stable unique ID | PARTIAL (confirmed as primary key) | Recycling / spoofing risk not directly tested. Probe whether anyone has been burned by a recycled number. |
| A3 | Co-founder trust transfers to platform | PARTIAL | No clean test yet. Joséphine's name needs to be introduced as a trust anchor (separately from the interviewer) and the response observed. |
| A4 | No-UI / conversational interface preferred | MIXED | Power-user GABRIELLE prefers structured tools (databases). Test this preference with non-power-users: "if there were a separate app for this, would you install it?" |
| A5 | Beta users representative of CH market | CHALLENGED (enriched) | Sample now includes Geneva power users + Lausanne low-volume + cross-border periodic + non-native speakers. Question: are we missing the agency-tied or DE-CH segments? |
| A6 | Willingness to self-KYC | PARTIAL | No direct probe. Test: "if a tool asked for an ID selfie at signup, would you do it? Why or why not?" |
| A7 | AI triage acceptable with user control | PARTIAL (boundary clarified by INT8) | Test the passive/active boundary with less technical users. |
| A8 | Willingness to pay at meaningful levels | CONFIRMED (behavioral) | Direct Scarlot-price ask still missing. Anchor: "if a tool replaced your BMG subscription for half the price, would you switch?" |
| A9 | Collective blacklist legally defensible | OPEN | Not addressable through interviews. Swiss legal counsel required pre-launch. |
| A10 | nFADP compliance achievable | OPEN | Not addressable through interviews. Swiss legal counsel required pre-launch. |

---

## 3. Candidate problems needing a second source

Three problems surfaced as Gold/Silver in single interviews. Each needs an unprompted second source before being added to the formal stack.

| ID | Candidate problem | Source | Test |
|----|-------------------|--------|------|
| P17 | Platform identity mismatch / forced categorization (especially for trans TDS) | INT8 GABRIELLE Gold | Probe trans interviewees: "have you ever felt the platforms force you into a category that doesn't fit?" |
| P18 | Client reputation threats / revenge as a weapon | INT8 GABRIELLE Silver | Direct probe: "has a client ever threatened to damage your reputation? What happened?" |
| P19 | Commitment-signal-as-screening (Uber order, Revolut deposit, ride-hail identity) | INT3 / INT6 / INT8 / INT10 | Already 3-source convergent. One more source moves this from candidate to formal P19. |

---

## 4. Structural gaps in the cohort

Things missing across all 10 interviews that would change product priorities if surfaced.

- **Agency-tied workers** (salons, studios). Sample is 100% independent. Are pain points different? Does the same product fit, or is there a B2B agency variant?
- **DE-CH workers** (Zurich, Basel). GABRIELLE has cross-regional visibility but is FR-CH primary. F-Girl's DE-CH dominance is unrepresented in our user data.
- **Non-Swiss-resident workers operating in Switzerland.** CAROLL is the only periodic / cross-border voice. The 90-day permit segment (flagged INT8) is a structural vulnerability worth a dedicated probe.
- **Reverse onboarding signal: someone who tried Scarlot-adjacent tools and dropped them.** Failure interviews would calibrate adoption friction.
- **A direct price probe.** No one has been asked "would you pay X CHF/mo for this?" Until someone is, A8 remains behavioral-only.

---

## 5. Closed-out axes (do not re-probe unless triggered)

These have been resolved enough to stop asking unprompted.

- **P1 messaging fragmentation as severe pain** - 10 interviews zero signal. One last probe scheduled (axis #5 above), then formally demote.
- **Whether power users want a separate UI** - GABRIELLE made it clear (structured tools yes, but conversation must stay human). Re-probe only if a non-power-user disagrees.
- **Whether mood-dependent reporting is universal** - GABRIELLE INT6/INT8 explicit; converges with most interviewees' implicit pattern. Treat as design constraint, not open question.

---

## How to use this file

After each interview record (`SCARLOT_INT_*.md`):

1. Open the new record's "What was not said" section and "For next interview" suggestions.
2. Add new axes to §1 if they would shift product direction.
3. Move resolved axes to §5.
4. Update assumption status in §2 if the new evidence changed it.
5. Update candidate problem signal counts in §3.
6. Bump the "Last updated" line at the top.

This file should never grow indefinitely. If §1 has more than 10 items, the discovery program is unfocused and needs a synthesis pass before the next session.

---
record_id: SCARLOT_INT_20260319_GABRIELLE
pseudonym: GABRIELLE
date: 2026-03-19
language: FR/EN
---

**Record ID:** SCARLOT_INT_20260319_GABRIELLE
**Pseudonym:** GABRIELLE
**Date:** 19/03/2026
**Duration:** ~60 minutes (estimated)
**Language:** FR/EN mixed
**Record Created:** 23/03/2026
**Note:** Second session with GABRIELLE. First was written
                    questionnaire (SCARLOT_INT_20260305_GABRIELLE).
                    This in-person interview reveals significantly
                    deeper technical and personal context.

## 01. Interviewee Profile

**Work model:** Independent (registered - raison individuelle)
**Location:** Western Switzerland
**Experience:** ~3–5 years active (3yr stated here; 5yr in INT6)
**Primary channel:** WhatsApp Business (90%), Telegram (client-initiated), SMS, calls
**Tools used:** WhatsApp Business (templates, tags, slash commands), Notion (DB - former), Airtable (former), Excel (former), Claude Code (learning tool), custom Rust tooling (in dev), personal server (self-hosted), SQL databases, BMG (lost access)
**Platform presence:** F-Girl, And6, xdate, Tryst
**Phone OS:** iOS (1 phone, 2 e-SIM)
**Notable context:** Trans woman TDS. Self-taught programmer (1 year: SQL, Rust, JS/TS). Built blacklist scraper + Airtable aggregation system. Building own data tools from scratch. Data-obsessed - tracks revenue, seasonality, client grades. Most technically sophisticated interviewee in the cohort by a wide margin. First interview conducted by advisor alone (no co-founder). Previously known for pre-recorded message triage and WhatsApp label CRM (INT6) - this session reveals the full depth of her technical capabilities.

## 02. Top Signals - Ranked By Evidence Quality

> [!warning] Signal 1 - 🥇 Gold - WORKAROUND
> **Statement:** "Created a DB with an API, gathering data from scraping, depends on the source, i.e. And6 etc. the scraper would collect them to add them in Airtable. Now starting from scratch." [PARAPHRASE]
>
> **Behaviour:** Built an automated scraper to aggregate blacklist data from multiple platform sources into a single Airtable database via API. Project abandoned due to scale; now rebuilding from scratch in Rust.
>
> **Frequency:** Ongoing project - started, abandoned, restarting.
> **Intensity:** Matter-of-fact / determined. Not frustrated - solving it herself.
> **Connects to:** P5 (no centralised blacklist), P15 (data siloing across platforms)
> **Implication:** A user independently attempted to build what Scarlot's C-01 (shared blacklist network) proposes. This is the strongest possible validation of the problem - she invested significant personal time and technical effort. The fact that she abandoned due to scale (not lack of need) confirms the problem requires collective infrastructure, not solo effort.

> [!warning] Signal 2 - 🥇 Gold - WORKAROUND
> **Statement:** "Tracking all the meetings, when, who, how much, how did it go, grades, then I could have a visualisation... Faire des fiches client c'est super, 1/4 of my data are actually customer records." [PARAPHRASE]
>
> **Behaviour:** Built a full client tracking database in Notion: meeting dates, client identity, revenue, quality grades, with visualization. 1/4 of her data is customer records.
>
> **Frequency:** Systemic - ongoing practice across all client interactions.
> **Intensity:** Delighted / matter-of-fact. Sees this as essential.
> **Connects to:** P2 (no client memory), P16 (no income tracking), C-03 (client intelligence profile)
> **Implication:** Most advanced self-built CRM in the cohort. Confirms P2 as a real need AND demonstrates what the solution looks like when a technically capable user builds it herself. Her schema (who, when, how much, grade) is a direct design input for C-03.

> [!warning] Signal 3 - 🥇 Gold - PAIN
> **Statement:** "When I am in a bad mood and there are no shows, I blacklist. When I am in a good mood I don't" [PARAPHRASE]
>
> **Behaviour:** Blacklisting behaviour is mood-dependent and inconsistent. Only systematic for severe offenses (rape, pedophile content). For no-shows, reporting depends on emotional state.
>
> **Frequency:** Recurring - multiple blacklist submissions mentioned.
> **Intensity:** Resigned. Accepts the inconsistency as normal.
> **Connects to:** P5 (no centralised blacklist), P6 (no-shows & time-wasters), A1 (collective blacklist contribution)
> **Implication:** Critical design insight for C-01: reporting friction must be near-zero to capture signals that currently get lost to mood/fatigue. Graduated severity (auto-flag no-shows vs. manual report for violence) would match natural behaviour.

> [!warning] Signal 4 - 🥇 Gold - PAIN / NEW SIGNAL
> **Statement:** "I want to get out of fgirl because I don't like how I am categorized, I am blocked in an imaginary of woman trans from the 90s which is totally outdated or doesn't correspond me at all... get out of the 'male gaze' forcing me to do and be what I am not" [PARAPHRASE - close to verbatim]
>
> **Behaviour:** Actively seeking to leave F-Girl. Exploring broadcast channels, personal branding, Tryst as alternatives. Wants to control her own narrative.
>
> **Frequency:** Ongoing strategic shift - not a one-time complaint.
> **Intensity:** Frustrated / determined. High emotional charge.
> **Connects to:** NEW SIGNAL - Platform identity mismatch / forced categorization
> **Implication:** Platforms impose identity categories that don't match the user's reality, especially for trans TDS. New problem not in the 16-problem inventory. "je veux ma propre épicerie" - she wants to own her brand. Adjacent to P10 (fake listings / AI-generated photos) but distinct: this is about agency over self-presentation.

> [!info] Signal 5 - 🥈 Silver - BEHAVIOUR
> **Statement:** "checks are done based on experience, cognition and experience and exchange. I don't check systematically but it would be amazing if it was automatic" [PARAPHRASE]
>
> **Behaviour:** Screening is intuition-based, not systematic. Does not routinely check blacklists for new contacts. Relied on asking in a group when she lost BMG access.
>
> **Frequency:** Habitual non-checking. Only checks when something feels off.
> **Intensity:** Matter-of-fact.
> **Connects to:** P3 (no inbound filtering), P5 (no centralised blacklist)
> **Implication:** Even the most technically sophisticated user in the cohort doesn't check systematically. Confirms that checking friction is too high - the solution must be passive/automatic.

> [!info] Signal 6 - 🥈 Silver - BEHAVIOUR
> **Statement:** "In the no code app, I was collecting the bank notification via email, so I could track the entry and exits of money" [PARAPHRASE]
>
> **Behaviour:** Built automated financial tracking by parsing bank email notifications into Airtable. Uses multiple banks (BCV, BKB, Postfinance). Wants to build a Mint-like aggregator.
>
> **Frequency:** Was a working system; now rebuilding.
> **Intensity:** Delighted / ambitious.
> **Connects to:** P16 (no income tracking), C-07 (revenue tracker)
> **Implication:** First real behavioral signal for P16 and C-07. She actually built and used income tracking. The bank-notification-parsing approach is a design insight.

> [!info] Signal 7 - 🥈 Silver - PAIN
> **Statement:** "But there are multiple lists and it's a bit cumbersome" [PARAPHRASE]
>
> **Behaviour:** Has reported to blacklists multiple times but finds the multi-list landscape fragmented and effortful.
>
> **Frequency:** Systemic - ongoing friction across multiple reporting attempts.
> **Intensity:** Resigned.
> **Connects to:** P5 (no centralised blacklist), P15 (data siloing across platforms)
> **Implication:** Even someone who actively contributes finds the fragmentation cumbersome. Confirms aggregation value prop.

> [!info] Signal 8 - 🥈 Silver - PAIN
> **Statement:** "Notion has AI everywhere, is expensive and have a full access to your data and can decide to suspend your account" [PARAPHRASE]
>
> **Behaviour:** Left Notion due to cost, AI integration concerns, data sovereignty fears, and account suspension risk. Moved to open source and self-hosting.
>
> **Frequency:** One-time migration, but reflects persistent concern.
> **Intensity:** Frustrated - trust issue with platforms.
> **Connects to:** A10 (nFADP compliance achievable), NEW SIGNAL - data sovereignty as hard requirement
> **Implication:** TDS have heightened data sovereignty concerns due to risk of platform account suspension. SaaS platforms that can access user data or suspend accounts are unacceptable for this population.

> [!info] Signal 9 - 🥈 Silver - PAIN / NEW SIGNAL
> **Statement:** "threats to demolish reputation - or do it after the facts as an act of revenge" [PARAPHRASE]
>
> **Behaviour:** Has experienced or witnessed clients threatening to damage TDS reputation, or doing so after a meeting as revenge.
>
> **Frequency:** Mentioned as a known pattern, not a one-time event.
> **Intensity:** Concerned.
> **Connects to:** NEW SIGNAL - Client reputation threats / revenge
> **Implication:** Reputation attacks are a distinct safety concern from physical threats. May inform blacklist design (flagging retaliatory clients) and argues against visible TDS profiles in any collective system.

> [!info] Signal 10 - 🥈 Silver - BEHAVIOUR / NEW SIGNAL
> **Statement:** "90 day permit - financial pressure because not staying a long time, taking advantage of that" [PARAPHRASE]
>
> **Behaviour:** Some clients exploit the financial pressure of TDS on short-term (90-day) permits who can't afford to refuse.
>
> **Frequency:** Mentioned as a known pattern.
> **Intensity:** Concerned.
> **Connects to:** NEW SIGNAL - Permit-based vulnerability exploitation
> **Implication:** Adds a structural safety dimension. Intersects with Piti's financial precarity signal (INT4). Safety features are MORE critical for precarious users, not less.

> [!info] Signal 11 - 🥈 Silver - BEHAVIOUR
> **Statement:** "People that contact you come better prepared, so they will use time based deletion of messages" [PARAPHRASE]
>
> **Behaviour:** Clients use WhatsApp's disappearing messages to prevent
>
> **Frequency:** Mentioned as a known pattern.
> **Intensity:** Matter-of-fact.
> **Connects to:** P3 (no inbound filtering), A2 (phone number as stable unique ID)
> **Implication:** Clients actively erase traces. Auto-archiving of inbound messages before deletion would be a high-value safety feature.

> [!note] Signal 12 - 🥉 Bronze - DESIRE
> **Statement:** "It would be incredible to have more people using this, understanding which areas work better, and which time of the week, the month, seasonality" [PARAPHRASE]
>
> **Behaviour:** Expressed desire for collective data analytics - aggregated insights across TDS on location, timing, seasonality.
>
> **Frequency:** Forward-looking desire, not past behavior.
> **Intensity:** Enthusiastic.
> **Connects to:** NEW SIGNAL - Collective business intelligence
> **Implication:** Bronze (hypothetical) but interesting: she envisions a data cooperative, not just a safety tool. V2+ opportunity.

## 03. Workarounds Inventory

> [!example] Workaround: Blacklist scraper + Airtable DB aggregation system
> **Problem it solves:** P5 (no centralised blacklist), P15 (data siloing across platforms)
> **Why it's inadequate:** Scale was unmanageable solo. Abandoned. Restarting from scratch with new tech stack.
> **Maps to concept:** C-01 (shared blacklist network) - she literally built a prototype of it


> [!example] Workaround: Notion database for client tracking (who, when, how much, grade, outcome)
> **Problem it solves:** P2 (no client memory), P16 (no income tracking)
> **Why it's inadequate:** Notion is expensive, has data sovereignty issues, could suspend account. Now migrating to self-hosted.
> **Maps to concept:** C-03 (client intelligence profile), C-07 (revenue tracker)


> [!example] Workaround: Bank email notification parsing into Airtable for financial tracking
> **Problem it solves:** P16 (no income tracking)
> **Why it's inadequate:** Tied to Airtable (abandoned). Multi-bank complexity. Rebuilding.
> **Maps to concept:** C-07 (revenue tracker)


> [!example] Workaround: WhatsApp Business tags to categorize contacts
> **Problem it solves:** P2 (no client memory)
> **Why it's inadequate:** Tags are limited. Blocks number but doesn't archive conversation. No cross-reference with blacklists.
> **Maps to concept:** C-03 (client intelligence profile)

  Note:                 Consistent with INT6 - WhatsApp labels with
                        "fantasmeur" tag. This session adds detail on the
                        sticker/tag mechanism.

> [!example] Workaround: WhatsApp Business templates (/cava slash command) for multilingual quick replies
> **Problem it solves:** P8 (manual repetition)
> **Why it's inadequate:** Manual trigger. Only works in WhatsApp.
> **Maps to concept:** C-04 (inbound message triage)

  Note:                 Consistent with INT6 - pre-recorded messages. This
                        session reveals the specific mechanism (WAB slash
                        commands, multilingual).

> [!example] Workaround: Asking in community groups to verify a client
> **Problem it solves:** P5 (no centralised blacklist), P3 (no inbound filtering)
> **Why it's inadequate:** Depends on group responsiveness. Lost BMG access. Fragmented across groups.
> **Maps to concept:** C-01 (shared blacklist network)


> [!example] Workaround: 2 e-SIMs on 1 phone for work/personal separation
> **Problem it solves:** Work/life boundary
> **Why it's inadequate:** Still one device, one WhatsApp.
> **Maps to concept:** None directly


## 04. Friction Map - The Full Interaction Lifecycle

> [!abstract] Inbound
> - 90% via WhatsApp. Also SMS, calls, Telegram (client-initiated).
> - Uses WAB templates (/cava) for initial multilingual responses.
> - Screening is intuition-based ("experience, cognition, exchange").
> - Does NOT systematically check blacklists for new contacts.
> - Clients use disappearing messages to erase traces.
> - Friction: No passive safety check. Manual group queries when
> - something feels off. Lost BMG access compounds the problem.

> [!abstract] Screening
> - Evaluates based on conversation quality: "when a customer isn't
> - simple."
> - Green flags: books, sends money, honours meeting.
> - Red flags: repeated cancellations, excessive negotiation,
> - incessant messaging, overcautious behavior.
> - 3 weeks ago: outcall, client had canceled twice before, was
> - negotiating taxi costs, writing incessantly. Asked community
> - group because lost BMG access. Session went fine - "just the
> - stressed type."
> - Friction: Entirely manual and subjective. Recent incident
> - required group query as fallback.

> [!abstract] Pre-Booking
> - Uses WhatsApp Business templates for initial replies.
> - Appointment details tracked in Notion DB.
> - Friction: No specific booking friction surfaced.

> [!abstract] Safety Check
> - For outcalls: shares info in community groups ("I'm going
> - there, here are the information").
> - Friction: Depends on group availability. No systematic protocol
> - beyond group notification.

> [!abstract] Post-Appointment
> - Records client in Notion DB with grade and outcome details.
> - 1/4 of her data is client records.
> - Blacklists no-shows inconsistently (mood-dependent).
> - Always reports severe offenses (rape, pedophile content -
> - would report to police).
> - Friction: Multiple fragmented blacklists. Mood-dependent
> - reporting = many no-shows go unreported.

> [!abstract] Financial
> - Built bank notification parser (email → Airtable) for income
> - tracking. Tracks revenue, costs, taxes.
> - Analyzes seasonality (Jan/Oct strong, March weak, Ramadan
> - impact, dominatrix counter-cyclical in March).
> - Factors: number of customers × revenue per customer.
> - Wants Mint-like aggregator across banks (BCV, BKB,
> - Postfinance).
> - Friction: Previous system abandoned with Airtable. Rebuilding
> - from scratch in Rust.

## 05. Assumption Tracker

> [!success] A1 - Collective blacklist contribution
> **Status:** CONFIRMS (with nuance)
> **Evidence:** Built a blacklist aggregation system (Gold behavioral). Reports to blacklists "multiple times." But reporting is mood-dependent for no-shows - only consistent for severe offenses. Shares info in groups for safety checks.
> **Confidence:** PROBABLE
> **Note:** Strongest behavioral confirmation yet - she tried to BUILD the collective infrastructure. But mood-dependent reporting = design constraint: contribution friction must be near-zero.

> [!success] A2 - Phone number as stable unique ID
> **Status:** CONFIRMS (partial)
> **Evidence:** Her blacklist aggregation was "mostly by phone number, then the other aspects." Phone number is the primary key in her data model. No spoofing concerns raised.
> **Confidence:** PROBABLE
> **Note:** Clients using disappearing messages is related - they protect anonymity through message deletion, not number spoofing.

> [!question] A3 - Co-founder's trust transfers to Scarlot
> **Status:** NO SIGNAL
> **Evidence:** Interview conducted by advisor without co-founder. No discussion of Scarlot or trust dynamics.
> **Confidence:** UNKNOWN

> [!danger] A4 - No-UI / conversational interface preferred
> **Status:** CONTRADICTS (partially)
> **Evidence:** Builds database-driven tools with structured queries. Uses WAB power features (templates, tags). Her data journey (spreadsheet → Notion → Airtable → custom Rust DB) is about structured data interfaces, not conversational ones. But: "answering customers: I am not sure it would be useful... it's important to be there and human."
> **Confidence:** POSSIBLE
> **Note:** Most technically sophisticated user in the cohort - may not represent the typical user for A4. But her preference for structured tools over conversational ones is genuine signal.

> [!success] A5 - Beta users represent the broader CH population
> **Status:** CONFIRMS (enriches)
> **Evidence:** Swiss-registered, raison individuelle, trans woman. Active in ZH (German-speaking) and FR-CH groups + Brussels. Cross-regional visibility unlike most prior interviewees.
> **Confidence:** POSSIBLE

> [!question] A6 - Willingness to self-KYC
> **Status:** NO SIGNAL
> **Evidence:** Mentions xdate requiring proof ("you have to fight to prove you're real") - familiarity but no direct willingness signal.
> **Confidence:** UNKNOWN

> [!warning] A7 - AI triage acceptable with user control
> **Status:** MIXED (clarifies the boundary)
> **Evidence:** WANTS automatic safety checks: "it would be amazing if it was automatic." REJECTS AI conversation: "it's important to be there and human," "not sure AI can replace the conversation," "not sure about AI would help to determine which ones are time wasters."
> **Confidence:** PROBABLE
> **Note:** Clearest articulation of the semi-auto boundary in the cohort. Background screening = yes. Auto-reply = no. Consistent with INT6 where her pre-recorded message system is human-initiated, not AI-driven.

> [!success] A8 - Willingness to pay at meaningful levels
> **Status:** CONFIRMS (behavioral)
> **Evidence:** Paid for Airtable (~50 CHF/mo per co-founder context). Paid for Notion. Bought a server. Tryst ~20 CHF/mo for touring. On F-Girl, And6, xdate (likely paid listings). Rejected Blink as "super expensive."
> **Confidence:** PROBABLE
> **Note:** Strongest WTP behavioral evidence in the cohort. Has actually paid for multiple tools. Ceiling seems around 50 CHF/mo but exact Airtable duration not captured.

> [!question] A9 - Collective blacklist legally defensible under Swiss law
> **Status:** NO SIGNAL
> **Evidence:** No discussion of legal concerns around blacklisting.
> **Confidence:** UNKNOWN

> [!question] A10 - nFADP compliance achievable
> **Status:** NO SIGNAL (but related signal)
> **Evidence:** Explicit data sovereignty concern about Notion: "have a full access to your data and can decide to suspend your account." Moved to self-hosted infrastructure.
> **Confidence:** UNKNOWN
> **Note:** Her data sovereignty posture suggests Scarlot's data practices will be scrutinized by power users.

## 06. New Signals - Not In Prior Research

> [!warning] Platform identity mismatch / forced categorization for trans TDS. "I am blocked in an imaginary of woman trans from the 90s." "Get out of the male gaze."
> **Type:** New problem (candidate P17)
> **Tier:** 🥇 Gold (unprompted, specific, emotional, behavioral - actively leaving F-Girl)
> **Priority:** Medium - outside current scope but affects platform choice
> **Implication:** Listing platforms force trans TDS into outdated categories. "je veux ma propre épicerie." If Scarlot ever builds a profile component, identity autonomy is non-negotiable.

> [!info] Client reputation threats / revenge as a weapon. "Threats to demolish reputation - or do it after the facts as an act of revenge."
> **Type:** New problem (candidate P18)
> **Tier:** 🥈 Silver (described as known pattern, not specific incident)
> **Priority:** Medium - intersects with blacklist design and trust
> **Implication:** Blacklist could include "retaliatory client" flags. TDS profiles in any collective system must not expose TDS to counter-retaliation.

> [!info] Permit-based vulnerability exploitation. "90 day permit - financial pressure, taking advantage of that."
> **Type:** New constraint
> **Tier:** 🥈 Silver (described as known pattern)
> **Priority:** Medium - intersects with Piti's financial precarity signal
> **Implication:** Short-term permit holders face structural exploitation. Safety features most critical for most vulnerable users.

> [!info] Clients use disappearing messages to erase evidence.
> **Type:** New constraint
> **Tier:** 🥈 Silver (observed pattern)
> **Priority:** High - affects C-01 and C-03 data integrity
> **Implication:** Message auto-archiving before deletion = safety feature. Bot should retain safety-relevant data even when client deletes messages.

> [!info] Data sovereignty as hard requirement for TDS tools. Notion rejected because "can decide to suspend your account." Self-hosts instead.
> **Type:** New constraint
> **Tier:** 🥈 Silver (specific past behavior - left Notion)
> **Priority:** High - affects architecture
> **Implication:** SaaS platforms that can suspend accounts or access data are unacceptable. Scarlot must address data ownership.

> [!note] Collective business intelligence desire - aggregated analytics on location, timing, seasonality.
> **Type:** New opportunity
> **Tier:** 🥉 Bronze (forward-looking)
> **Priority:** Low - V2+
> **Implication:** Data cooperative model. She tracks seasonality herself (Jan/Oct strong, March weak, Ramadan impact, dominatrix counter-cyclical). Multi-user anonymized data could surface market insights.

> [!info] Tryst as trans-positive platform alternative.
> **Type:** New ecosystem
> **Tier:** 🥈 Silver (she uses it, 1 client from it)
> **Priority:** Low - ecosystem awareness
> **Implication:** Tryst: free base, ~20 CHF/mo touring, good comms, trans- positive, good UX. Low CH traffic. Add to ecosystem table.

> [!info] WhatsApp Business power features as existing TDS infrastructure. Templates with slash commands (/cava), multilingual, tag filtering.
> **Type:** New workaround detail
> **Tier:** 🥈 Silver
> **Priority:** High - design input
> **Implication:** WAB templates and tags already do primitive CRM + auto- reply for sophisticated users. Scarlot should extend these patterns, not replace them.

## 07. Contradictions & Surprises

> [!danger] Contradiction
> **Expected:** A technically sophisticated user who built a blacklist aggregation system would systematically check blacklists for new contacts.
> **Found:** She does not check systematically. Screening is intuition-based.
> **Possible explanations:** Checking friction across fragmented lists is so high that even someone who understands the value defaults to intuition unless something feels off. The aggregation project was precisely an attempt to fix this - its abandonment left her back to manual intuition.
> **Investigate next:** If even the most technically capable user defaults to intuition, passive/ automatic checking may be the ONLY viable approach for anyone.

  ---

> [!danger] Contradiction
> **Expected:** A data-obsessed, technically capable user would strongly desire AI automation for all workflows.
> **Found:** She explicitly rejects AI for customer conversation but wants automatic background safety checks. Clear line between passive AI (yes) and active AI (no).
> **Possible explanations:** She draws a precise boundary: AI for safety analysis (background, passive) = acceptable. AI for human interaction (conversation, active) = unacceptable. "c'est important d'être là pour des gens qui ne sont pas des trous du cul" - being human IS the service.
> **Investigate next:** Is this semi-auto boundary universal, or specific to power users? Test with less technical interviewees.

  ---

> [!danger] Contradiction
> **Expected:** INT6 (written questionnaire) painted a complete picture of GABRIELLE.
> **Found:** INT6 captured maybe 20% of her reality. The blacklist scraper, Rust programming, financial tracking, data sovereignty concerns, platform identity frustrations, seasonality analysis - none surfaced in the questionnaire.
> **Possible explanations:** Written questionnaires constrain depth. In-person conversation with a technical interviewer unlocked a completely different level of signal.
> **Investigate next:** This validates the need for in-person follow-ups with all questionnaire participants who showed interesting signals.
## 08. What Was Not Said

> [!question] Silence on P1 (messaging fragmentation)
> She uses multiple channels (WhatsApp 90%, Telegram, SMS, calls) but expressed no pain about fragmentation. Consistent with the cohort trend - 8 interviews, zero signal. P1 may not be a real problem.


> [!question] Silence on P11 (manual calendar / booking)
> Despite tracking everything in databases, she mentioned no booking or scheduling friction. Either her system handles it or it's not a pain point.


> [!question] Silence on trans-specific safety risks
> Section 5 (Trans-Specific Experience) has no captured notes. Either not explored or she didn't engage. Given the richness of her signals elsewhere, this is likely a gap in the interview. GABRIELLE is one of two trans interviewees (with PITI) - this perspective remains under-represented. Must be explored in follow-up.


  Silence on P12 (agency dependency) is expected: She is fully independent
  and registered as raison individuelle. Agency dependency is not her
  problem.

## 09. User Stories - Inferred From This Interview Only

> [!tip] US-INT8-01 - 🥇 Gold
> As an independent TDS receiving 90% of client contact via WhatsApp,
> I want incoming numbers automatically checked against known blacklists,
> so that I don't have to manually query community groups when something
> feels off.
> via groups; "it would be amazing if it was automatic."
>
> **Evidence:** Built blacklist aggregation system; still checks manually
> **Tags:** #safety #blacklist

> [!tip] US-INT8-02 - 🥇 Gold
> As a data-driven TDS tracking meetings, revenue, and client grades,
> I want a persistent client record that captures interaction history,
> so that I can see patterns and understand my business.
> "1/4 of my data are actually customer records."
>
> **Evidence:** Built Notion DB with meeting records, grades, revenue.
> **Tags:** #crm #financial

> [!tip] US-INT8-03 - 🥇 Gold
> As a TDS contributing to multiple blacklists after bad experiences,
> I want a single place to report,
> so that I don't have to submit to multiple fragmented lists.
> multiple lists. Mood-dependent reporting for no-shows.
>
> **Evidence:** Reports to blacklists but finds it "a bit cumbersome" across
> **Tags:** #safety #blacklist

> [!tip] US-INT8-04 - 🥈 Silver
> As a TDS who tracks revenue and analyzes seasonality,
> I want automated income tracking from bank notifications,
> so that I understand my financial patterns without manual data entry.
> Mint-like aggregator. Tracks seasonality.
>
> **Evidence:** Built bank notification parser (email → Airtable). Wants
> **Tags:** #financial

> [!tip] US-INT8-05 - 🥇 Gold
> As a trans TDS forced into outdated categories on listing platforms,
> I want to control how I present myself to potential clients,
> so that I attract people who desire me for who I am, not a stereotype.
> "je veux ma propre épicerie."
>
> **Evidence:** Actively leaving F-Girl. "Get out of the male gaze."
> **Tags:** #identity #platform

## 10. Willingness To Pay - Signals

> [!warning] Paid for Airtable subscription
> **Type:** Existing spend reference
> **Tier:** 🥇 Gold (actual past spend)
> **Anchor:** ~50 CHF/month (per co-founder context)
> **Notes:** Left Airtable - unclear if price or functionality drove switch. Duration not captured.

> [!info] Paid for Notion subscription
> **Type:** Existing spend reference
> **Tier:** 🥈 Silver (mentioned leaving due to cost + sovereignty)
> **Anchor:** Not specified
> **Notes:** "Expensive" - price was a factor alongside data concerns.

> [!warning] Bought and maintains a personal server
> **Type:** Commitment signal (capital investment + ongoing effort)
> **Tier:** 🥇 Gold (significant investment of time and money)
> **Anchor:** Not specified, but server hardware + hosting is non-trivial
> **Notes:** Strongest commitment signal: investing real resources into self-hosted infrastructure. Proxy WTP - she'd pay for a tool that did what she's building herself.

> [!info] Tryst touring subscription
> **Type:** Existing spend reference
> **Tier:** 🥈 Silver
> **Anchor:** ~20 CHF/month
> **Notes:** Moderate spend on a platform she likes but gets little traffic from.

> [!info] Rejected Blink as "super expensive"
> **Type:** Price ceiling signal
> **Tier:** 🥈 Silver
> **Anchor:** Unknown (above her threshold)
> **Notes:** Indicates a ceiling - not unlimited willingness to pay.

## 11. Verbatim Quotes - Gold & Silver Only

> [!quote] "je veux ma propre épicerie"
> **Tier: 🥇  |  Connects to: NEW SIGNAL - Platform identity mismatch**
> **Context:** Discussing her desire to leave F-Girl and control her brand.

> [!quote] "c'est important d'être là pour être pour des gens qui ne sont pas des trous du cul"
> **Tier: 🥈  |  Connects to: A7 (AI triage acceptable with user control)**
> **Context:** Explaining why human presence in conversation matters.

> [!quote] "When I am in a bad mood and there are no shows, I blacklist. When I am in a good mood I don't"
> **Tier: 🥇  |  Connects to: P5 (no centralised blacklist), A1 (collective**

  blacklist contribution)
  Context: Describing her inconsistent blacklist reporting behavior.

> [!quote] "I don't check systematically but it would be amazing if it was automatic"
> **Tier: 🥈  |  Connects to: P3 (no inbound filtering)**
> **Context:** Describing her approach to screening new contacts.

> [!quote] "I am blocked in an imaginary of woman trans from the 90s which is totally outdated or doesn't correspond me at all"
> **Tier: 🥇  |  Connects to: NEW SIGNAL - Platform identity mismatch**
> **Context:** Explaining why she wants to leave F-Girl.

> [!quote] "Faire des fiches client c'est super, 1/4 of my data are actually customer records"
> **Tier: 🥈  |  Connects to: P2 (no client memory), C-03 (client**

  intelligence profile)
  Context: Describing her Notion-based client tracking system.

## 12. Recommended Next Actions

  For the interviewer:
  Strong session with a unique depth of technical and personal signal.
  The prep document clearly guided the conversation well - the blacklist
  aggregation and data journey blocks yielded the richest signals. First
  interview conducted by advisor alone (no co-founder) - produced clean
  evidence without social desirability bias. The closing section yielded
  rich unprompted signals (identity, reputation threats, permit
  exploitation, "je veux ma propre épicerie").

  Next time, try:
  - Section 5 (trans-specific experience) was not captured - this is the
    most important gap. GABRIELLE is one of two trans interviewees. Even
    if conversation flows elsewhere, return to this topic explicitly.
  - Probe deeper on BMG: she "lost access" - cost, technical, or other?
    Directly informs P7 (blacklist paywalled / BMG).
  - Capture Airtable pricing precisely: exact amount, duration, what
    broke, what was worth it. Rare WTP behavioral data.
  - Ask about her Rust tool plans - her spec IS the user need articulated
    by a technical user.
  - Audio recording (with consent) would significantly improve verbatim
    quote capture. Notes lose nuance.

  For the co-founder:
  - GABRIELLE is a potential power user AND technical collaborator. Her
    blacklist aggregation attempt, data schema, and self-hosting instincts
    are directly relevant to Scarlot's architecture.
  - Ask about the ZH community group structure - she has cross-regional
    visibility (FR-CH + ZH + Brussels) that no other interviewee has.
  - Explore whether she'd participate in a beta matching her technical
    capabilities (API access, data export, etc.).

  For next interview:
  - Test the "passive check yes / active AI no" boundary with less
    technical users. Is this universal or power-user specific?
  - Probe P1 (messaging fragmentation) explicitly - 8 interviews, zero
    signal. Time to confirm it's not a real problem or find the edge case.
  - The "reputation threat" signal (P18 candidate) needs a second source.
    Ask directly about client retaliation / revenge.
  - The "disappearing messages" signal needs validation - do other TDS
    experience this? How do they handle evidence retention?
  - Follow up on questionnaire participants with in-person sessions.
    INT6→INT8 gap proves the format difference is massive.

## 13. Researcher Notes & Bias Flags

**Interview quality:** 3.5/5
  Leading questions detected: NO - questions drawn from prep document, well-structured. Some sections under-explored (5, parts of 4) but not due to leading.
  Co-founder present bias:    N/A - co-founder not present. First interview without co-founder, which is a positive methodological step.
**Interviewee comfort level:** HIGH - spoke freely, offered detailed technical and personal information without prompting.

  What went well:
  - Prep document was effective - priority blocks structured conversation
  - Interviewer allowed participant to go deep on technical topics without
    redirecting too early
  - Closing section yielded rich unprompted emotional signals
  - No co-founder = cleaner evidence
  - Second session with same participant unlocked dramatically deeper
    signal than questionnaire format - validates in-person follow-ups
  - Participant was highly articulate and offered both past-tense
    behavioral evidence AND forward-looking vision

  Suggestions for next time:
  - Trans-specific experience block was empty - must be explored in
    follow-up for this under-represented perspective
  - Some notes are shorthand losing context ("allows to seclude" -
    unclear in WAB context). Capture more context in notes.
  - Probe pricing more precisely: exact amounts, duration, what broke.
  - Notes-based format loses verbatim richness. Audio recording would
    improve Gold signal capture significantly.

**Confidence in this record:** MEDIUM-HIGH
  Notes are detailed enough for robust signal extraction. Main
  limitations: (1) notes not full transcript - some quotes may be
  paraphrases, (2) Section 5 gap, (3) some shorthand lacks context.
  Despite this, signals are strong, specific, and behavioral. High-value
  interview that dramatically enriches GABRIELLE's profile beyond INT6.


---

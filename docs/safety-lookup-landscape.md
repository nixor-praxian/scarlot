# Safety Lookup Landscape - Competition & Substitutes

*April 2026*

Existing platforms, tools, and initiatives that offer some form of client lookup, bad client list, or safety screening for sex workers. This is the competitive landscape for Scarlot's core MVP function (Bloc 1 - Safety).

---

## Direct Competitors (Structured Lookup Tools)

### National Ugly Mugs (NUM)

- **Geography:** UK, Ireland
- **Operator:** Charity (government-funded)
- **Model:** Free for workers
- **What it does:** Collects reports of dangerous individuals from sex workers. Verifies reports through trained review. Shares alerts within a closed network. Processes ~1 million safety alerts/year. 585 violence cases logged in 2023. ~50% of recipients report avoiding a dangerous person.
- **How lookup works:** Reporting-based alert system. Workers receive warnings, not on-demand lookup of arbitrary numbers.
- **Limitations:** UK/Ireland only. Not a CRM. Not conversational. No personal blacklist. Reporting system, not a screening tool. Police endorsement is a strength for legitimacy but a trust barrier for some workers.
- **Relevance to Scarlot:** The benchmark. Proves the model legally and operationally. Cited in counter-arguments as direct precedent for collective safety features.

### Projet Jasmine

- **Geography:** France
- **Operator:** Medecins du Monde, with STRASS (sex worker union) and partner organizations
- **Model:** Free, registration required for alert features
- **What it does:** Violence prevention program with a client alert database. 8-tier danger classification from low-risk ("Lapin" for no-shows, "Negociateur" for price haggling) through medium ("Desagreable", "Rendez-vous difficile") to high ("Dangereux", "Tres dangereux" for documented assault/rape).
- **How lookup works:** Members can search by email, phone number, or license plate number against the alert database.
- **Additional tools:** Health resources, self-defense guidance, cybersecurity tips, legal rights, provider locator.
- **Has a mobile app.**
- **Limitations:** France-only. France operates the Nordic model (buying criminalized since 2016), which shapes the legal and operational context. Community-run with limited hours (Mon-Fri 9-6). Web/app interface, not conversational.
- **Relevance to Scarlot:** Closest functional parallel to the collective blacklist vision. The 8-tier classification system is sophisticated and worth studying for Scarlot's behavioral tagging design. Operates in a hostile legal environment (Nordic model) and survives -- suggests a safety tool in Switzerland's fully legal framework has even less legal risk. Referenced by interviewees.

### FlagHim

- **Geography:** Not specified (servers in Iceland)
- **Operator:** Unknown (worker-run community)
- **Model:** Free, closed membership
- **What it does:** Private network where workers share safety information. Manual account approval. AES-256 encryption. Iceland-based servers chosen for strong privacy protections.
- **How lookup works:** Closed community model. Only verified members can access or contribute. No information enters the public sphere.
- **Limitations:** Opaque -- no public stats on membership, reports, or coverage. Manual approval creates friction. Unclear how active or comprehensive the database is. No CRM, no personal blacklist, no conversational interface.
- **Relevance to Scarlot:** Represents the worker-run, privacy-maximalist approach. The design choice to use Iceland hosting for privacy laws is notable. Could be a trust model reference for Scarlot's collective features.

### P411 (Preferred411)

- **Geography:** US, Canada primarily
- **Operator:** Private company
- **Model:** Paid subscription (~$100-150/year for both providers and clients)
- **What it does:** Client verification platform. Clients create profiles and get "okays" (vouches) from verified providers they've seen. Providers check if a client has a P411 profile and positive references. Mutual verification system. Clients provide real ID to P411 for initial verification.
- **How lookup works:** Provider searches for a client by P411 handle or phone number. Sees verification status and reference history.
- **Stats:** Claimed tens of thousands of members at peak. One of the longest-running verification systems.
- **Status:** Active but disrupted by FOSTA-SESTA (2018). Went offline temporarily, returned with modified operations.
- **Limitations:** US-centric. FOSTA-SESTA exposure makes it legally fragile. No European market presence. Not a blacklist -- it's a positive verification system (vouch-based), not a negative one (warning-based).
- **Relevance to Scarlot:** The closest existing commercial model to a subscription-based screening service. Proves willingness to pay for verification. The vouch model (positive references) is complementary to Scarlot's blacklist model (negative flags). Could inform Phase 2 design.

### CheckClient.ch (linked to Bemygirl)

- **Geography:** Switzerland
- **Operator:** Linked to Bemygirl platform
- **Model:** Free trial (screenshots show "6 days left" -- likely time-limited or tied to Bemygirl membership)
- **What it does:** Phone number lookup and reporting tool. Users can search a phone number against a community database and submit reports.
- **Report categories (7-tier):**
  - Safe (green)
  - Time Wasters
  - Non-Payment Clients
  - Health Risk Clients
  - Drugs Abusers
  - Aggressive Clients
  - No-Show Clients
- **Report fields:** Phone number (required), status category (required), city (required), comment (required), username (optional -- e.g. "@sarah_geneva")
- **Lookup results show:** Number of reports, consolidated status, city, carrier info (e.g. "Switzerland - MVNO"), all comments with dates, last report date.
- **Example from screenshots:** A number with 4 consolidated reports tagged "No-Show Client" in Geneva, with detailed comments from multiple reporters describing the same person's behavior.
- **Tagline:** "Stay Secure, Stay Independent"
- **Limitations:** Web-based (not conversational). Appears to require account/trial. No CRM. No personal blacklist. No AI. Unclear moderation process -- reports appear to go live without verification.
- **Relevance to Scarlot:** This is the most direct Swiss competitor for the lookup function. It's already live, already has data (reports from 2024), and is tied to an existing platform with users. The 7-tier classification is less granular than Projet Jasmine's 8-tier but covers the same ground. The lack of verification/moderation (reports go live immediately?) could be a weakness vs. Scarlot's approach. The "6 days left" trial model suggests a freemium or subscription model.

### BMG

- **Geography:** Switzerland, EU
- **Operator:** Commercial (premium escort service)
- **Model:** Paid, ~EUR 450/month (per interview data)
- **What it does:** Premium escort blacklist. Siloed within the BMG network.
- **How lookup works:** Accessible to paying members within the BMG ecosystem.
- **Limitations:** Extremely expensive. Access tied to platform membership -- one interviewee lost access when leaving the platform. Siloed data. Not available to independents outside the BMG network.
- **Relevance to Scarlot:** The anti-pattern. Validates that blacklist data has commercial value (workers pay for it), but the pricing and lock-in model is exactly what Scarlot should not replicate. GABRIELLE's experience losing access to BMG data is a key interview finding.

### CheckPunter

- **Geography:** Multi-country
- **Operator:** Unknown
- **Model:** Free
- **What it does:** Client blacklist database.
- **How lookup works:** Basic search functionality.
- **Limitations:** Limited functionality. No detail available on verification, moderation, or data quality.
- **Relevance to Scarlot:** Exists, but appears to be low-quality/low-trust. Validates demand.

### Date-Check

- **Geography:** US
- **Operator:** Private company
- **Model:** Paid (per-check ~$5-10 or subscription)
- **What it does:** Client verification service. Providers submit client phone numbers/emails; Date-Check verifies identity using proprietary database checks against public records.
- **Status:** Uncertain -- needs live verification (2026).
- **Relevance to Scarlot:** Different model (real-identity verification against public records vs. community-sourced reports). Interesting as a potential integration layer.

---

## Emerging / Planned

### ProCoRe "Bad Client List"

- **Geography:** Switzerland (national)
- **Operator:** ProCoRe -- umbrella organization of 27 counseling centers for sex workers in Switzerland
- **Status:** Survey phase (April 2026). Planned launch 2027.
- **What it does (planned):** National "Bad Client List" for sex workers in Switzerland. Web/mobile-first platform (not an app). Exclusively for sex workers.
- **Planned features:**
  - Submit reports ("alerts") about violent or problematic clients using predefined categories
  - Categories: threats, theft, coercion, physical violence, sexual violence, etc.
  - Data protection constraint: only criminal offenses can be reported
  - Each entry includes: location, date, gender, client's phone/email/profile name, category of incident
  - Search function: phone numbers, email addresses, profile names, license plate numbers
  - Pop-up on submission linking to ProCoRe counseling centers across Switzerland
- **Access control (being surveyed):**
  - Recommendation by an already registered sex worker
  - Recommendation by a ProCoRe counseling center
  - Verification based on having an ad on a platform
- **Languages (being surveyed):** German, French, Italian, English, Spanish
- **Survey questions reveal:**
  - They're gauging whether workers already use a bad client list (and which one)
  - Whether workers would use it to submit reports, view reports, or both
  - Desired frequency: before every client, only on suspicion, monthly, rarely
- **Relevance to Scarlot:** This is the most significant competitive signal. ProCoRe has institutional reach (27 organizations across Switzerland), legitimacy, and the trust of NGOs/counseling centers. If they launch successfully, they occupy the collective blacklist space in Switzerland specifically.

**Key differences from Scarlot:**
  - ProCoRe is a standalone web platform. Scarlot is conversational, embedded in messaging.
  - ProCoRe is collective-only (shared database). Scarlot starts individual (personal blacklist/CRM) and adds collective later.
  - ProCoRe is restricted to criminal offenses (data protection). Scarlot's personal blacklist has no such restriction (your own data, your own rules).
  - ProCoRe is a lookup tool. Scarlot is an AI agent that combines lookup with CRM, screening, booking, and more.
  - ProCoRe is NGO-run (free, grant-funded). Scarlot is commercial (subscription).

**Potential relationship:** ProCoRe could be a data source or integration partner rather than a competitor. If they build the collective database, Scarlot could query it -- adding value to both.

---

## Regional "Ugly Mugs" / Bad Date List Programs

These are the equivalent of NUM in other countries. All are NGO/charity-run, free, and distribution-based (not on-demand lookup). None are digital-first.

### Australia (fragmented by state)

- **Operator:** Scarlet Alliance (national umbrella) + state-level sex worker organizations
- **Model:** Free, government/grant-funded
- **How it works:** Workers report to local orgs. Reports anonymized and circulated as "Ugly Mugs" alerts via email, SMS, and outreach workers. Each state org maintains its own list.
- **State schemes:** RhED (Victoria), SWOP NSW (New South Wales), Respect Inc (Queensland), SIN (South Australia), Magenta (Western Australia)
- **Limitations:** No single national database. Fragmented. Paper/email-based distribution. No on-demand lookup.
- **Status:** Active. Operating since the 1990s in various forms.

### New Zealand

- **Operator:** NZPC (New Zealand Prostitutes' Collective)
- **Model:** Free, government-funded (NZPC receives public health funding post-decriminalization 2003)
- **How it works:** Workers report bad clients to NZPC. Compiled into "Bad Trick" sheets distributed through regional offices, outreach, and to managed premises.
- **Key insight:** Full decriminalization since 2003 means highest reporting rates globally. Workers report without fear of prosecution.
- **Limitations:** Paper-based distribution. No digital lookup.
- **Status:** Active since 1987.

### Canada (city-level)

- **Model:** Free, funded by public health and violence prevention grants
- **How it works:** Bad date reports collected by outreach workers, posted in drop-in centers, distributed as physical sheets, increasingly shared digitally.
- **Key programs:**
  - **PACE Society** -- Vancouver. One of the oldest. Played a role in identifying serial predator patterns.
  - **Stella** -- Montreal. Francophone. "Mauvais clients" lists through outreach.
  - **Maggie's Toronto** -- Peer-led safety info distribution.
  - **DEED** -- Edmonton. **SWAP** -- Hamilton. **SHARP** -- Winnipeg.
- **Limitations:** City-level only. No national coordination. Physical/email distribution.
- **Status:** Active across multiple cities.

---

## Platform-Embedded Safety Features

Advertising/listing platforms that include some form of safety tooling within their ecosystem.

### German Platforms

- **Ladies.de** -- Major German escort platform with forum sections including dedicated safety warning threads, organized by city/region.
- **Kaufmich.com** -- German platform with verification features and some safety reporting.
- **Lusthaus.cc** -- Forum with worker-posted client warnings.
- **Model:** Free (embedded in platform use). Community-moderated.
- **Limitations:** Siloed within each platform. Forum-based (unstructured, unsearchable at scale). Useful only if you're active on that specific platform.

### Swiss Platforms

- **Bemygirl (bemygirl.ch) / CheckClient.ch** -- Bemygirl operates CheckClient.ch as a linked safety lookup tool (see dedicated entry above). Also has a platform-linked WhatsApp community group that functions as an informal collective blacklist (per INT9). Screenshots: `docs/screenshots/checkclient/`
- **F-Girl (fgirl.ch)** -- Dominant in DE-CH. [To confirm: whether any built-in blacklist or lookup feature exists beyond basic platform tools.]
- **And6** -- Referenced by interviewees as a data source for blacklist scraping (GABRIELLE).

### US Platforms (diminished post-FOSTA)

- **TER (The Erotic Review)** -- Client reviews of providers. Providers use review history as a screening proxy. Paid. Active but diminished post-FOSTA. Controversial: client-review-of-provider model, not safety-first.
- **Tryst.link** -- Sex worker-founded advertising platform. Worker-to-worker messaging, blocking tools. Not a blacklist, but part of the safety ecosystem. Active and growing post-FOSTA.
- **Eros.com**, **Slixa** -- Have/had internal safety features. Limited detail available.

---

## Sex Worker Organizations with Safety Networks

These are advocacy/union organizations that facilitate safety information sharing among members, but do not operate a structured digital lookup tool. They represent the informal layer -- WhatsApp groups, meetings, outreach.

| Organization | Geography | Type | Notes |
|-------------|-----------|------|-------|
| Hydra e.V. | Germany (Berlin) | Sex worker org | Germany's longest-running. Safety info through network and publications. |
| BSD (Berufsverband Sexarbeit) | Germany | Professional association | Networking + informal client info sharing among members. |
| PROUD | Netherlands | Sex worker union | Information sharing among members. Licensed premises maintain their own informal blacklists. |
| UTSOPI | Belgium | Sex worker collective | Safety information sharing among members. |
| Hetaira | Spain (Madrid) | NGO (since 1995) | Long-running. Facilitates safety information networks. |
| CATS | Spain | Support committee | Safety resources and informal networks. |
| SWEAT | South Africa | NGO | Violence reporting system + Sex Worker Helpline. Documented thousands of incidents. |
| Sisonke | South Africa | Worker-led movement | Peer safety info sharing. |
| DMSC / Durbar | India (Kolkata) | Collective (70K+ members) | Community policing model. Self-regulatory boards track violence, collectively ban violent clients from areas. |
| Empower Foundation | Thailand | NGO (since 1985) | Safety education. Published "Can Do!" bar worker guide with screening advice. |
| AMMAR | Argentina | Union (CTA-affiliated) | Most organized in LATAM. Warnings through meetings and network. |
| RedTraSex | Latin America (15+ countries) | Regional network | Umbrella coordinating body. Individual country orgs handle local safety info. |
| Stella | Montreal | NGO | Francophone. Runs bad client list (see Canada section above). |

---

## Defunct / Uncertain

### ClientEye

- **Geography:** UK/EU
- **Model:** Freemium app
- **Status:** Possibly defunct (March 2025). Appears to have dissolved.
- **What it did:** Anonymous phone/email checking. Mixed reviews. Vindictive reporting concerns.
- **Relevance:** Cautionary tale about moderation. Vindictive reporting is the key risk for any collective system.

### Switter

- **Geography:** Global (Australia-based)
- **Status:** Shut down (2022)
- **What it did:** Mastodon-based social network (430,000+ users) with bad date lists and safety tools.
- **Why it died:** Australian, UK, and US legislative changes made cross-jurisdictional compliance impossible.
- **Relevance:** Jurisdiction selection is existential. Switzerland's legal framework is a structural advantage.

### VerifyHim

- **Geography:** US
- **Model:** Paid (provider subscription)
- **What it did:** Phone number lookup against a database of community-sourced reports.
- **Status:** Uncertain -- may be defunct or rebranded. Needs live verification.

### ECCIE (Escort Client Community Information Exchange)

- **Geography:** US
- **Status:** Shut down post-FOSTA-SESTA
- **What it did:** Major US forum with client/provider verification.

---

## Grassroots Digital Tools

Post-FOSTA and post-Switter, safety information sharing has fragmented into informal digital channels.

| Channel | How it works | Limitations |
|---------|-------------|-------------|
| **Telegram private groups/bots** | Invite-only groups, often city-specific. Some have structured bot-based lookup (submit a phone number, get reports). Vouched entry. | Hard to find, hard to trust. No quality control. Intentionally hidden. |
| **WhatsApp groups** | TDS share warnings in private group chats. Platform-linked groups exist (e.g., bemygirl community group per INT9). | Unstructured, unsearchable, ephemeral. Depends on network access. |
| **Mastodon/Fediverse** | Post-Switter migration. Assembly.social was one attempt. | Low adoption. Fragmented. |
| **Reddit (r/SexWorkers etc.)** | Occasional safety info sharing. | Against platform rules in some cases. Not reliable. |

---

## Safety Timer / Check-in Tools (Adjacent)

Not client databases, but safety tools sex workers use alongside screening:

| Tool | What it does |
|------|-------------|
| Kitestring | Text-based check-in. Sends alert to contacts if you don't check in by a set time. |
| bSafe | Personal safety app with live GPS tracking. |
| Various "SOS" / dead man's switch apps | Repurposed for session timing and check-ins. |

---

## Informal Substitutes (from 8 Scarlot Interviews)

| Substitute | How it works | Limitations |
|-----------|-------------|-------------|
| Word of mouth / WhatsApp groups | TDS share warnings in private group chats | Unstructured, unsearchable, ephemeral. Depends on network access. |
| Personal notes (phone, paper) | TDS keep their own records of bad clients | Not shared. Lost on phone change. No search. Mood-dependent (P6). |
| Platform-specific tools | Some platforms (BMG) offer internal blacklists | Siloed. Lost when leaving the platform. Expensive. |
| DIY solutions | GABRIELLE built a blacklist scraper in Rust | Requires technical skill. One-off. Not replicable. |
| Google / reverse phone lookup | Check numbers against public databases | Low hit rate. Not specific to sex work context. |
| Gut feeling | Read the tone, timing, and language of first messages | No data. No memory. Cognitive load (P4). |

---

## Landscape Summary

### Structured Lookup Tools

| Platform | Geo | On-demand Lookup | Personal BL | Collective BL | CRM | Conversational | Price | Status |
|----------|-----|-----------------|-------------|---------------|-----|----------------|-------|--------|
| NUM | UK/IE | No (alerts only) | No | Yes (reports) | No | No | Free | Active |
| Projet Jasmine | FR | Phone/email/plate | No | Yes (8-tier) | No | No | Free | Active |
| CheckClient.ch | CH | Phone | No | Yes (7-tier) | No | No | Freemium? | Active |
| FlagHim | Global? | Closed network | No | Yes | No | No | Free | Active |
| P411 | US/CA | Handle/phone | No | No (vouch-based) | No | No | ~$100-150/yr | Active |
| BMG | CH/EU | Internal | No | Yes (siloed) | No | No | ~EUR 450/mo | Active |
| CheckPunter | Multi | Basic search | No | Yes | No | No | Free | Active |
| Date-Check | US | Phone/email | No | No (ID verify) | No | No | ~$5-10/check | Uncertain |
| ProCoRe BCL | CH | Phone/email/plate | No | Yes (planned) | No | No | Free (planned) | 2027 |
| **Scarlot** | **CH** | **Phone + AI** | **Yes** | **Phase 2** | **Yes** | **Yes** | **CHF 15-55/mo** | **Building** |

### Key Observations

1. **No one has built the unified digital tool.** Every existing solution is either (a) paper/email-based NGO distribution, (b) forum threads on advertising platforms, (c) paid US-centric verification, or (d) informal WhatsApp/Telegram groups.

2. **The "credit bureau for clients" model does not exist yet.** This is the ProCoRe 2027 vision and Scarlot's implicit direction. Nobody globally has built structured, persistent, searchable client reputation data with a dispute mechanism.

3. **The NGO/ugly mugs model is universal but analog.** Every country with sex worker organizations has some version of "share warnings about bad clients." Almost all are paper-based, email-based, or rely on outreach workers. None offer real-time lookup from a messaging app.

4. **Post-FOSTA fragmentation created the opportunity.** Switter's shutdown (2022), ECCIE's closure, and the chilling effect on US platforms left a global vacuum. Workers migrated to Telegram groups which are hard to find, hard to trust, and have no structured data.

5. **P411 is the closest commercial precedent** -- subscription model, client verification, mutual references. But US-centric, FOSTA-disrupted, and vouch-based (positive verification) rather than warning-based (negative flags).

6. **Legal framework determines reporting rates.** NZ (full decrim) has highest rates. Criminalized jurisdictions suppress reporting. Switzerland's regulatory model is favorable.

7. **ProCoRe is the one to watch in Switzerland.** If they launch in 2027 with a functional national database, the strategic question is: compete, integrate, or complement?

**The gap remains:** No existing tool combines personal blacklist + collective lookup + CRM + conversational interface + AI screening. Projet Jasmine and ProCoRe come closest on the collective side. P411 comes closest on the commercial side. Nobody addresses the individual side (personal CRM, client memory, behavioral tagging). Nobody is conversational. Nobody is AI-augmented.

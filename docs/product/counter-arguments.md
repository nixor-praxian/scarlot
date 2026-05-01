# Maia - Counter-Argument Briefing

*Structured rebuttals to the ten most common objections. Equip founders for investor meetings, legal consultations, partnership discussions, and media interactions.*

*Draft - April 2026. Not legal advice. Consult Swiss counsel before relying on legal arguments.*

---

## Quick-Reference Table

| #   | Objection                       | One-line counter                                             | Key precedent                                                  |
| --- | ------------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------- |
| 1   | Facilitating prostitution       | CRM for a legal profession is not facilitation               | Accountants, landlords, ad platforms all serve TDS legally     |
| 2   | Payment processors will refuse  | SaaS subscription MCC, not adult; SEPA/Swiss banking backup  | German/Dutch sex work businesses use standard banking          |
| 3   | App store ban                   | No app needed - messaging bot + PWA                          | NUM is on app stores; architecture avoids the problem entirely |
| 4   | FOSTA-SESTA reaches Switzerland | US law, Swiss company, no US nexus, dual criminality defense | No extraterritorial application to non-US entity documented    |
| 5   | Blacklist = defamation          | Truth defense + credit bureau model + NUM precedent          | Credit bureaus, National Ugly Mugs (UK govt-funded)            |
| 6   | nFADP violation                 | Regulated, not prohibited; health platforms do more          | Swiss health platforms process far more sensitive data legally |
| 7   | Investors won't touch it        | Impact investing + lone worker safety framing                | UK government funds NUM; OnlyFans proved the market            |
| 8   | Founders personally liable      | Swiss GmbH shields; no criminal act = no prosecution         | Backpage was active concealment; this is the opposite          |
| 9   | Messaging platform will ban you | Multi-channel by design; no single platform dependency       | Architecture survives any channel eviction                     |
| 10  | AI can't be trusted for safety  | AI flags, human decides - standard model in medicine/banking | FDA diagnostic AI; bank fraud detection; airport screening     |

---

## 1. "You're facilitating prostitution"

### The objection
Building tools that help sex workers manage their business amounts to facilitating or promoting prostitution.

### Counter-arguments

**A. Swiss law explicitly legalizes sex work.**
Sex work in Switzerland has been legal since 1942. TDS register with cantonal authorities, pay income tax, and contribute to social insurance (AHV/IV).

**Article 195 StGB** criminalizes *encouraging* someone into prostitution *against their will*, or exploiting someone already in prostitution. The key elements are coercion, exploitation, and restriction of freedom. A CRM/safety tool for consenting adults meets none of these elements.

**B. The tool/platform distinction is decisive.**

| What we ARE | What we are NOT |
|---|---|
| A productivity and safety tool USED BY sex workers | A marketplace FOR sex work |
| Analogous to a CRM, calendar, or safety app | Analogous to Backpage or an escort directory |
| Installed on the user's own device/account | Operating a public-facing platform |
| Post-contact (operates after client reaches out) | Connecting buyers and sellers |

**C. Precedents: tools serving legal sex work without "facilitating."**

Accounting software used by TDS - legal everywhere. WhatsApp / Telegram used for sex work communications daily - no prosecution. BMG / F-Girl in Switzerland - legal, regulated advertising platforms. National Ugly Mugs (UK) - safety tool, endorsed by police and funded by government. Landlords renting to TDS - legal, regulated.

**Recommended talking point:**
> "Sex work is legal in Switzerland. We build safety and productivity tools for independent professionals. Saying our CRM facilitates prostitution is like saying QuickBooks facilitates accounting fraud because some users might commit it."

---

## 2. "Payment processors will refuse you"

### The objection
Visa, Mastercard, Stripe, and PayPal routinely refuse service to adult/sex work businesses.

### Counter-arguments

**A. Maia is not processing sex work transactions.**
Maia charges a monthly SaaS subscription for a productivity tool. The sex work transactions themselves happen between the TDS and their client (cash, Twint, bank transfer). Maia never touches that money.

**B. Merchant Category Code (MCC) determines restrictions.**
Maia registers under software MCC codes (5734, 5817, 7372), not adult content codes. None trigger restrictions.

**C. European payment infrastructure exists.**

| Channel | Viability | Notes |
|---|---|---|
| SEPA direct debit | Fully viable | Bank-to-bank, no card network restrictions |
| Swiss direct debit (LSV/DD) | Fully viable | Swiss-specific, no intermediary restrictions |
| Twint | Fully viable | Already used by TDS for client payments |
| Swiss bank transfer | Fully viable | Standard invoice-and-pay |
| Stripe | Likely viable | SaaS MCC, not adult content |

**D. Legal sex work businesses in Germany and Netherlands process payments through standard European banking.** The financial system already serves this industry.

**Recommended talking point:**
> "We're a SaaS company charging a monthly subscription. We don't process sex work transactions. Our MCC code is 'software,' not 'adult.' SEPA direct debit and Swiss banking don't have card network restrictions."

---

## 3. "You'll get banned from app stores"

### The objection
Apple and Google will reject any app associated with sex work.

### Counter-arguments

**A. Maia doesn't need an app store.** The primary interface is existing messaging apps. If a dashboard is built, it's a PWA (Progressive Web App) - no app store submission required.

**B. Sex worker safety apps ARE on app stores.** National Ugly Mugs has iOS and Android apps. StaySafe (lone worker safety) is on both stores. These pass review because they're framed as safety tools.

**C. The messaging-first architecture makes this moot.** The user never installs anything new. The bot connects to their existing messaging account.

**Recommended talking point:**
> "We don't have an app. We don't need an app. The product lives inside the messaging apps already on every user's phone."

---

## 4. "FOSTA-SESTA will reach you in Switzerland"

### The objection
FOSTA-SESTA creates liability for platforms facilitating sex trafficking. It could reach a Swiss company.

### Counter-arguments

**A. FOSTA-SESTA is a US federal law with US jurisdictional limits.** It applies to entities within US jurisdiction or using US-based infrastructure. A Swiss entity with Swiss servers, Swiss customers, and no US operations has no nexus.

**B. No documented extraterritorial application.** There is no publicly documented case of FOSTA-SESTA being applied to a company incorporated outside the US, operating outside the US, serving non-US customers.

**C. Swiss dual criminality defense.** Switzerland cooperates with foreign legal requests under IMAC/EIMP, but requires dual criminality - the act must be a crime in both jurisdictions. Sex work is legal in Switzerland. A US request based on sex work facilitation would fail the dual criminality test.

**D. Minimizing any theoretical US nexus:**
- Swiss incorporation
- European hosting (Infomaniak, Exoscale)
- No US customers targeted
- European payment processing
- Evaluate AI provider dependency (US-based providers create a potential nexus)

**Recommended talking point:**
> "FOSTA-SESTA is a US law. We're a Swiss company, with Swiss infrastructure, serving Swiss customers. Switzerland requires dual criminality for international legal cooperation - and sex work is legal here."

---

## 5. "The collective blacklist is defamation"

### The objection
Sharing negative information about identifiable individuals constitutes defamation under Swiss law.

### Counter-arguments

**A. Truth is an absolute defense.** Swiss defamation law (Art. 173-178 StGB): if the reported incident factually occurred, truth is a complete defense under Art. 173(2).

**B. The credit bureau model is directly analogous.** CRIF, Creditreform, and Intrum in Switzerland collect and share negative information about individuals (missed payments, defaults). They are legal because they: report factual, verifiable events; provide a dispute process; apply data retention limits; have a legitimate interest basis. The blacklist uses the same model.

**C. Multiple international precedents exist.** National Ugly Mugs (UK, since 2012): government-funded, police-endorsed, processes ~1M safety alerts/year. Projet Jasmine (France, Medecins du Monde): operates a searchable client alert database with 8-tier danger classification -- under the Nordic model, a more hostile legal environment than Switzerland. Ugly Mugs programs in Australia (state-level), New Zealand (NZPC), and Canadian Bad Date Lists (PACE Vancouver, Stella Montreal, etc.) have operated for decades. ProCoRe (Switzerland's own umbrella of 27 counseling centers) is building a national "Bad Client List" for 2027 launch. The model is legally proven across multiple jurisdictions.

**D. Design choices that minimize risk:**
- Reports are factual events ("no-show on 15 March"), not opinions ("untrustworthy person")
- Personal blacklist (private) has zero defamation exposure
- Shared entries require verification/moderation before going live
- Time-limited entries (auto-expire)
- Dispute mechanism for flagged individuals
- Phone number only, no names or photos
- Closed network, not public

**Recommended talking point:**
> "Credit bureaus share negative information about people legally, every day. Our blacklist uses the same model: factual reports, verified entries, dispute mechanisms, and data retention limits."

---

## 6. "Storing sex work data violates nFADP"

### The objection
The nFADP classifies data about sexual behavior as sensitive personal data, triggering strict requirements.

### Counter-arguments

**A. The nFADP does not prohibit processing sensitive data - it regulates it.** Art. 5(c) identifies sensitive personal data. Processing requires: explicit consent, proportionality, purpose limitation, data security, transparency, and a DPIA for high-risk processing. All achievable.

**B. What Maia actually stores is mostly operational.** Phone numbers, booking times, availability, behavioral notes ("no-show," "late"), communication preferences. A booking time is a booking time. A no-show is a no-show. This is CRM data.

**C. Swiss health platforms provide the compliance blueprint.** They process genuinely sensitive health data under the nFADP with explicit consent, encryption, access controls, DPIAs, retention policies, and processing registers.

**D. Compliance strategies available (architecture TBD):**

Data architecture is not yet decided. These are strategies to evaluate during beta design:

- On-device processing where possible
- Encryption at rest
- Minimal data collection
- Automatic data expiration
- Data portability and deletion rights
- Swiss hosting for any shared features

**Recommended talking point:**
> "The nFADP doesn't ban sensitive data - it regulates it. Swiss health platforms process far more sensitive data legally. We will follow the same compliance framework: explicit consent, purpose limitation, data minimization, and a DPIA before launch."

---

## 7. "Investors won't touch a sex work company"

### The objection
VCs and angels will refuse to fund anything associated with sex work.

### Counter-arguments

**A. We're not a sex work company.** We're a personal AI agent platform for independent service providers. Sex workers are the first vertical because the need is most acute. The platform is architecturally general-purpose.

**B. Investors HAVE funded adjacent companies.** OnlyFans: self-funded, multi-billion valuation. National Ugly Mugs: UK Home Office grants, Comic Relief, charitable trusts. The Dancer Resource: venture-backed safety/CRM for dancers.

**C. Impact investing is a natural fit.** Maia addresses: physical safety of a vulnerable population, economic empowerment, digital inclusion, gender equity. Maps directly to impact investor criteria.

**D. The lone worker safety market ($1.2-1.8B) is the investor-friendly framing.** Same value proposition, same user profile, delivered through conversational AI. "Doctolib for independents" is a pitch that works.

**E. The reputational landscape is shifting.** Amnesty International (2016), WHO, UNAIDS, Human Rights Watch all support decriminalization. Building safety tools for sex workers is increasingly seen as progressive, not scandalous.

**Recommended talking point:**
> "We're building lone worker safety and productivity tools. Sex workers are our first vertical because the need is most urgent. The UK government literally funds National Ugly Mugs. This is impact investing with a clear business model."

---

## 8. "The founders will be personally liable"

### The objection
Founders could face personal criminal or civil liability.

### Counter-arguments

**A. Swiss corporate law provides strong liability shields.** Swiss GmbH (Art. 794 OR) and AG (Art. 620 OR) provide limited liability. Directors are personally liable only for breach of duty (Art. 754 OR) - running a lawful business is not a breach.

**B. Criminal liability requires a criminal act.** Art. 195 StGB requires exploitation, coercion, or involvement with minors. Building a CRM for adults managing their legal business meets none of these elements. *Nulla poena sine lege.*

**C. Tech founder prosecution precedents involve active wrongdoing.** Backpage founders edited ads to conceal trafficking. Silk Road operated a drug marketplace. In every prosecution, founders took active steps to enable crime. Building a safety tool is the opposite.

**D. Structural protections:**
- Swiss GmbH/AG structure (limited liability)
- Legal opinion on record from Swiss attorney
- ToS prohibiting illegal use
- D&O insurance
- Flat subscription pricing (never commission - avoids Art. 195 "financial advantage" element)

**Recommended talking point:**
> "Swiss corporate law provides clear liability shields. Founder prosecution happens when founders actively facilitate crime. We're building a safety tool. That's the opposite."

---

## 9. "The messaging platform will shut you down"

### The objection
WhatsApp/Meta or any messaging provider will detect the bot and ban the account.

### Counter-arguments

**A. This is a real risk - and we design for it.** We don't pretend platform dependency doesn't exist. The architecture is built to survive any single channel dying.

**B. Multi-channel by design.** Maia is channel-agnostic. It adapts to whatever messaging platform the user prefers and the market requires. No single platform is a dependency.

**C. The bot's messages contain no problematic content.** What any messaging provider would see: professional, polite text about scheduling and availability. No explicit content flows through automated messages.

**D. Historical precedent:** Messaging platforms have coexisted with automated tools for years. Business automation on messaging is a growing market, not a shrinking one.

**Recommended talking point:**
> "We're architected to survive platform eviction, not to avoid platform risk. No single messaging provider is a dependency. The intelligence and client data persist across channels."

---

## 10. "AI can't be trusted with safety decisions"

### The objection
Using AI to screen clients is irresponsible - mistakes could lead to physical harm.

### Counter-arguments

**A. AI flags, humans decide.** The model is semi-automatic: AI handles message triage and surfaces risk signals. The TDS makes every safety decision. The AI never says "this client is safe." It says "this number appears on the blacklist" or "this client has canceled 3 times."

**B. AI in safety-critical domains is well-established.** Medical diagnostics (FDA-approved AI radiology). Financial fraud detection (every major bank). Airport security screening. Content moderation. All use the same model: AI augments human judgment.

**C. The current alternative is worse.** Without Maia: no screening, no blacklist (or €450/month for one), no client history, safety decisions based on gut feeling during a text exchange. Even a basic number lookup is a massive improvement over nothing.

**D. Liability framework:** AI provides information, TDS decides. No claim of guaranteeing safety. Blacklist entries are factual reports, not AI predictions. Full audit trail. TDS can override any AI action instantly.

**Recommended talking point:**
> "Our AI doesn't make safety decisions - the user does. The AI surfaces information: 'this number is on the blacklist,' 'this client was a no-show three times.' The user decides what to do. Same model as AI in medicine, banking, and airport security."

---

## Legal Architecture Summary

From the parallel legal structure research:

| Decision           | Recommendation                                              | Why                                                                   |
| ------------------ | ----------------------------------------------------------- | --------------------------------------------------------------------- |
| Entity type        | Swiss SA (AG)                                                | CHF 100K capital, stronger liability shield, shareholder privacy, investor-ready structure |
| Revenue model      | Flat subscription only                                      | NEVER commission - Art. 195 StGB risk                                 |
| Blacklist entity   | Separate Verein (association)                               | Isolates highest-risk feature; follows NUM charity model              |
| AI transparency    | Disclose AI to clients                                      | Required by EU AI Act Art. 50; good practice                          |
| Data storage       | On-device where possible; Swiss hosting for shared features | nFADP compliance + data sovereignty                                   |
| DSA classification | Tool, not platform                                          | Recital 13 exempts software facilitating use of intermediary services |
| Insurance          | D&O + professional liability (E&O) + cyber                  | Standard startup protection                                           |

**The single most important structural decision: flat subscription pricing, never commission.** This is the firewall between "software vendor" and "someone profiting from prostitution."

---

*"The question is not whether we can serve this population legally - Swiss law is clear that we can. The question is whether we will, when no one else does."*

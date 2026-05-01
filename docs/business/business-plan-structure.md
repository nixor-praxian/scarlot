# Business Plan - Two-Round Structure

*April 2026*

---

## Round 1 - Survive (Pre-seed / Angel / Bootstrap)

**Ask:** CHF 150K–500K
**Timeline:** 12–18 months
**Story:** "We know this market better than anyone. We have the data to prove it. We have insider access. We have a working product."

### What makes Round 1 credible

1. **Proprietary market census** (from scarlot-market-data scraper)
   - [X] unique deduplicated identities across 8 Swiss platforms
   - [X]% independent (non-agency) - our actual addressable market
   - [X] active in FR-CH (Geneva + Vaud + Neuchâtel) - our launch market
   - City-by-city breakdown with actual numbers, not estimates
   - Seasonality curves from weeks of continuous scraping
   - Agency vs. independent split - algorithmically detected + manually verified
   - Churn rate and new entrant rate - market dynamics from real data
   - Willingness-to-pay scoring based on observable behavior (premium placement, multi-platform, update frequency)

2. **8 validated user interviews**
   - 16 problems mapped with evidence tiers (Gold/Silver/Bronze)
   - 10 assumptions tracked with status
   - Top 3 problems confirmed across all interviews: safety screening, client memory, cognitive overload
   - GABRIELLE (INT8): built her own blacklist scraper and CRM - ultimate problem validation

3. **Insider community access**
   - Co-founder with deep trust in Geneva TDS community
   - Aspasie (Geneva NGO) as distribution channel
   - ProCoRe network (27 organizations) as awareness multiplier
   - 10-user beta network ready

4. **Working prototype**
   - [Status: confirm whether the POC exists or is in progress]
   - AI agent running on messaging, tested with real users
   - Safety + Memory coverages functional

5. **Clear legal home**
   - Switzerland: sex work legal since 1942
   - Swiss Sàrl structure with limited liability
   - Flat subscription pricing (never commission - Art. 195 firewall)
   - Full risk analysis with mitigation plan

### Round 1 Deck Outline

| Slide | Content | Data Source |
|-------|---------|------------|
| 1. Problem | "40M people, zero tools." GABRIELLE's story. Three pain points. | Interviews (Gold tier) |
| 2. Market - Precise | Swiss market census from our scraper. Exact numbers. City map. | scarlot-market-data DB |
| 3. Market - Context | $100-200B global. 700K-1M Europe. Legal in 21+ countries. | Market research report |
| 4. Solution | Maia: personal AI agent. Coverages model. Demo screenshot. | Platform vision |
| 5. Why Now | AI agents work now. Decrim accelerating. Every platform died. | Market research |
| 6. Traction | 8 interviews. Beta network. Prototype. Proprietary data pipeline. | Interviews + scraper |
| 7. Business Model | CHF 15-55/mo subscription tiers. Unit economics. | Platform vision |
| 8. Competition | Landscape table. The gap nobody fills. ProCoRe 2027 as complement not threat. | Safety lookup landscape + Market research |
| 9. Go-to-market | FR-CH → Switzerland → DACH → Europe. Community-driven. | Platform vision |
| 10. Risk | Honest risk matrix with mitigation. WhatsApp dependency addressed. | Risk analysis |
| 11. Team | Co-founder (insider), Advisor (product/tech), [CTO needed] | Founder memo |
| 12. Ask | CHF [X]. 18 months. Milestones: beta launch, 50 users, revenue. | This document |

### Round 1 Key Metrics to Present

**From the scraper (precise, proprietary - pulled April 7, 2026):**

| Metric                       | Value                                                 | Notes                                                                                                                          |
| ---------------------------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Total unique identities      | **5,229**                                             | Deduplicated by phone (E.164) across 7 platforms                                                                               |
| Total active listings        | **9,091**                                             | Same person on 3 platforms = 3 listings, 1 identity                                                                            |
| Active in FR-CH              | **1,238**                                             | Geneva (547) + Lausanne (296) + Neuchâtel (48) + Fribourg (63) + others                                                        |
| Geneva alone                 | **547**                                               | Incl. "Genève" (495) + "Genf" (52) variants                                                                                    |
| Active in all of Switzerland | **5,229**                                             | Current potential Footprint based on our scraping of the major platforms                                                       |
| Top 5 cities                 | GE 547, Lausanne 296, Basel 170, Bern 161, Zurich 161 | Geneva is #1 - validates FR-CH launch                                                                                          |
| Multi-platform presence      | **869 workers (17.6%)** on 2+ platforms               | 769 on 2, 86 on 3, 14 on 4                                                                                                     |
| Premium/featured placement   | **Not yet reliable**                                  | Detection only implemented on 2/7 platforms; xdate marks 100% as "premium" by default. Needs per-platform audit before citing. |
| Agency vs. independent       | **0 flagged agency** (detection Phase 4 pending)      | Phone-based dedup done; image-based not yet                                                                                    |
| Scraping period              | March 16 – April 8, 2026                              | 150 completed runs, 21,364 snapshots                                                                                           |
| Platforms                    | xdate, fgirl, and6, lolla, sex4u, catgirl, bemygirl   | 7 active adapters                                                                                                              |

**From interviews (validated, qualitative):**

| Metric                              | Value                                                                                                                   |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Interviews conducted                | 8 (7 unique participants)                                                                                               |
| Top problems validated at Gold tier | P3 (no inbound filtering), P5 (no centralized blacklist), P4 (cognitive overload), P6 (no-shows), P2 (no client memory) |
| Workaround sophistication           | GABRIELLE built a blacklist scraper + CRM in Rust                                                                       |
| Willingness to pay (behavioral)     | GABRIELLE paid CHF 50/mo for Airtable, bought a server                                                                  |
| AI acceptance boundary              | Passive screening: yes. Active conversation: no.                                                                        |

### Round 1 Unit Economics

| Metric | Conservative | Optimistic |
|--------|-------------|-----------|
| Year 1 users | 50 | 200 |
| ARPU | CHF 20/mo | CHF 35/mo |
| Year 1 ARR | CHF 12K | CHF 84K |
| Year 2 users | 200 | 500 |
| Year 2 ARR | CHF 48K | CHF 210K |
| Year 3 users | 500 | 2,000 |
| Year 3 ARR | CHF 120K | CHF 840K |
| CAC | ~CHF 0 (community-driven) | ~CHF 0 |
| LTV (24mo avg retention) | CHF 480–840 | |
| Gross margin | 85%+ (SaaS) | |

---

## Round 2 - Scale (Seed / Series A)

**Ask:** CHF 1–3M
**Timeline:** 18–36 months after Round 1
**Prerequisite:** Product-market fit proven. Revenue. Second vertical validated.
**Story:** "We proved the model with the hardest vertical. Now we're the operating system for independent service providers. Here's the $1B+ market."

### What makes Round 2 credible

1. **Proven product-market fit**
   - [X] paying users in Scarlot vertical
   - Retention rate, NPS, usage metrics
   - Revenue growth curve

2. **Second vertical validated**
   - Therapists/masseuses (Soigneur pack) or trainers (Mentor pack)
   - Interviews + beta users in second vertical
   - Same engine, different coverage - platform thesis proven

3. **Platform economics emerging**
   - Coverage add-on revenue (users upgrading tiers)
   - Cross-vertical patterns starting to transfer
   - Collective data growing (reputation network effects)

4. **Macro market data**
   - TAM: €100-250M (safety + CRM in legal markets, sex work only)
   - Platform TAM: €1B+ (all independent service providers)
   - Adjacent market: $1.2-1.8B lone worker safety, growing to $4-6B by 2033
   - 11 Tier 1 countries + 10+ Tier 2 countries
   - Decriminalization trend expanding market annually

5. **Defensibility**
   - Accumulated behavioral intelligence (can't be replicated without users)
   - Community trust in first vertical (co-founder network)
   - Switching cost (client data + patterns)
   - Multi-channel resilience (survived WhatsApp issues if any)

### Round 2 Deck Outline

| Slide | Content |
|-------|---------|
| 1. Traction | Revenue, users, retention - let numbers speak |
| 2. The Insight | "Every independent service provider faces the same three problems" |
| 3. Platform | Coverages model. Multiple verticals. One engine. |
| 4. Market - Macro | TAM/SAM/SOM. 84 countries where sex work is legal. Lone worker safety adjacent. |
| 5. Flywheel | Individual → Vertical → Cross-vertical tuning |
| 6. Expansion | New verticals proven. Geographic expansion plan. |
| 7. Competition | Still no integrated solution. First-mover advantage compounding. |
| 8. Unit Economics | LTV/CAC, cohort retention, coverage upgrade rates |
| 9. Risk & Mitigation | Updated risk matrix with real-world experience |
| 10. Team | Full team by now - CTO, community leads per vertical |
| 11. Vision | "The operating system for independent service providers" |
| 12. Ask | CHF [X]M. 36 months. Milestones: 5 verticals, 5K users, 5 countries. |

### Round 2 Market Framing

```
PLATFORM TAM (all independent service providers)
├── Sex workers (21+ legal countries): €100-250M
├── Therapists/masseuses: €200-400M (est.)
├── Personal trainers/coaches: €150-300M (est.)
├── Tutors/educators: €100-200M (est.)
├── Nannies/home-care: €100-200M (est.)
├── Freelance creatives: €200-400M (est.)
└── TOTAL PLATFORM TAM: €1-2B+

ADJACENT COMPARABLE
└── Lone worker safety solutions: $1.2-1.8B (2024) → $4-6B (2033)
```

*Note: Non-sex-work vertical TAMs are hypothetical. They become credible only after Round 1 validation.*

---

## Data Pipeline Integration

The scarlot-market-data scraper is the proprietary asset that makes Round 1 credible. Here's how it maps to the business plan:

| Scraper Output | Business Plan Use |
|---|---|
| Total unique identities | Precise Swiss TAM (not estimates) |
| Independent vs. agency split | Actual addressable market size |
| City distribution | Launch market prioritization |
| Multi-platform presence % | WTP proxy ("they pay for visibility") |
| Premium placement % | WTP proxy ("they invest in their business") |
| Seasonality curves | Revenue projection seasonality adjustment |
| New entrant rate | Market growth rate for projections |
| Churn rate | Market dynamics, retention assumptions |
| Phone-based dedup | "We know the real number, not the listing count" |
| Image-based cross-platform matching | "Same person on 3 platforms = one customer, not three" |

### What to say in the pitch

> "Everyone else cites UNAIDS estimates of 20,000-30,000 sex workers in Switzerland. We don't estimate. We've been monitoring 7 Swiss platforms continuously since March 16. We have 5,229 deduplicated, phone-resolved unique identities. 1,238 are in our launch market. 547 are in Geneva alone. 17% maintain profiles on multiple platforms. This is not a guess - it's a census."

---

## Immediate Next Steps

1. **Pull actual numbers from the DGX Spark database** - populate the [PULL FROM DB] placeholders
2. **Confirm prototype status** - does a working demo exist? If not, when?
3. **Swiss legal opinion** - engage counsel on Art. 195, nFADP, blacklist defamation
4. **Build the Round 1 deck** - 12 slides, precise data, honest risks
5. **Update the founder memo** - integrate scraper data, qualify claims per audit findings
6. **Name decision** - Sola? Cura? Something else? Needed before any external materials

---

*"Two rounds, two stories. Round 1: we know this market. Round 2: we are this market."*

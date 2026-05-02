# Hernest — what we're building, and why

**Status**: pilot, pre-production · 2026-05-01

This is the high-level recap. For the engineering picture see `docs/ARCHITECTURE.md` and `docs/v2/architecture/overview.md`.

---

## In one paragraph

**Hernest** is professional infrastructure for independent workers whose business runs on their phone. Our first product, **Scarlot**, is a private assistant for independent sex workers (TDS) in Switzerland that lives inside the conversation they already use all day — WhatsApp. Scarlot helps them screen new contacts, remember who is who, get warned when a number has been reported, and keep the operational side of their work (clients, appointments, payments, blacklist) under control. The TDS stays in charge of every decision. The pitch is "your safety net and your memory, living in WhatsApp."

## Who this is for

Independent sex workers operating solo in Switzerland. They almost universally:

- run their entire business through a personal mobile phone,
- treat WhatsApp as their primary CRM, calendar, and inbox,
- have no employer-provided tooling and no industry-specific software they trust,
- are the target of scams, time-wasters, and unsafe contacts, and pay the cost of a bad screen personally,
- and care a lot about who can see what.

Existing options either ignore them (mainstream SMB tools), put them in a directory they don't want to be in (platform sites), or assume they'll leave WhatsApp for a separate app (they won't).

## The problem we keep hearing

Three jobs collapse onto the same person, with no support:

1. **Memory.** "Have I seen this number before? What did I write down about him last time?" — answered today by scrolling chat history.
2. **Screening.** "Is this number safe? Has anyone else flagged it?" — answered today by group chats and word of mouth.
3. **Operations.** Appointments, payments, who paid, who didn't, who's blacklisted, when to send a reminder — tracked today in head, in notes apps, sometimes nowhere.

Each one is doable manually. Stacked together, on a phone, while doing the actual work, they're the constant tax we want to remove.

## What Scarlot is

A private assistant the TDS talks to in WhatsApp, in natural language, in the language she already uses with clients (FR, DE, IT, EN). She types something like:

> "J'ai vu Marc hier soir, il a payé 300. Rappelle-moi avant son prochain rendez-vous mardi."

…and Scarlot updates the right records, schedules the reminder, confirms back. Destructive things ("blacklist this number") require an explicit *oui* on a second message before they take effect.

Two ways to set it up, depending on how much control she wants Scarlot to have:

- **Contact mode** — Scarlot has its own number. She saves it as a contact and writes to it like a colleague. Lowest commitment, easiest to try. Scarlot only sees what she forwards.
- **Integrated mode** — She pairs WhatsApp with Scarlot once (the way she'd link WhatsApp Web). Then she writes to herself, on her own number, and Scarlot answers in that thread. More capable, requires more trust.

In both modes, **Scarlot never reads or replies to client conversations on its own.** That is the core product promise, and it's enforced by routing in code, not by a prompt. She lives in the admin chat. Clients never hear from her.

## What we are explicitly *not* doing

- We are not a directory. The TDS isn't listed anywhere because of us.
- We are not an autoresponder. Scarlot doesn't pretend to be the TDS to clients.
- We are not a payment processor.
- We are not a marketplace, a forum, or a social product.

The product surface area is intentionally small: a single private chat, on a channel she already trusts.

## Why the system has the shape it has

The repository is a monorepo with four apps, deployed across a few small VMs:

| App | What it does | Why it exists separately |
|---|---|---|
| `scarlot` | The TDS-facing tenant: WhatsApp connection, admin chat orchestration, LLM agent, SQLite. One container per TDS. | Privacy and blast radius. Each TDS gets her own process, her own database, her own volume. |
| `scarlot-admin` | Operator control plane: signup intake, tenant registry, provisioning saga, dashboard, reverse proxy to tenants. | We need a single place to manage tenants without giving tenants access to each other. |
| `scarlot-provisioner` | Docker-host agent: creates and destroys tenant containers on demand, behind mTLS. | Provisioning is privileged work; it lives on the host, talks only to the admin via mTLS. |
| `scarlot-website` | Public marketing site + signup flow (phone verify, mode choice, pairing handoff). | The only public surface; isolated from anything that holds tenant data. |

The corresponding deployment posture is: a public host (`host-www`) with the website and admin behind Caddy, a private host (`host-a`) running the provisioner and the tenant containers, mTLS between them. A bug in the public component cannot directly read tenant data. ([deployment-architecture.md], [host-a/Caddyfile], [pilot-disclosure.md] for the current honest scope.)

## Current state — being honest about it

We are **in pilot**. That means specifically:

- The system is wired end to end: a TDS can sign up on the website, get her own container, pair WhatsApp, and use the assistant.
- **Data is not encrypted at rest yet.** The trust model is "trust the operator." This is documented in `docs/v2/legal/pilot-disclosure.md` and shown to pilot users.
- We have no SLA, no soft-delete, no kernel-level tenant isolation. All on the roadmap; none shipped.
- The agent itself is good enough to be useful but is still being tuned per skill (clients, appointments, payments, blacklist, settings, style). The pre-LLM gate (ADR 005) handles the cheap deterministic paths so most messages don't even reach Claude.

## Where this goes next

In rough order of importance:

1. **Real pilot users.** A small number of TDSs we know personally, with constant feedback. Discovery is ongoing — interview transcripts and the assumption log live alongside the codebase.
2. **Encryption at rest** (SQLCipher per-tenant DEK — ADR 003 — designed, not yet wired) so the trust model stops being "trust the operator."
3. **Community lookup** — the safety-net half of the pitch. A reputation signal on incoming numbers, sourced from the community, with the TDS deciding what to do with it. The plumbing (`scarlot/src/safety/community-lookup.ts`, mock server) is in; the network and the policy aren't.
4. **Hardening the operational story** — backups, restores, tenant deletion, repair. The runbook is filling out as we hit the cases for real.
5. **Sharpening the assistant** — fewer LLM calls (more gate paths), better confirmation UX, better localization, daily digest opt-ins.

## Why we think this works

Three bets, in order of how load-bearing they are:

- **Channel.** The TDS will not leave WhatsApp for an app. Anything that requires her to switch context loses. Living inside her existing chat is the unlock.
- **Posture.** Semi-automatic, admin-only, never-talks-to-clients is not a limitation — it's the feature. It's what makes the product trustable enough to install at all.
- **Stack.** Per-tenant isolation, modest infrastructure, an LLM only when a deterministic path can't answer, and a small set of well-shaped tools. We can run this for a small number of TDSs cheaply enough that we don't have to compromise on privacy to make the unit economics work.

If those three hold, the product is defensible because the trust is. If any of them breaks, we'll know quickly because the pilot is the test.

---

*Maintained alongside the code. If you change what Scarlot does or who it's for, update this doc.*

# Scarlot website — overview

_Date: 2026-05-01 · Source repo: `scarlot-website`_

## What this is

The cold-traffic landing page for Scarlot, plus the thin backend that turns a visitor into a provisioned tenant on the rest of the stack.

This site's only job is to get a beta tester from "I clicked the ad" to "my Scarlot is paired in WhatsApp." Everything that happens after the QR scan is owned by `scarlot-admin`.

## Who Scarlot is for

Independent service providers (TDS) in French-speaking Switzerland. Scarlot is a WhatsApp-resident safety net: it screens new clients, remembers context across conversations, and warns when a number has been reported by other workers. Discovery interviews drive the product; this landing page is one of the channels that surfaces the prototype to the audience and recruits beta testers.

## How the site fits in the stack

```
Visitor                              (browser)
  │
  ▼
scarlot-website                      (the site repo)
  │   POST /api/contact               — phone in, qr_url out
  ▼
scarlot-admin                        (the hernest repo, served at start.scarlot.app)
  │   POST /v1/signup                 — provisions a tenant, returns qr_url
  │   /qr/<token>                     — WhatsApp pairing UI
  ▼
WhatsApp Linked devices              (visitor scans the QR on their phone)
```

The website never sees admin internals: no tenant IDs, no bearer keys in the bundle, no provisioning details. It's a strict edge — validate, forward, redirect.

## Why a backend at all

The integration contract requires a server-held bearer key (`LEAD_INTAKE_API_KEY`). The browser must never see it. The site therefore can't stay purely static — `server.js` exists to hold the secret, validate input, and forward to admin. Beyond that the site stays light: React 18 UMD + Babel-in-browser, no frontend build step.

## Current state (POC, as of 2026-05-01)

| What works                                                                        | What's deferred                                          |
| --------------------------------------------------------------------------------- | -------------------------------------------------------- |
| Visitor enters phone → backend forwards to admin → page redirects to the QR page  | OTP / SMS verification                                   |
| Backend synthesizes the `email` + `name` fields the admin contract requires       | Visitor-supplied email, company, free-text message       |
| Bearer key held server-side, never bundled                                        | Per-IP rate limit on `/api/contact`                      |
| Four locales (EN/FR/DE/ES) and IP-based country guess for the phone field         | "Send code" button label rewritten for the no-OTP flow   |
| Legacy `pair.html` still served                                                   | `pair.html` retired from the production path             |

The POC trades polish for shippable beta. We trust the testers we already know; the admin tenant gets a placeholder email derived from the phone (`<digits>@beta.scarlot.app`) so operators can correlate via the original E.164 we pass through in `message`. `source` is fixed at `marketing-site-poc`.

## What this site is NOT

- Not the WhatsApp bot — conversational logic lives in `scarlot-admin` and the per-tenant containers.
- Not the pairing UI — after signup the visitor is on `start.scarlot.app/qr/<token>`, served by admin.
- Not the source of truth for leads or tenants — lead rows live in the admin DB.
- Not a CMS-driven marketing site — copy is in `i18n.jsx`, edited in code.

## Pointers

In the `scarlot-website` repo:

- Frontend: `flow-app.jsx`, `i18n.jsx`, `messaging-mock.jsx`, `tokens.css`, `index.html`
- Backend: `server.js`
- Run instructions, env, POC trade-offs: `README.md`

In the `hernest` repo (scarlot-admin):

- Integration contract (the website ↔ admin protocol): `docs/website-signup-integration.md`

In this repo (`scarlot-poc`):

- Original landing-page spec: `docs/poc/specs/website/Landing Page - Spec.md`

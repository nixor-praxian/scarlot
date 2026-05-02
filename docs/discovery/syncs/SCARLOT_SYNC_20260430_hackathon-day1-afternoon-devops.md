---
date: 2026-04-30
participants: [Andrew, Daniel, Philippe]
duration: ~17 min
type: technical consultation
tags: [hackathon, devops, multi-tenant, hosting, networking, tls, isolation]
---

## Narrative

Andrew opened the call by asking Daniel for a sounding board on the deployment topology for Scarlot's POC. He laid out two halves of the architecture as he had been thinking about them. The first half is a set of isolated tenants, each with its own SQLite database and its own process, running side by side on a single VM but sharing nothing across the boundary. The second half is a separate control plane that provisions tenants on demand. The control plane lives on its own host, has no exposed inbound ports on the tenant side, and connects outward to a Docker host on the tenant VM via TLS or Tailscale.

Daniel pushed back on three layers of the design.

The first was SQLite. He let it pass for the POC but flagged it: SQLite as a multi-tenant store will hit IO and concurrency problems before Andrew expects, and it does not handle multi-threaded writes cleanly. For a POC where each tenant has its own file and the volume is low, it is fine. As soon as Scarlot wants any cross-tenant query or any meaningful write concurrency, it needs to move to Postgres. Andrew agreed and parked the migration as a known follow-up.

The second was networking. Daniel walked through three options for how the control plane talks to tenants. Option one is to give every tenant a direct path to the control plane via the public network, which is simple but creates a public-facing surface that needs to be defended. Option two is to put the tenants on a private network that only the control plane can reach, which is cleaner but adds a layer of overlay networking (Tailscale was the candidate). Option three is for the control plane to call into the host directly via SSH or a Docker socket and operate on the tenants from inside, which has the smallest exposed surface but the broadest blast radius if compromised. Andrew was leaning toward option two with Tailscale; Daniel agreed it was the right call for the POC and easy to harden later.

The third was TLS. Andrew had been planning self-signed TLS between control plane and tenants. Daniel pushed back hard on this. Self-signed TLS works but it requires the calling code to bypass certificate verification, which is a footgun in any non-trivial codebase: somebody will copy the bypass into a context where it should not be, and the failure mode will be silent. Daniel's recommendation was to drop TLS for the POC and use IP allowlisting plus rate limiting on the tenant side instead. The control plane gets a known IP, the tenant accepts requests only from that IP, and the request rate per IP is capped. This stack is simpler, less footgun-prone, and adequate for the threat model at the POC's scale. If the team later wants TLS, the right way is to terminate it in a sidecar (Traefik, Caddy, or an init container with proper certs) rather than in the application code.

The IP allowlisting plus rate limiting recommendation extended to the public side too. Daniel was emphatic: rate limit everything, return 429 on abuse, do not expose any unrate-limited endpoint to the public. This is the cheapest defence and the one most often skipped in POCs.

Andrew flagged one specific networking constraint he had been thinking about. The tenant exposes a QR code briefly during onboarding so the user can pair their WhatsApp via Bailey's. After pairing, the tenant has no reason to expose anything externally; everything is outbound from the tenant. This means the tenant can be locked down to almost zero inbound surface in steady state, with only a transient onboarding endpoint. Daniel agreed this was a clean pattern and easier to secure than the typical always-on web service.

The conversation drifted into Daniel's own work on Enote, an iOS and Android app for oenologists that uses voice input to capture field notes. The relevance to Scarlot was tangential but real: Daniel had built a robust offline mode (he had consulted Serge and David himself for offline-first architecture) and he had crossed two hundred users with about thirty paying. Philippe noted he had recently invested in a wine business and asked Daniel to follow up.

The session closed with a half-serious GPU-mutualisation aside. Philippe has a DGX Spark, Andrew has GPUs at home, Daniel pays for tokens. Daniel referenced the SETI distributed-computing project and suggested the three of them build a private GPU pool. Parked as a recurring joke that may become serious.

## Decisions

**Use Tailscale for the control plane to tenant network path.**
Mesh networking via Tailscale is the V1 choice for connecting Scarlot's control plane (which provisions tenants) to the tenant hosts. This avoids exposing the tenant API to the public internet and removes the need for self-signed TLS in the application. The control plane authenticates to the Tailscale network with its own identity; the tenant accepts traffic only from the Tailscale tailnet.
Confidence: PROBABLE. Revisit if Tailscale becomes a single point of failure or if the team needs cross-cloud connectivity that Tailscale does not handle well.

**Drop self-signed TLS for the POC; rely on IP allowlisting plus rate limiting instead.**
TLS via self-signed certificates requires verification bypass in calling code, which is a footgun. The cheaper and safer V1 choice is IP allowlisting on the tenant side (only accept requests from the control plane's known IP) plus per-IP rate limiting (return 429 on abuse). If TLS becomes necessary later, terminate it in a sidecar (Traefik, Caddy, or an init container with valid certs), not in the application.
Confidence: CERTAIN for V1. Revisit at production hardening time.

**Rate limit every public endpoint from day one.**
Any public-facing endpoint must have per-IP rate limiting and 429-on-abuse behaviour. This is the cheapest defence against trivial denial-of-service and against credential-stuffing-style probes, and it is the most commonly skipped step in POCs.
Confidence: CERTAIN.

**Use SQLite for the POC; migrate to Postgres before V2 scale.**
Per-tenant SQLite is acceptable for the hackathon and the early beta because each tenant's data fits comfortably in a single file and write concurrency is low. SQLite will not survive multi-tenant cross-query workloads or higher write concurrency, so the migration to Postgres is on the roadmap before V2 scaling.
Confidence: CERTAIN for V1 use of SQLite. PROBABLE for the migration timing.

**Tenant exposes only a transient QR endpoint during pairing; locked down otherwise.**
The Bailey's pairing flow needs an exposed QR endpoint during initial connection; after pairing, the tenant has no reason to expose anything externally. The deployment pattern reflects this: the tenant has zero inbound surface in steady state and a transient endpoint only during onboarding (or re-pairing).
Confidence: CERTAIN.

## Context Shifts

**The control plane plus tenant pattern survives review.**
Daniel did not push back on the structural separation between control plane and tenants. The pattern is sound; the questions were about implementation details (TLS, networking, database). This is a useful confirmation: the architectural shape we landed on in the morning session holds up under DevOps scrutiny.

**Footgun avoidance trumps theoretical purity.**
Andrew had been leaning toward TLS-everywhere as the secure-by-default posture. Daniel reframed: in a small team building a POC, TLS-everywhere with self-signed certs creates more surface for human error (verify-bypass copied into the wrong place) than it removes for attack surface. IP allowlisting plus rate limiting is the more honest engineering choice at this scale. The same logic will apply to other "best practice but expensive in dev time" choices the team will face during the hackathon.

**Voice-first as a cross-vertical pattern, not a Scarlot quirk.**
Daniel's Enote product is voice-first for oenologists. Joséphine's framing of voice-first for TDS came from a different vertical. The pattern is broader than either of them. This reinforces the morning and noon-david sessions' framing of Scarlot as an entry point for a larger "solo professional administrative reduction" thesis.

## Action Items

- [ ] Set up the Scarlot Tailscale tailnet for control plane to tenant connectivity — Andrew
- [ ] Add per-IP rate limiting to every public-facing endpoint of the agent and the safety service from day one — Andrew, Philippe
- [ ] Document the tenant deployment pattern (transient QR endpoint, locked down in steady state) so the production-hardening pass has a clear target — Andrew
- [ ] Choose the Swiss VPS host for control plane and tenants (Hetzner was mentioned, Infomaniak and Exoscale earlier). The decision needs to land before the end of the hackathon — Both
- [ ] Plan the SQLite to Postgres migration window for V2; do not push it to "when there is time" — Andrew
- [ ] Connect Daniel with Philippe's wine investment contact (separate from Scarlot) — Philippe

## To Think About

- Whether the GPU pooling idea (Philippe's DGX Spark plus Andrew's home GPUs plus Daniel's hosted compute) is worth a serious sprint at any point. It would not help the Scarlot POC directly but it could materially reduce inference costs at the platform level if Scarlot ever runs hosted models.
- The trade-off between SQLite simplicity and Postgres operational weight at the migration point. Litestream as a middle option (SQLite with continuous replication to S3-compatible storage) might extend SQLite's useful life by an order of magnitude.
- Whether to adopt Daniel's offline-first patterns from Enote. The TDS use case is mostly online (WhatsApp requires connectivity) but the agent state could be more resilient if the tenant container can survive transient network loss without losing user data.

## Open Questions

- Which Swiss VPS host wins on price, network quality, and bare-metal availability? Hetzner is German but cheap; Infomaniak and Exoscale are Swiss with the right legal posture but more expensive. Decision required.
- Is Tailscale's Swiss legal posture clean enough for nFADP compliance, or does the team need to run a self-hosted alternative (Headscale)? Worth checking.
- What is the actual rate-limit threshold per endpoint? Has to be calibrated against real cohort traffic before beta.
- How does the control plane authenticate to the tenant beyond Tailscale-network membership? A shared API key over the tailnet is simple; mTLS over the tailnet is stronger; OAuth-style is overkill for the POC.

## Key Quotes

> "Si on est dans une approche POC, le TLS, je le drop."
> — Daniel, on the TLS-versus-IP-allowlist trade-off

> "Rate limit, c'est important aujourd'hui. Si le mec en fout, tu le bloques, tu lui envoies 429 dans les dents. C'est le basique."
> — Daniel, on rate limiting as a non-negotiable day-one feature

> "Putain de SQLite. Tu vas avoir des problèmes. Non, mais s'en fout, c'est le POC."
> — Daniel, on the SQLite trade-off for the POC stage

## Connections

- **Morning sync (architecture split)** — the control-plane plus tenant pattern is the deployment-side counterpart to the morning's two-service architecture (scarlot-poc plus scarlot-safety-data). The control plane provisions; the tenants run.
- **Noon-david session (user_id scoping)** — Daniel's tenant-isolation pattern is the infrastructure-level enforcement of the application-level user_id scoping. Two layers of defence on the same property.
- **Noon-harness session (sandbox-per-query)** — sandboxes inside the tenant; tenants inside the host; control plane outside. Three concentric layers of isolation, each cheap to add and each useful in its own threat model.
- **A10 (nFADP compliance achievable)** — the rate-limiting and IP-allowlisting practices materially help the operational posture for nFADP. None of them are sufficient alone but together they form the baseline.

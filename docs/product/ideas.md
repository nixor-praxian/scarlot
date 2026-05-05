# Product Ideas Log

Append-only log of product ideas. Most recent on top. Each entry: date, one-line title, short body. Promote to a proper spec when an idea earns serious work.

---

## 2026-05-06 — SMS emoji safety lookup

Send a phone number to a Scarlot number by SMS. The only thing you get back is an emoji.

The interaction has zero learning curve and minimal data exchange in either direction. The emoji encodes the verdict (e.g. green / yellow / red, or category-specific). No app, no chat thread, no account required to perform the lookup itself. Works on any device with SMS, including burner phones, and leaves no app trace.

Open: how the responder service knows who is allowed to query (auth model), how rate-limiting works, whether the emoji vocabulary fits the existing safety category set (dangerous / health_risk / fraud / etc.), and whether SMS is acceptable under nFADP for sensitive-data lookups.

# WhatsApp contact export options

**Date:** 2026-05-07
**Context:** Many independent service providers keep their full client roster as WhatsApp threads with unsaved phone numbers, never adding the contact to the device address book. Bootstrapping Scarlot's contact base from a phone's address book therefore misses the largest part of the roster. This note catalogues the practical paths to export those WhatsApp-only contacts.
**Linked friction:** F4 in `docs/discovery/syncs/Recordings/2026-05-06_08_26 - whatsapp project feedbacks josephine/friction-points.md` (no fuzzy matching, no synchronisation with the WhatsApp repertoire) and the co-founder's voice-note thread T7 in the prior transcript decomposition.

---

## Constraint: WhatsApp does not expose a built-in export

There is no first-party feature in the WhatsApp app, on WhatsApp Web, or in the Business API that lets a user download "all the phone numbers I have a thread with." The Cloud API supports the inverse direction (uploading a list of numbers to verify which ones are on WhatsApp), not the download case. So every viable path goes around the lack of a primary export.

---

## Option 1: WhatsApp Web with a browser extension

The fastest practical path. The user opens `web.whatsapp.com` in Chrome (linking the phone), installs an extension, and runs an extraction across the chat list, a specific group, or labelled chats. Output formats are typically CSV, Excel, JSON, or vCard.

Candidate extensions found on the Chrome Web Store as of 2026-05-07:

- Contact Extractor for wa
- WAXP, Contacts Exporter for WhatsApp
- WAContactSaver
- SharedContacts (browser tool plus web wizard)

Capabilities and limits:

- Captures every thread and group visible in the WhatsApp Web session, including unsaved numbers.
- Output is essentially `phone number plus thread title or last-seen name`. No profile picture, status, or last-seen metadata. That is a WhatsApp privacy boundary, not a tool limitation.
- Several extensions support filtering to "unsaved only" or to specific groups.

Risks specific to the Scarlot user:

- Granting an unvetted extension access to a working WhatsApp session is a meaningful privacy risk. The extension sees every thread in the session window. Before recommending one to a beta user, we should review code, permissions, telemetry, and the publisher.
- Some of these extensions are paid or freemium. The free tier is often capped at a few dozen contacts.
- Extensions update frequently and may stop working when WhatsApp Web changes its DOM. Versions documented in this note are current at the date above and should be re-checked before each beta cohort.

---

## Option 2: Browser DevTools on WhatsApp Web

Same approach as Option 1 without third-party code. The user opens DevTools on `web.whatsapp.com`, runs a small console snippet that walks the chat list nodes and extracts the number plus title. Output is a CSV pasted out of the console.

Tradeoffs:

- No extension to vet. The user is in full control of what runs.
- Requires a co-pilot (us or the co-founder) to walk the user through it. Not a self-serve flow.
- Same metadata limits as Option 1.
- A reusable snippet maintained in the Scarlot repo would let beta users run the export without granting any extension access. Worth prototyping if Option 1 turns out to have privacy concerns we cannot resolve.

---

## Option 3: Per-chat export and number parsing

Inside WhatsApp on the phone, "Chat, then more, then Export chat" produces a `_chat.txt` like the one we processed for the recent project-feedbacks log. For one-to-one chats with unsaved numbers, the header includes the number; for group chats, message author lines surface every member's number.

Tradeoffs:

- No third-party tooling. Every byte stays on the user's device until they choose to send it.
- Tedious at scale. A roster with hundreds of threads would take hours to export one chat at a time.
- Useful as a fallback for a small subset of high-value contacts the user wants to seed manually.

---

## Option 4: WhatsApp Business API and the Cloud API

The Cloud API supports a "contact upload" endpoint where a business uploads phone numbers to check which ones are reachable on WhatsApp. This is the inverse of what we need: it requires the list to already exist. Not a fit for the bootstrap problem, but worth keeping in mind for later flows where Scarlot wants to verify that a freshly-entered number is reachable before sending a message.

---

## Implications for Scarlot

1. The bootstrap path for an early beta user is Option 1 plus a human cleanup pass on the resulting CSV. Names in the export are rarely the names the user actually uses for the contact in their head; the user has to relabel before the file becomes the canonical contact table.
2. Before recommending an extension, we should pick one, read its source if available, audit its permissions, and document a step-by-step that minimises session exposure (for example, run the extraction in a fresh browser profile, then revoke the WhatsApp Web link).
3. Option 2 (DevTools snippet) is the privacy-cleanest path and is worth a small implementation effort if we want to maintain control over the import experience without depending on third-party extensions.
4. None of these paths give us anything richer than "phone number plus best-guess label." Profile photos, status, last-seen, and group membership beyond the visible chat list remain inaccessible.
5. Whatever path is chosen, the CSV becomes a one-time bootstrap input. From that point on, ongoing capture has to come from inside the bot's conversation with the user, not from re-scraping WhatsApp.

---

## Sources

- [Export WhatsApp Contacts: Extract Contacts from WhatsApp Group](https://sharedcontacts.com/blog/how-to-export-whatsapp-contacts)
- [Contact Extractor for wa, Chrome Web Store](https://chromewebstore.google.com/detail/contact-extractor-for-wa/chhclfoeakpicniabophhhnnjfhahjki)
- [WAXP, Contacts Exporter for WhatsApp, Chrome Web Store](https://chromewebstore.google.com/detail/waxp-contacts-exporter-fo/mdpelimehdooponahfdneckpfnooebii)
- [WAContactSaver, Chrome Web Store](https://chromewebstore.google.com/detail/wacontactsaver/nolibfldemoaiibepbhlcdhjkkgejdhl)
- [How to export your contacts from WhatsApp, bot.space](https://www.bot.space/blog/how-to-export-your-contacts-from-whatsapp)
- [Export WhatsApp Contacts to CSV, freeviewer.org](https://www.freeviewer.org/blog/export-whatsapp-contacts-to-csv/)
- [About contact upload, WhatsApp Help Center](https://faq.whatsapp.com/1191526044909364)

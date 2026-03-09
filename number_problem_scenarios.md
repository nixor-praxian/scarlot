Model A: One central Scarlot number

  - Scarlot runs on a single phone number (e.g. a Swiss SIM bought for the project)
  - Clients write to that number
  - The bot figures out which TDS they're trying to reach (how? a code? a link? "Hi I'd like to book
  Marie"?)
  - Problem: this is basically building a switchboard. Clients don't want to talk to a platform, they want
  to talk to "Marie." And the TDS doesn't want clients knowing a shared number — it breaks the personal
  illusion.

  Model B: One number per TDS (Scarlot provides)

  - Scarlot buys/provisions a number for each TDS
  - Each number runs its own NanoClaw instance (or a multi-tenant version)
  - The TDS advertises this number on their listings
  - Client writes to "Marie's number" and gets the AI assistant
  - Problem: cost (a SIM per TDS), operational complexity, and the TDS already has a number that clients
  know

  Model C: The TDS's own number

  - The TDS connects their own WhatsApp to NanoClaw (scans the QR code)
  - The bot now handles ALL incoming messages on their existing number
  - Clients write to the same number they already find on ads/listings
  - The TDS manages the bot from a "main" self-chat group (NanoClaw's admin channel)
  - Problem: the TDS loses direct WhatsApp access on their phone while baileys is connected. You can't have
   WhatsApp Web AND baileys on the same account simultaneously... actually, baileys registers as a linked
  device, so the phone keeps working. But it's the TDS's personal/work number — risky if the bot
  misbehaves.
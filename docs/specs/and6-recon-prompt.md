# Claude Chrome - And6 Recon Prompt

Paste this into Claude Chrome while you have the And6 page open that you want mapped. Run it once per distinct page type (listing, profile, forum thread, comment page, search results, etc.). Each run produces a structured report we feed into the `and6` adapter.

---

## Prompt

You are doing **reverse-engineering reconnaissance** on the And6 page currently open in this browser tab. The goal is to produce a structured report that a separate engineer can use to build a Playwright scraper targeting this page type. Do not modify the page. Do not log in. Do not click destructive controls. You may scroll, hover, expand collapsed elements, and follow read-only links inside the same domain if needed to understand structure.

### Context (what we are building)

We are building `scarlot-safety-data`, a service that scrapes safety-relevant information from sex-worker platforms and exposes a reverse phone-number lookup API. We are not interested in worker-profile data on And6 (a sibling project already covers that). We **are** interested in any surface where a worker, a moderator, or another visitor reports, warns about, or comments on a **client**, especially anything containing a phone number, a handle, or a behavioral tag (no-show, time-waster, aggressive, non-payment, health risk, drugs, dangerous).

If the page you are on does not contain client-side safety content, say so explicitly in section 1 and still produce sections 2-7 for whatever the page does contain. We may need to navigate elsewhere on And6 to find the right surface.

### What to produce

Output a single Markdown report with these sections in this exact order. Use fenced code blocks for selectors, JSON, and HTML samples.

#### 1. Page identification

- **URL of the current page** (full).
- **Page type** in plain English (e.g. "regional girls listing", "single profile detail page", "forum thread", "comment list", "search results"). One sentence.
- **Why this page is or is not relevant to client-safety scraping.** One paragraph. Be honest if it is not.
- **Locale / language** (de, fr, it, en) and any visible language switcher.
- **Auth state** (anonymous? logged in? gated content visible?). If anything is paywalled or login-walled, list what.

#### 2. URL pattern

- Generalize the current URL into a template using `{placeholders}`.
- List query parameters that change behavior (filters, pagination, sort).
- If pagination is JS-driven rather than URL-driven, say so and describe the trigger.
- Sibling URL patterns reachable from this page that look relevant (e.g. "from a profile page, a `/comments` tab links to ..."). List up to 10.

#### 3. DOM map

For every meaningful data field on the page, give:

| Field name | CSS selector (preferred) or XPath | Sample value | Notes |
|---|---|---|---|

Cover at minimum, where present: title, body text, author/poster handle, post date, location/city, embedded phone numbers, embedded other contact identifiers (email, Telegram, Snapchat handles), category/tag chips, severity/status indicators, reply count, view count, attached images.

If a phone number appears anywhere - in a heading, body, footer, image alt text, structured-data block, or HTML comment - flag it explicitly with the selector and the surrounding text.

If structured data is present (JSON-LD, microdata, OpenGraph, embedded `<script>` JSON), dump the relevant blocks verbatim in a fenced code block.

#### 4. Pagination and navigation

- How are additional pages reached? (numbered links? next button? infinite scroll? load-more button?)
- If JS-driven, what is the click target's selector?
- How do you know you have reached the last page?
- Are there per-page or per-region listing endpoints we can iterate over?

#### 5. Anti-bot signals

Look for and report:
- Cloudflare or other CAPTCHA challenge pages encountered.
- Cookie/consent walls and their exact selectors.
- `robots.txt` directives if visible.
- `noindex`/`nofollow` meta tags.
- Suspected rate limits (any "too many requests" messaging visible).
- Whether the page renders without JS (check by inspecting whether content is in the initial HTML or hydrated client-side).
- Any honeypot links or form fields.

#### 6. Sample raw HTML

Paste **one** representative snippet (50-200 lines) showing the structural unit we will iterate over (a single forum post, a single comment, a single profile card - whichever is the unit of interest). Include the surrounding container so a scraper can identify the loop boundary.

#### 7. Recommended scraper shape

Propose, in pseudocode, the smallest Playwright-async loop that would extract this page type's data into the schema below. No real code - just enough that the engineer can translate it directly.

```
for page in paginate(URL_TEMPLATE):
    goto(page); wait_for(SELECTOR_X)
    for unit in page.locator(UNIT_SELECTOR):
        yield {
          phone_raw: ...,
          category: ...,
          comment: ...,
          city: ...,
          reporter_handle: ...,
          reported_at: ...,
          source_url: ...
        }
```

Flag any field in the schema we cannot fill from this page type.

#### 8. Open questions for the human

A short bulleted list of things you could not determine from one page (e.g. "is there a member-only section behind this login?", "are phone numbers visible to anonymous users on profile pages?", "does the forum exist on this domain at all?"). Keep it actionable.

### Style rules

- Be terse. We do not need prose around the data.
- Selectors must be copy-pasteable. Prefer stable CSS attributes over nth-child positions.
- If you are uncertain about a value, mark it `?` rather than guessing.
- No screenshots needed; the structured report is the artifact.
- Do not include personally identifiable information about workers in the sample HTML. Redact names and any phone numbers in worker profiles. Phone numbers that appear in a **client-safety context** should be preserved (that is the data we are trying to capture); flag them clearly.

### When done

End the report with a single line:

```
RECON COMPLETE - <page type> - <URL>
```

So we can grep across multiple recon runs.

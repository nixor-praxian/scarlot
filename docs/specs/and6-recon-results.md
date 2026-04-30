# And6 Recon Report — Client Blacklist (escort dashboard)

## 1. Page identification

- **URL:** `https://www.and6.com/my/escort/client-blacklist/escorts-list`
- **Page type:** Authenticated escort dashboard view of a site-wide "Client Blacklist" — a paginated flat list of bad-client reports submitted by other escorts, plus a sibling "Alerte de l'admin" tab for admin-issued warnings.
- **Relevance to client-safety scraping:** **Highly relevant — this is exactly the surface scarlot-safety-data targets.** Each row is a client report keyed on a partially-masked phone number, with a free-text behavioural complaint (no-show, time-waster, aggressive, fake, manipulative, blocking, price pressure, etc.), a date, often a city, often a name/handle, and frequently a fully-formatted phone number repeated inside the comment body. The list is global across And6 (not scoped per profile), making it a high-yield reverse-phone source.
- **Locale / language:** UI is **fr** (`<html lang="fr">`; "Rapports", "Afficher les photos", "29 avr., 2026"). User-submitted comments are mixed de / en / fr / it. A language switcher button labelled `FR` (`button.sc-language-button`) opens a `mat-menu`. Body has class `notranslate`; the page warns logged-in users that an installed online translator may break it.
- **Auth state:** **Authenticated escort session.** The path lives under `/my/escort/...`. Anonymous users hitting this URL are login-walled. Gated content includes: the entire blacklist listing, the search input, the "Ajouter Client" submission button, the "Afficher les photos" image reveal, and the chat (which additionally requires a paid online profile — message "Chat est désactivé / Tu dois avoir un profil online pour utiliser le chat"). No paywall on the blacklist itself once logged in.

## 2. URL pattern

- **Template:** `https://www.and6.com/my/escort/client-blacklist/{tab}` with `{tab} = escorts-list` for this page. The "Alerte de l'admin" Material tab may map to `admin-alerts` (?) but its URL was not confirmed since switching tabs may not push history.
- **Query parameters:** None observed. Pagination, search query, and tab state are held entirely in Angular component state, not in the URL. There is no `?page=`, no `?q=`, no `?city=`.
- **Pagination:** **JS-driven, not URL-driven.** Clicking page numbers updates the DOM in place; `location.href` does not change. See section 4 for click targets.
- **Sibling URL patterns reachable from this page that look relevant:**
  - `/my/escort/client-blacklist` (parent route stub)
  - `/my/escort/client-blacklist/escorts-list` (current — escort-submitted reports)
  - `/my/escort/client-blacklist/admin-alerts` (?) — inferred from second `mat-tab` "Alerte de l'admin"
  - `/my/escort/support` — possible moderation channel (?)
  - `/comments/deutschschweiz` — public surface listed in `robots.txt` as `Disallow`; almost certainly profile/visitor comments. **Strong candidate for a separate recon run.**
  - Other `/my/escort/*` left-menu links (`profile`, `tours`, `daily-ads`, `banners`, `girl-of-the-day`, etc.) are not safety-relevant per scarlot scope.

## 3. DOM map

The list is an Angular SPA. Each row is `<sc-escort-blacklist-item>`. Attributes `_ngcontent-rks-c1XX` hash per build — **do not select on `_ngcontent-*` attributes**. Class names appear stable.

| Field | Selector (relative to `sc-escort-blacklist-item`) | Sample value | Notes |
|---|---|---|---|
| Row container | `sc-escort-blacklist-item` | — | Iterable unit. 20 rows per page. |
| Phone (masked, row key) | `.item-number span` | `+417975*3503` | **PHONE NUMBER, client-safety context — flag.** Always masked at digits 7-8 with `*`. Country prefix preserved. |
| Date | `.item-date > span.mb-4` (first span) | `29 avr., 2026` | Localised French date `DD MMM., YYYY` with abbreviated month. |
| City / location | `.item-date span.ng-star-inserted` (second span in `.item-date`) | `Aarau`, `Visp`, `Berne`, `Winterthur`, `Chur`, `Zurich`, `Niederglatt`, `Hallwil`, `Altendorf` | Optional — many rows have no city. |
| Name / handle | `.message > div.text-secondary.text-medium.fs-16` | `Andre`, `felix`, `Caeser`, `Yanick`, `Thomas`, `Andi`, `„Jürg" Jeremias`, `Patrick`, `Oli`, `Resu`, `Francesco`, `Faeba`, `No name` | Free text. **Sometimes contains a fully-formatted phone instead of a name** (next row). |
| Phone (unmasked, in body header) | `.message > div.text-secondary.text-medium.fs-16` | `+41 79 752 35 03`, `+41 76 512 63 68`, `+41 79 732 42 32` | **PHONE NUMBER, client-safety context — flag.** Same number as `.item-number`, unmasked. Primary value to scarlot. |
| Comment body | `.message > span.word-break` (also matches `.message span.text-secondary.fs-16.text-normal`) | `"Andre carrot dangler timewaster. A friend has seen him..."` | Full behavioural report. **May contain additional embedded phones** and inline behavioural keywords: `no show`, `timewaster`, `aggressive`, `fake`, `blocked`, `Preisdrücker` / price pressure, `manipulieren`, `gratis`. Mixed languages. |
| "Show photos" toggle | `button` containing text `Afficher les photos` inside `.message` (?) | `Afficher les photos` | Some rows attach client photos. Image URL pattern: `https://i.and6.com/{escort_id}/blacklisted_client/{uuid}_profile.jpg?v=1`. |
| Reporter handle | — | **Not exposed** | Reports are anonymous to readers. Cannot be scraped. |
| Severity / status | — | **Not present** | No chips, no upvotes, no resolved flag. Tab-level differentiation only (`Rapports` vs `Alerte de l'admin`). |
| Reply count / view count | — | **Not present** | Not a threaded surface; flat list. |
| Category / tag chips | — | **Not present as structured chips** | Behavioural categories must be inferred from comment body (regex / NLP). |

**Phone-number flag (explicit):**
- `.item-number span` — masked phone, every row, e.g. `+417975*3503`
- `.message > div.text-secondary.text-medium.fs-16` — sometimes an unmasked phone, e.g. `+41 79 752 35 03`
- `.message > span.word-break` — comment body may contain additional phone numbers inline

**Structured data:** None present. No `<script type="application/ld+json">` (`hasJsonLd: false`). No microdata, no OpenGraph beyond defaults. The only meta tags are `viewport` and an empty `google-site-verification`. No `<meta name="robots">`. The page is a pure Angular SPA — server returns a ~3.3 KB shell, all content is hydrated client-side.

## 4. Pagination and navigation

- **Mechanism:** numbered pagination, JS-driven. Container: `div.sc-pagination-container > div.navigation`.
- **Page-number click target:** `span.page-item.with-icon-state` (text content is the page number).
- **Active page indicator:** `span.page-item.with-icon-state.active`.
- **First / Prev / Next / Last buttons:** `span.page-item.actions.with-icon-state`, in DOM order: double-back (`i.sc-double-arrow-back`), back (`i.sc-arrow-back`), next (`i.sc-arrow-next`), double-next (`i.sc-double-arrow-next`). Disabled state: extra class `.disabled`.
- **Last-page detection:** the next and double-next actions both gain `.disabled`. Confirmed by jumping to last page: total **518 pages**, last page contained 15 rows ⇒ ≈ 517 × 20 + 15 ≈ **10 355 entries** at recon time.
- **URL never changes** during pagination — Playwright must click and wait for the row list to refresh; deep-linking to page N is not possible via URL.
- **Per-region listing endpoints:** none. The list is global; city is a display field. The only filter is a free-text search input `input[placeholder^="Recherche par numéro"]` which matches phone, name, or body text.
- **Backing XHR not observed.** After patching `window.fetch` and `XMLHttpRequest.prototype.open`, neither pagination clicks nor search input fired any captured request, suggesting the full list (or a large window of it) is loaded once on route entry and paginated client-side. **Caveat:** the initial XHR happened before patching — a fresh-load capture with DevTools open at navigation is required to confirm the underlying API endpoint and whether it is reusable directly (would be much faster than DOM walking 518 pages).

## 5. Anti-bot signals

- **Cloudflare:** present (`https://static.cloudflareinsights.com/beacon.min.js/...` loaded). No challenge encountered this session, but Cloudflare bot-management is in front of the domain.
- **Cookie / consent wall:** none on this authenticated route. May appear on the public site.
- **robots.txt** (full body):
```
User-agent: *
Disallow: /en
Disallow: /contact
Disallow: /private
Disallow: /page
Disallow: /*filter_
Disallow: /adstool
Disallow: /so/
Disallow: /fxo/
Disallow: /images_anzeige
Disallow: /is/
Disallow: /escorts/happy-hours
Disallow: /p/prices
Disallow: /comments/deutschschweiz
Disallow: /search
Disallow: /escorts/regular
Disallow: /p
Disallow: /member
```
`/my/` (the dashboard, including this blacklist page) is **not** disallowed but is auth-gated. Note `/comments/deutschschweiz` and `/member` are disallowed — respect them. `/comments/...` is the public comment surface most relevant to scarlot's wider scope.
- **noindex / nofollow:** no `<meta name="robots">` on this page.
- **Rate-limit messaging:** none observed.
- **JS rendering:** **mandatory.** Raw `fetch('/my/escort/client-blacklist/escorts-list')` returns a 3 316-byte Angular shell with no `blacklist-item` strings. `requests`/cheerio will not work; Playwright (or equivalent headful/headless browser) is required.
- **Honeypots:** none seen. The `id="goog-gt-*"` inputs in the DOM are noise from an injected Google Translate widget the page itself warns against; ignore inputs with `id` starting `goog-gt-`.
- **Translation-detection:** the site warns logged-in users that an installed online translator interferes with functionality. Run Playwright with translator extensions disabled.

## 6. Sample raw HTML

Representative iterable unit. The phone number is preserved verbatim because it appears in a **client-safety context** per instructions.

```html
<sc-escort-blacklist-item _ngcontent-rks-c136="" _nghost-rks-c135="" class="ng-star-inserted">
  <div _ngcontent-rks-c135="" class="blacklist-item d-grid">
    <div _ngcontent-rks-c135="" class="blacklist-info d-flex">
      <div _ngcontent-rks-c135="" class="text-secondary text-medium fs-14 item-name mb-4 ng-star-inserted">
        <div _ngcontent-rks-c135="" class="item-number d-grid ng-star-inserted">
          <span _ngcontent-rks-c135="" class="ng-star-inserted"> +417975*3503 </span>
        </div>
      </div>
      <div _ngcontent-rks-c135="" class="text-gray fs-14 mb-4 text-normal d-grid item-date">
        <span _ngcontent-rks-c135="" class="mb-4">29 avr., 2026</span>
        <span _ngcontent-rks-c135="" class="ng-star-inserted"> Aarau </span>
      </div>
    </div>
    <div _ngcontent-rks-c135="" class="message">
      <div _ngcontent-rks-c135="" class="text-secondary text-medium fs-16 mb-4 ng-star-inserted">
        +41 79 752 35 03
      </div>
      <span _ngcontent-rks-c135="" class="text-secondary fs-16 text-normal word-break ng-star-inserted">
        Andre carrot dangler timewaster. A friend has seen him and he was very aggressive with face fucking. You need strong boundaries and a lot of patience
      </span>
    </div>
  </div>
</sc-escort-blacklist-item>
```

Loop boundary: rows live inside `sc-escorts-list > div.sc-loader-overlay`. The Playwright loop selector is `sc-escort-blacklist-item`.

A second variant — row whose body header is a *name* instead of an *unmasked phone* — is structurally identical except `.message > div.text-secondary.text-medium.fs-16` contains text like `Caeser`, `Yanick`, `No name`, etc. The scraper must accept both shapes and regex-match for phones in either field.

## 7. Recommended scraper shape

```
# URL_TEMPLATE = "https://www.and6.com/my/escort/client-blacklist/escorts-list"
# Auth required. Pagination is JS-only; no ?page= param.

context = new_browser_context(storage_state=AND6_AUTH_STATE)   # cookie jar from a prior manual login
page = context.new_page()
goto(URL_TEMPLATE)
wait_for("sc-escort-blacklist-item")

current = 1
while True:
    wait_for("sc-escort-blacklist-item")
    rows = page.locator("sc-escort-blacklist-item").all()
    for row in rows:
        masked   = row.locator(".item-number span").inner_text().strip()
        date_str = row.locator(".item-date span.mb-4").inner_text().strip()
        city     = row.locator(".item-date span.ng-star-inserted:not(.mb-4)").inner_text_or_none()
        head     = row.locator(".message > div.text-secondary.text-medium.fs-16").inner_text_or_none()
        body     = row.locator(".message > span.word-break").inner_text().strip()

        # head may be a name OR an unmasked phone
        phones = regex_find_all(r"\+?\d[\d\s\-]{6,}\d", " ".join([head or "", body]))
        name   = head if head and not is_phone(head) else None

        yield {
            "phone_raw":       unmask_or_extract(masked, phones),   # prefer unmasked match against masked pattern
            "phone_masked":    masked,
            "category":        infer_tags(body),                    # regex/NLP: no_show, timewaster, aggressive, fake, ...
            "comment":         body,
            "city":            city,
            "reporter_handle": None,                                 # NOT EXPOSED on this surface
            "reported_at":     parse_fr_date(date_str),              # "29 avr., 2026" -> 2026-04-29
            "name_or_handle":  name,
            "source_url":      URL_TEMPLATE + f"#page={current}",    # synthetic; real URL doesn't change
        }

    next_btn = page.locator("span.page-item.actions:has(i.sc-arrow-next)")
    if "disabled" in (next_btn.get_attribute("class") or ""):
        break
    next_btn.click()
    wait_for_dom_settle()
    current += 1
```

**Schema fields we cannot fill from this page type:**
- `reporter_handle` — never displayed; reports are anonymous to readers.
- A stable per-row `source_url` — no permalink and no `?page=` param; we must synthesise (`#page=N`) or use offset within the global list.
- `category` as a structured value — only inferable from free text.
- `severity` — does not exist on this surface.
- Reply / view counts — flat list, not a thread.

## 8. Open questions for the human

- **Auth strategy:** scarlot needs a logged-in escort account to access this list. Is account creation/maintenance in scope, or should we look for a different surface? (I will not create the account myself.)
- **Public alternative:** does an unauthenticated version of the client blacklist exist (e.g. `/client-blacklist` without `/my/`)? Worth a quick anon check — high value if yes.
- **Backing API:** is there a JSON endpoint behind this list (likely something under `/api/...` or `/my/escort/...`)? A fresh-load network capture with DevTools open at navigation would reveal it and could replace DOM walking 518 pages.
- **`/comments/deutschschweiz` and similar `/comments/*` routes:** robots.txt disallows one such path, strongly implying a public per-region comments surface exists. Recon that next — likely the second-most-valuable surface for client-safety data on And6.
- **Admin-alerts tab:** does the second `mat-tab` ("Alerte de l'admin") expose different fields (severity, expiry, scope) worth a separate scraper?
- **"Ajouter Client" submission form:** what fields does it require? Knowing this tells us the canonical schema And6 itself uses (phone format, category enum if any, attachments, etc.) — useful for normalising scraped output.
- **Photo attachments:** does "Afficher les photos" require an extra request (lazy-load), and are those URLs auth-gated or public on the `i.and6.com` CDN?
- **Per-row stable IDs:** is there a hidden `id` attribute or data-attribute on `sc-escort-blacklist-item` not visible in this snapshot (worth re-inspecting the full element with all attributes)? Without one we can only de-dupe on `(masked_phone, date, body_hash)`.

```
RECON COMPLETE - escort dashboard client blacklist - https://www.and6.com/my/escort/client-blacklist/escorts-list
```
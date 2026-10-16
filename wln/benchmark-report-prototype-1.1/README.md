# WeLearn interactive report: developer handoff

Learning Strategy Benchmarking Data Report, Second Edition (August 2026).
A finished, production-ready page for WeLearnLS.com (WordPress). **Place `index-production.html` on the site as it is. Don't rebuild it in Elementor.**

## What's here

- `index-production.html`: the clean, production-ready report (no review notes or review toggle). Its version and build time are in a comment and a `version` meta tag at the top of the file.
- `index-review.html`: the same report with yellow review notes and the "Review notes" toggle, for internal review only.
- Both files contain the whole report. CSS and JavaScript are inline; all styles are scoped under `.wl`. Plain JavaScript, no framework or build step.
- One small unscoped rule sits just before `</head>`: `html,body{margin:0;padding:0}`. It removes the browser's default 8px page margin. Most themes already do this; keep it only if the page template you use doesn't, or a thin white border appears around the cover.
- `assets/img/`: section photos (FPO, pulled from the PDF at 1200px; swap in the high-resolution originals) and `share-1200x630.jpg` (draft link-preview image).
- `assets/img/people/`: speaker and author headshots, cropped to 4:5 (640×800). Sean Stowers and Loren Sanders are FPO crops from the PDF until better web versions are found.
- `assets/logo/`: WeLearn logos (SVG).

## Implementation

The report is one self-contained page: markup, CSS and JavaScript are all in `index-production.html`, styles are scoped under `.wl`, and it needs no Elementor widgets, plugins or build step. The job is to place it on a WordPress page, not to rebuild it.

1. **Use a blank page template.** The report has its own full-screen cover, sidebar menu and progress bar, so it runs without the site's header and footer. Use a full-width blank template: Elementor's "Canvas" template with the markup in one HTML widget, or a small custom PHP page template that prints the file's `<body>` contents. With the theme header in place, the sticky sidebar and the full-height cover sit wrong.
2. **Keep the markup, styles and script together and unchanged.** Copy everything inside `<body>` (the `.wl` wrapper, the dialogs and the `<script>`) plus the `<style>` block from the head. Don't split the CSS into Elementor's styling panels or re-create sections as widgets.
3. **Head tags go to the SEO plugin.** Move the title, meta description, canonical, Open Graph and X tags, citation and Dublin Core tags, and the JSON-LD into Yoast / Rank Math or the template head, so nothing is duplicated (see "SEO, AEO and sharing" below). Keep the Google Fonts link for Plus Jakarta Sans unless the theme already loads it with weights 400–800.
4. **Assets.** The file refers to images as relative paths (`assets/img/...`, `assets/logo/...`). Either upload the `assets` folder so those paths resolve from the page, or upload the images to the Media Library and update the paths (search the file for `assets/`). Point `og:image` at the uploaded share image's full URL.
5. **Check for theme bleed.** The report's styles can't leak out, but global theme styles (headings, `p`, buttons, links, `img`, tables, `dialog`) can reach in. During QA, compare the live page with `index-production.html` opened locally; where something differs, raise the report's selector specificity or remove the theme rule from this template rather than editing the report's design.
6. **Scripts.** Keep the report's `<script>` at the end of the body. Don't let optimization plugins defer, combine or minify it in a way that changes load order, and exclude the page from JavaScript "delay until interaction" settings so the navigation and charts work on first load.
7. **Deploy only `index-production.html`.** `index-review.html` is for internal review and shouldn't go live.

## Before launch

1. **Config.** Edit `WL_CONFIG` at the top of the script: PDF URL, HubSpot form IDs, scheduler URL. (index-production.html already has the review notes and toggle removed.)
2. **Review highlights (index-review.html only).** Anything not word-for-word from the Word doc carries a `data-review` note: added visuals, PDF-only content, new UI copy, copy checks. Review mode is on by default in the prototype. Add `?review=0` to the URL to hide it. Decide on each item, then delete the attribute.
3. **Placeholders.** The three "Learn more about" page URLs carry `data-todo`. (LinkedIn URLs are in place.)
4. **Ben Sieke.** He's shown in the contributors list for review. An HTML comment marks his list item; delete it to remove him at launch, and remove his Person entry and `contributor` reference from the structured data.
5. **Launch date.** Replace every `YYYY-MM-DD` with the launch date: `article:published_time`, `article:modified_time`, and the WebPage `datePublished` / `dateModified` in the structured data. The edition date (August 2026) stays on the Report, citation and Dublin Core tags.
6. **Page URL.** The canonical link, share tags and structured data use the placeholder `https://welearnls.com/ebook/learning-strategy-benchmarking-data-report-2026/`. Update all occurrences, and point the `og:image` at the uploaded media URL.

## HubSpot

- **Forms.** Both use the developer embed (`js.hsforms.net/forms/embed/developer/8668897.js`), so fields render as page HTML and are restyled by the report CSS. In HubSpot, leave form styling at its defaults.
  - PDF download: `6df702ec-6307-4b8b-bc56-f24672de3831` (new form, no redirect). Keep "Display a thank-you message" on. The report shows its own confirmation with the PDF link.
  - Contact (Let's talk, "Send a message" tab): `ed9a5aec-d6df-4089-92c0-4666aa942d07`. This is WeLearn's existing site contact form. After a send, the report hides the form and shows its own confirmation ("Thanks, your message is on its way."), so the form's HubSpot thank-you or redirect setting doesn't matter here. If that setting is a redirect, confirm during live QA that the page stays put.
  - A new form only needs its form ID changed (in `WL_CONFIG` and on the `.hs-form-html` element).
- **Submit detection.** Listens for `hs-form-event:on-submission:success`, with a fallback to the legacy `hsFormCallback` message. Confirm the event fires with the live form during QA.
- **Repeat downloads.** After one submission, later Download clicks go straight to the PDF link (stored in that browser). Install the HubSpot tracking code on the page as planned.
- **Scheduler.** The HubSpot meetings embed loads only when the Let's talk modal opens. Its colors come from the scheduling page settings in HubSpot.
- **Loading.** The scripts load on demand, the first time a modal needs them.

## Future editions

The report is published every six months; the Third Edition is scheduled for February 27. When a new edition launches, update the edition line, the Report `version` and `datePublished`, the citation and Dublin Core dates, and the page dates. Recommended (10/14, pending team sign-off): one page per edition, so each edition keeps its own links, citations and search history; each new edition links back to the previous one.

## SEO, AEO and sharing (drafts for WeLearn to edit)

In WordPress, move the head tags into the SEO plugin fields (Yoast / Rank Math) or the page template, so they aren't duplicated (Implementation, step 3).

- **Basics:** title, meta description, canonical, robots and language.
- **Sharing:** Open Graph and X cards with the 1200×630 image.
- **Structured data (JSON-LD):** WebPage (with speakable), Report (authors, publisher, dates, edition, the PDF, and every section as `hasPart` with its own anchor and description), Dataset, Organization and Person.
- **Citation and archive tags:** `citation_*` tags (Google Scholar, Zotero) and Dublin Core tags.
- **Readable structure:**
  - one `h1`, with the heading order kept intact
  - every page and section has an `id` and a "copy link" button
  - figures are real `<table>`s with captions, so answer engines can read and quote the numbers
- **Print:** the print stylesheet expands the Appendix and hides navigation.

## Behavior

- **Navigation:**
  - The sidebar slides in after the cover.
  - It stays open until the reader clicks Hide, and reopens from Menu. The choice is remembered in that browser (it never opens or closes by itself).
  - Tablets (960–1279px) start with the mini strip until the reader opens it.
  - Below 960px, a top bar and a bottom sheet replace the sidebar.
- **Motion:** in-page links (contents, mini strip, progress bar, section links) scroll with a slow ease-in-out (0.9–1.8s depending on distance). Scrolling the wheel or touching the screen cancels it. Hovers, contents groups, dialogs and the Appendix also ease. All motion is off with reduced motion turned on.
- **Progress bar:** phones and small tablets only (below 960px). Visible by default after the cover; readers can hide it and the choice is remembered. On desktop it's hidden by CSS (`@media (min-width:960px)`). A refined desktop version is parked for v2.
- **Progress (cont.):** shows the current page; ticks fill through Sections 1–11.
- **Accordions:** the Section 11 priorities and the Section 9 "WeLearn take" factors are numbered toggles (the first one starts open; several can be open at once). Without JavaScript, and in print, every panel shows.
- **Charts:** animate once as they scroll into view. Stat numbers count up. Hover tooltips repeat values that are already in the tables. With reduced motion turned on, everything renders in its final state. Without JavaScript, all content is visible.
- **Section band color:** change one class on `.wl-band` (default `wl-band--orange`). Options:
  - `--amber`, `--yellow`, `--beige`, `--beige-light`, `--yellow-light`
  - `--pink`, `--blue-light`, `--purple`
  - `--maroon`, `--green-dark`, `--blue-dark`, `--black`

  Text colors adjust automatically for contrast.

## Figure text standard

Every figure, chart, table and Appendix table uses the same five text styles. Keep to these when adding figures:

| Role | Style |
| --- | --- |
| Figure caption | 12px semibold, caps |
| Column headers | 12px semibold, caps, charcoal |
| Supporting text (meta, notes, legends, axis ticks) | 12px regular, charcoal |
| Row and category labels | 14px medium |
| Values | 16px semibold |
| Stat-card captions | 14px |
| Tooltips | 12px, with a 14px semibold value |

Column alignment: labels left; counts (Respondents, Organizations, Rank) and ranges (Figure 18, rating numbers) centered (`.ctr`); measured values right so decimals line up. On phones, tables stack and everything aligns left; rows with three values (Figures 2–12 question scores, 14, 15, 18) show each label above its value in three even columns. Narrow layouts are handled with container queries, so they also apply when the sidebar is open: Where to start rows stack below 620px, the Section 7 "what moved" card stacks with a down arrow below 300px, and the Section 10 band cards become compact rows below 440px. The four Executive summary stat cards show 4 across when the content column is 760px or wider, 2×2 below that, and stacked below 300px (never 3 + 1). The three-card stat rows (Sections 3–8) show 3 across at 560px or wider and stack below that (never 2 + 1). When cards in a full row are narrower than about 200px, the stat numbers drop from 48px to 40px.

## Spacing standard

Scale: 8 · 16 · 24 · 32 · 48 · 80.

| Between | Desktop | Phone |
| --- | --- | --- |
| Topic blocks (`.wl-block`) | 80 | 56 |
| Heading → first content | 24 | 16 |
| Text → text, lists, stat cards | 24 | 16 |
| Text ↔ figure, quote, callout or boxed visual | 48 | 32 |
| Page title → lead → first content | 28 / 48 | 28 / 32 |

| Box | Used for | Desktop padding | Phone |
| --- | --- | --- | --- |
| Large | pullquotes, scorecard CTA | 48 × 56 | 28 × 22 |
| Standard | callouts, takeaways, figure boxes, Where to start rows, author cards | 32 × 36 | 24 × 20 |
| Small | stat cards, band cards, contributor cards | 24 × 24 | 20 × 18 |
| Ask your team | | 16 × 20 | 16 × 20 |

## Analytics

Events are pushed to `window.dataLayer` and fired as a `wl:track` DOM event, all prefixed `wl_report_`:

| Area | Events |
| --- | --- |
| Reading | `page_view`, `chart_view`, `read_report`, `progress_hide`, `progress_show` |
| Navigation | `nav_collapse`, `nav_expand`, `copy_link`, `logo` |
| Download | `download_open`, `download_submit`, `pdf_download` |
| Let’s talk | `talk_open`, `talk_tab`, `contact_submit` (fires when the confirmation shows) |
| Links | `scorecard_cta`, `scorecard_link`, `learn_more`, `linkedin` |
| Appendix | `appendix_open` |
| Accordions | `accordion_open` |
| Modals | `*_close` |

Consent is handled by the site's existing cookie banner and GTM.

## Accessibility (WCAG 2.2 AA target)

- skip link, landmarks and labelled navigation
- native `<dialog>` modals: focus stays inside, Esc closes, focus returns to the button that opened them
- keyboard-operable tabs, plus disclosure widgets in the Appendix
- visible focus states and alt text on every image
- color contrast checked per band color

Test with VoiceOver and NVDA before launch.

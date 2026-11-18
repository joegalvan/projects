# WeLearn Phase 2: "Let's talk" modal, developer handoff

Prepared November 18, 2026. Copy is locked. Mockup and prototype were approved on November 17.

The purple CTA buttons on welearnls.com open one shared modal instead of going to the Contact us page. The modal holds a HubSpot form ("Send a message") and a HubSpot scheduler ("Book a time"). The button's link decides the header copy, the pre-selected topic, which tab opens first, which scheduler loads and the thank-you message. 

## What's in this folder

| File | What it is |
| --- | --- |
| `wl-talk-modal.html` | The production code: styles, dialog markup and script in one block. Paste it into Elementor Custom Code. |
| `README.md` | This guide. |
| `WeLearn - Phase 2 Modal Copy.docx` | The locked copy, by topic. The code already contains this copy. |
| `lets-talk-modal-prototype.html` | The approved clickable prototype, updated to the locked copy. Open it in a browser. It uses a stand-in form and calendar. |
| `ai-enablement-mockup.html` | The approved AI Enablement page mockup with the modal. |

## 1. HubSpot setup (done by JG, verify only)

- Portal `8668897`, region `na1`.
- Form `1ff52ef3-d343-4293-b050-9ea9a3527a30`, a clone of the site contact form without Phone and "How did you hear about us?".
- Fields: First name, Last name, Work email, Company, "CTA discussion topic", the default `message` field (label "What's on your mind?", optional) and a hidden "CTA label" field.
- After submit: "Display a message" with the text "Thanks for reaching out. Someone from the WeLearn team will be in touch as soon as possible." Visitors only see this if the modal's own thank-you can't show. Do not switch it to a redirect.
- Nurture stream and follow-up messages are disabled during production
- JG to activate workflow at time of publishing. 

**Contact properties**

| Label | Internal name | Type |
| --- | --- | --- |
| CTA discussion topic | `cta_discussion_topic` | Dropdown select |
| CTA label | `cta_label` | Single-line text, hidden on the form |

**Dropdown values** (the code sends the internal value; labels can change later)

| Label | Internal value |
| --- | --- |
| General question | `general` |
| An L&D project | `project` |
| Strategy Maturity Assessment | `maturity-assessment` |
| Alignment Sprint coaching | `alignment-sprint` |
| Learning strategy (SOAP or Strategy Workshop) | `learning-strategy` |
| AI Enablement | `ai-enablement` |
| Learning Measurement | `measurement` |
| FRAME™ / FOAP call | `frame` |
| Help choosing where to start | `recommendation` |
| Something else | `other` |

**Schedulers**

| Key in code | Used by topics | Scheduler |
| --- | --- | --- |
| `general` | general, project, recommendation | meetings.hubspot.com/seans5/sean-s-scheduling-page- (also on Contact us) |
| `strategy` | maturity-assessment, alignment-sprint, learning-strategy | meetings.hubspot.com/seans5/strategy-journey-meeting (round robin) |
| `ai` | ai-enablement | meetings.hubspot.com/seans5/ai-enablement-consultation- (round robin) |
| `frame` | measurement, frame | meetings.hubspot.com/seans5/foap-scheduler- (round robin) |

Scheduler links must not carry tracking parameters (`__hstc`, `__hssc`, `__hsfp`).

## 2. Install the code

1. In WordPress, go to **Elementor > Custom Code > Add New**. Name it "Let's talk modal".
2. Paste all of `wl-talk-modal.html`.
3. Set **Location** to `</body> - End`, **Priority** to 1, and **Display conditions** to Entire Site. Publish.
4. Do not wrap it in an Elementor Popup. The code is already a modal, and a popup would put a modal inside a modal.
5. Clear the page cache (and any CDN cache) after publishing.

The script only acts on buttons with the `wl-modal-cta` class. Nothing else on the site changes. It loads the HubSpot form script only when a visitor first opens the modal, and the scheduler only when they open "Book a time".

**Fonts and brand.** The modal uses Plus Jakarta Sans from the site, purple `#8754EF` for buttons, deep purple `#7A41ED` for hover and focus, and orange `#EF5617` for the eyebrow.

## 3. Set up each button

Every button needs two things in Elementor:

- **Link** (Content tab): `/contact-us/?topic=<topic>`, adding `&v=<variant>` where the table says so.
- **CSS Classes** (Advanced tab): `wl-modal-cta`

Elementor puts the class on the widget wrapper, not the link. The script handles both. Keep the existing button labels unless the table says otherwise. The script sends the label to HubSpot as `cta_label`, so a label change shows up in the data.

If JavaScript fails, or the visitor Ctrl/Cmd-clicks, the link still goes to Contact us.

### CTA inventory

| Page | Button (location) | Link | Opens on |
| --- | --- | --- | --- |
| What we do `/what-we-do/` | Let's talk (hero) | `/contact-us/?topic=general` | Message |
| What we do | Discuss your needs | `/contact-us/?topic=project` | Message |
| Services & Consulting `/what-we-do/services-and-consulting/` | Hero button | `/contact-us/?topic=project` | Message |
| Services & Consulting | Discuss your needs | `/contact-us/?topic=project` | Message |
| Content & Program Development `/what-we-do/content-and-program-development/` | Hero button | `/contact-us/?topic=project` | Message |
| Content & Program Development | Discuss your needs | `/contact-us/?topic=project` | Message |
| Workforce Development `/what-we-do/workforce-development/` | Let's talk (hero) | `/contact-us/?topic=project&v=workforce` | Message |
| Workforce Development | Discuss your needs | `/contact-us/?topic=project&v=workforce` | Message |
| How we help `/how-we-help/` | Hero button | `/contact-us/?topic=general` | Message |
| Why WeLearn | Hero button | `/contact-us/?topic=general` | Message |
| Maturity Assessment `/how-we-help/learning-strategy-maturity-assessment/` | Learn about the Maturity Assessment (hero) | `/contact-us/?topic=maturity-assessment` | Message |
| Maturity Assessment | Discuss your needs | `/contact-us/?topic=maturity-assessment` | Message |
| Maturity Assessment | Talk to us about coaching | `/contact-us/?topic=alignment-sprint` | Message |
| AI Enablement `/how-we-help/ai-enablement/` | Talk to us about AI enablement (hero) | `/contact-us/?topic=ai-enablement` | Message |
| AI Enablement | Discuss your needs | `/contact-us/?topic=ai-enablement` | Message |
| Learning Measurement `/how-we-help/learning-measurement/` | Let's talk about measurement (hero) | `/contact-us/?topic=measurement` | Message |
| Learning Measurement | Discuss your needs | `/contact-us/?topic=measurement` | Message |
| Learning Measurement | Book a call (FOAP section) | `/contact-us/?topic=frame` | Book a time |
| Pricing `/pricing/` | Book a call (hero) | `/contact-us/?topic=recommendation` | Book a time |
| Pricing | Get a recommendation | `/contact-us/?topic=recommendation` | Book a time |
| Pricing | Talk to us about coaching (Alignment Sprint) | `/contact-us/?topic=alignment-sprint` | Message |
| Pricing | Talk to us about your strategy (SOAP card) | `/contact-us/?topic=learning-strategy&v=soap` | Message |
| Pricing | Talk to us about your strategy (Strategy Workshop card) | `/contact-us/?topic=learning-strategy&v=workshop` | Message |
| Pricing | Discuss one-to-one coaching | `/contact-us/?topic=ai-enablement&v=one-to-one` | Message |
| Pricing | Discuss your project | `/contact-us/?topic=project&v=pricing` | Message |

Notes:

- The Pricing hero "Book a call" currently opens the FOAP scheduler. Point it at `recommendation` as above. The label "Book a call" stays.
- Optional, from the copy doc: relabel the Maturity Assessment hero to "Request the assessment", since it now opens a request form.
- **Out of scope for this build:** the header navbar "Let's talk" (keeps linking to `/contact-us/`), and the Learning Strategy Scorecard landing page. Both are proposed for the next round.

## 4. How it works

- **Opening.** A click on a `wl-modal-cta` button reads `topic` and `v` from its link, fills in the header copy, and opens a native `<dialog>`. An unknown topic falls back to `general`.
- **Form.** On first open the script loads HubSpot's developer embed (`js.hsforms.net/forms/embed/developer/8668897.js`). When the form is ready (`hs-form-event:on-ready`), and on every later open, the script sets:
  - `cta_discussion_topic` to the topic value, so the dropdown is pre-selected (visitors can change it)
  - `cta_label` to the clicked button's text
  - the message field's label and example text, and the submit button text, to the topic's copy
- **Thank-you.** On `hs-form-event:on-submission:success` the modal hides the form and shows the topic's own thank-you, with the visitor's first name when HubSpot provides it. A HubSpot form can only be sent once per page view. If the visitor opens the modal again after sending, they see HubSpot's fallback message in the form area, and they can still book a time.
- **Scheduler.** "Book a time" loads the topic's scheduler as an iframe (`<scheduler URL>?embed=true`), resized from HubSpot's height messages. When HubSpot reports a booking (`meetingBookSucceeded`), the `frame` and `recommendation` topics show their own "You're booked" message. Other topics keep HubSpot's own confirmation. HubSpot also sends its calendar invite.
- **Accessibility.** Focus stays inside the modal. Esc, the close button and a click on the backdrop close it. Focus returns to the button that opened it. The tabs work with the arrow keys. Motion is turned off for visitors who prefer reduced motion. On phones the modal is a bottom sheet.
- **Copy changes.** All copy and links sit in the `WL_TALK` config at the top of the script. Edit there; no other code changes are needed. To add a button blurb, add an entry under that topic's `variants` and use `&v=<name>` in the link.

## 5. Analytics

Each event goes to `window.dataLayer` (Google Tag Manager) and fires a `wl:track` DOM event. Every event carries `topic`, `variant` and `cta_label`.

| Event | When |
| --- | --- |
| `wl_talk_open` | Modal opens (adds `first_tab` and `page`) |
| `wl_talk_tab` | Visitor switches tabs (adds `tab`) |
| `wl_talk_submit` | HubSpot form sent |
| `wl_talk_booked` | Meeting booked (adds `scheduler`) |
| `wl_talk_thankyou_click` | Primary thank-you button clicked (adds `label`) |
| `wl_talk_close` | Modal closes |

HubSpot already records the page URL and the form submission on the contact. GTM triggers are optional and can be added later.

## 6. QA checklist

Test on a staging copy or on a private draft page first. A test submission creates a real HubSpot contact, so use a test email and delete the contact afterwards.

**HubSpot wiring (the parts we could only check against a mock)**

- [ ] The form renders inside the modal and the loading text disappears.
- [ ] The dropdown is pre-selected to the button's topic. Open from two different buttons in a row and check it changes.
- [ ] The submitted contact shows the right `cta_discussion_topic` and `cta_label` in HubSpot.
- [ ] The message field label, example text and submit button text match the topic. If HubSpot re-renders and overwrites them, note which ones, and we'll adjust.
- [ ] After submit, the modal shows the topic's thank-you with the first name, not HubSpot's fallback.
- [ ] Each of the four schedulers loads for its topics, and the iframe grows to fit without a double scrollbar.
- [ ] Booking from the FRAME™ "Book a call" and the Pricing "Book a call" shows the custom "You're booked" message.
- [ ] The HubSpot contact owner receives the notification email.

**Every button**

- [ ] Each row of the CTA inventory opens the right header and the right first tab.
- [ ] The five blurbs (workforce, pricing, soap, workshop, one-to-one) show their own header copy.
- [ ] Thank-you buttons go to the right pages: case studies, Mass General Brigham, 3M, AI use-case finder, FRAME™, Learning Strategy Scorecard.

**Behavior and devices**

- [ ] Esc, the close button and a backdrop click close the modal; focus returns to the button.
- [ ] Tab key stays inside the modal; arrow keys switch tabs.
- [ ] Ctrl/Cmd-click on a button opens Contact us in a new tab.
- [ ] Phone (about 390px wide): bottom sheet, nothing cut off, the page behind doesn't scroll.
- [ ] Latest Chrome, Safari (macOS and iOS), Firefox and Edge.
- [ ] `wl_talk_*` events appear in GTM preview or the browser console (`dataLayer`).
- [ ] The header "Let's talk" still goes to `/contact-us/`.

## 7. Open items

- Follow-up emails: HubSpot sends one automatic email per form. Per-topic emails would need a workflow that branches on `cta_discussion_topic`. Not in this build.
- Optional later: replace the Alignment Sprint message box with checkboxes for the six areas.
- Next round: header navbar "Let's talk" to the modal, the Learning Strategy Scorecard page "Let's talk" button with Sean's scorecard scheduler (new topic `scorecard`), and removing pasted tracking IDs from that page's "Get access" buttons.

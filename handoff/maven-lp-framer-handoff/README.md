# Maven Pro Black Friday landing pages: Framer handoff

Three landing page concepts for Maven Analytics' Black Friday Pro offer, built as a whole-concept test so one or two clear winners can be picked. All three are finished as HTML and have had round 1 of client feedback (Jason, Loom review) applied. This package is what's needed to rebuild them in Framer.

**Read `OPEN-ITEMS.md` before anything goes live.** The checkout links don't charge the right price yet.

---

## 1. The three pages

| # | Concept | Folder | Live preview (noindex) |
|---|---|---|---|
| 01 | **Annual only.** One offer, one price, push Pro Annual. | `pages/01-annual/` | https://thomasbrosch.github.io/maven-pro-annual-lp/ |
| 02 | **Video sales page (VSL).** Video first, then the 3 plans. Light background. | `pages/02-vsl/` | https://thomasbrosch.github.io/maven-pro-annual-lp/plans/ |
| 03 | **3 plans + comparison.** The 3 plans plus a matrix against other ways to learn. Dark background. | `pages/03-compare/` | https://thomasbrosch.github.io/maven-pro-annual-lp/plans-compare/ |

Each `index.html` is self-contained: CSS, images and scripts are inside the file. Double-click it to open it in a browser, offline. The live previews are the same files.

### Headlines
- 01: **Become the analyst AI can’t replace**
- 02: **Watch: how analysts are using AI to get ahead, not get replaced**
- 03: **More than a course. Costs less than you’d think.**

### Section order

| 01 Annual | 02 VSL | 03 Compare |
|---|---|---|
| Sticky header (price + CTA) | Sticky header | Sticky header |
| Hero: text banner, photo 2/3 + offer card 1/3 | Hero: headline, **16:9 video**, CTA, "compare all 3 plans" link, AMA pill | Hero: text + small photo, then **3 plan cards** |
| Logo strip | Logo strip (dark band) | Problem → fix |
| Problem → fix (2×2 cards) | Problem → fix | **Comparison matrix** |
| Testimonial carousel | **3 plan cards** | Logo strip |
| Everything you get (3×3 cards) | Testimonial carousel | Testimonial carousel |
| Proof stats + J.B. Hunt case + quote | Everything you get | Everything you get |
| 5 steps (AI for Data Analysts path) | Proof | Proof |
| Real human experts (7 headshots) + small comparison | 5 steps | 5 steps |
| Price breakdown: "Get the annual plan for $239 today" | Real human experts | Real human experts |
| FAQ | Why Annual is the best value | Why Annual is the best value |
| Close: "Lock in your Black Friday price" | FAQ | FAQ |
| Mobile sticky bar (≤520px) | Close | Close |

On mobile, the order inside the hero is: text, then the offer or plans, then the photo. On 02 and 03, the **Annual card shows first** on mobile.

---

## 2. Package contents

```
maven-lp-framer-handoff/
├── README.md                 this file
├── OPEN-ITEMS.md             blockers + questions for Maven
├── pages/                    the 3 finished pages (self-contained HTML)
├── screenshots/              full-page desktop (1440px) + mobile (390px) for each page
└── assets/
    ├── photos/               hero photo (Unsplash, page 01 and 03)
    ├── logos/                KPMG, Pfizer, Mitsubishi, Caterpillar (light logos on a #262626 background, not transparent; better to use Maven's own logo files in Framer)
    ├── headshots/            7 people, 184px, on the brand gradient (as used)
    │   └── cutouts-transparent/   the same 7 as full-size transparent PNGs from Maven's team page
    └── testimonials.json     the 14 carousel quotes + featured quote, verbatim, with source
```

---

## 3. Design

**Brand:** Lato (400/700/900), background `#262626`, teal `#26d7d7`, lime `#e4ec84`, gradient `linear-gradient(90deg, #26d7d7, #e4ec84)`.

| Token | Dark (01, 03) | Light (02) |
|---|---|---|
| Page background | `#262626` | `#f5f7f7` |
| Card | `#303030` | `#ffffff` |
| Band / alt section | `#1c1c1c` | `#e9eeee` |
| Text | `#ffffff` | `#1c1c1c` |
| Body text | `#d6d6d2` | `#3d4444` |
| Muted | `#a0a09a` | `#667070` |
| Lines | `#444444` | `#d8dede` |
| Accent text | `#26d7d7` | `#0b8582` (darker, for contrast on white) |
| Buttons | `#26d7d7`, dark text `#0e1a1a` | same |

- Content width is 1040px with a 16px side gutter. Breakpoints are 860px (stack to one column) and 520px (mobile sticky bar appears).
- **Primary CTA:** solid teal button with a glow and an arrow. Monthly and Lifetime use outline buttons, so Annual is the only solid button in the plan cards.
- **Annual plan card:** "BEST VALUE" badge, 2px teal border with glow, biggest price, lime "Save $349 vs paying monthly" pill.
- **Comparison marks:** teal ✓ = included, red ✕ = not included, amber badge = partial ("Varies", "Rare", "Forums"). There's a legend above the matrix. The "Live workshops every week" row is highlighted with a lime outline and a "Maven standout" pill, per Tom.
- **Testimonial carousel:** cards scroll on their own (90s loop), pause on hover or focus, and become a static swipe row when the visitor has turned on reduced motion.

---

## 4. Offer, prices and links

| Plan | Shown | Fine print (matches Maven's pricing page) |
|---|---|---|
| **Pro Annual** | ~~$399~~ **$239** first year · Under $20 a month, billed annually · Save $349 vs paying monthly | Offer not available for existing annual subscribers. Total due today is $239 ($160 off). Plan renews at $399/year. Cancel anytime. |
| Pro Monthly (02, 03) | ~~$49~~ **$37**/mo for your first 3 months, then $49/mo | Offer not available for existing subscribers. Plan renews at $49/mo after 3 months. Cancel anytime. |
| Pro Lifetime (02, 03) | ~~$1,699~~ **$999** one time · No renewals, ever | One-time payment of $999 ($700 off). No renewals. |

- Label: **"Black Friday price"**. This is the price for people who did *not* register for AI Analyst Week. That group gets a lower price elsewhere.
- Bonus: "Join by Mon, Nov 16 and get a seat in a private AMA with Chris Dutton".
- **Annual checkout (every Annual CTA):** `https://fe.mavenanalytics.io/signup/plan/annual-pro-399?promotionCode=<PROMO>&type=pro`. The code in the files now is the **old** one. See OPEN-ITEMS.
- **Monthly / Lifetime buttons** link to `#plans` as placeholders, marked `TODO` in the HTML.
- Do **not** use `checkout.stripe.com/c/pay/cs_live_…` links. Those are single-use sessions.

---

## 5. Tracking

- **GTM only:** container `GTM-N4QRGZQ`, the same container as mavenanalytics.io. Don't add separate pixels, or events will count twice. If the Framer site already loads this container, don't load it again on these pages.
- **CTA clicks:** every buy link has `data-cta="<placement>"`. A click pushes `{event: 'cta_click', cta_location: '<placement>'}` to `dataLayer`.
  - Placements: `header`, `hero`, `video`, `plan-annual`, `plan-monthly`, `plan-lifetime`, `stack`, `outcomes`, `compare`, `price`, `close`, `mobile-bar`.
  - **Keep these names** in the Framer build so results can be compared across the 3 pages.
- **UTM / click-ID passthrough:** `utm_*`, `fbclid`, `gclid`, `ttclid` and `li_fat_id` on the landing URL are copied onto every `data-cta` link. The script is at the bottom of each HTML file.
- All three pages are `noindex,nofollow`.

## 6. Video (page 02)

The `#video` block holds a 16:9 placeholder. Put any YouTube, Vimeo or Wistia **embed** URL in its `data-embed` attribute and the page swaps in the player. In Framer, use a Video or Embed component in the same 16:9 frame (max width 860px, full width on mobile). Plans stay visible; nothing is hidden until a point in the video.

---

## 7. Content rules (please keep)

- **Testimonials are verbatim** from Maven's public testimonial wall: `assets/testimonials.json`. Don't edit them, fix typos in them, or add star ratings (ratings weren't captured per review).
- **FAQ answers** (except the two price questions) are copied word for word from mavenanalytics.io/pricing, including the refund policy and help-center links.
- **Instructor names and titles** come from Maven's Meet the Team page.
- **No competitor names.** The comparison uses categories: video course marketplace, online certificate subscription, data bootcamp, AI chat tutor.
- **No made-up stats, reviews or per-item dollar values.** Third-party stats are labelled with source and "Oct 2026". See OPEN-ITEMS #4.
- Copy style: "real instructors" / "real human experts", not "named instructors" or "not hallucinations" (client feedback).

## 8. Changes from client feedback, round 1

| Feedback | Change |
|---|---|
| "AI Analyst Week price" is the wrong label | Now "Black Friday price" everywhere. Removed the line saying the offer is for AI Analyst Week registrants. |
| Too many lists | Value stack → 3×3 card grid (2-col on mobile). Steps → 5-step row. Problem/solution → 2×2 cards. |
| "Learn from humans, not hallucinations" | → "Learn from real human experts" |
| "Named instructors" | → "Real instructors" (copy + both tables) |
| "One plan. One price. Plain billing." reads as AI | → "Get the annual plan for $239 today" |
| "Less than you'd expect" undersells the product | → "More than a course. Costs less than you’d think." |
| Hard to tell ✓ from ✕ | Colored marks + legend |
| FAQs not from Maven's page; refund policy | Replaced with Maven's own pricing-page answers |
| (Review) a featured quote had been edited | Restored to Rui Carreira's verbatim review |

Source repo: https://github.com/thomasbrosch/maven-pro-annual-lp. Full history is in the `main` branch.

# Prompt: rebuild the Maven Black Friday landing pages in Framer

> **How to use:** start a new Claude conversation and attach the `maven-lp-framer-handoff` folder or zip (at minimum the 3 `index.html` files, the `screenshots/` folder, `README.md` and `OPEN-ITEMS.md`). Paste everything below the line. If Claude is connected to Framer (for example through a Framer MCP / plugin), it can build directly. Otherwise it guides you step by step while you build.
>
> Fill in the **[brackets]** before sending, or delete the lines that don't apply.

---

You're helping me rebuild three finished landing pages inside Maven Analytics' Framer site. The attached package has everything: three self-contained HTML pages (the source of truth for copy, layout and behavior), full-page desktop and mobile screenshots, extracted image assets, a README with the design tokens, prices, tracking and content rules, and OPEN-ITEMS.md with launch blockers.

**Read README.md and OPEN-ITEMS.md first, then open each `index.html` and its screenshots before building anything.**

## Goal
Build three pages in Framer that match the HTML versions in content, section order and behavior, but are **native Framer layers**: stacks, components, breakpoints and CMS where useful. Maven's team must be able to edit them in Framer later. Don't paste the HTML into an Embed component, except for the small scripts listed under "Tracking".

- Framer project: **[project name / link]**
- Page paths: **[e.g. /bf-annual, /bf-watch, /bf-compare]**
- Who's building: **[me in the Framer editor / you through the Framer connection]**

## Rules
1. **Copy is final.** Use the exact text from the HTML, including headlines, prices, fine print, FAQ answers and testimonials. Don't rewrite, shorten or "improve" it. Testimonials and FAQ answers are verbatim from Maven's own sources. If something won't fit a layout, tell me; don't edit it.
2. **Reuse what the Framer site already has.** That means the header and footer, the text styles and color styles (Lato, `#262626`, teal `#26d7d7`, lime `#e4ec84`), the button component and the logo set. Only create new components where none exist, and name them clearly (for example `LP / Plan Card`, `LP / Compare Row`).
3. **Make repeated sections into components** so the three pages stay in sync:
   - Problem → fix 2×2 cards
   - Testimonial carousel. Use a Ticker, or a CMS collection fed from `assets/testimonials.json`.
   - "Everything you get" 3×3 grid
   - 5-step row
   - Proof stats
   - Real human experts row (7 headshots, from `assets/headshots/`)
   - Price breakdown, FAQ, close
   - Plan cards (02, 03)
   - Comparison matrix (03)
   - Mobile sticky bar
4. **Page 02 uses the light theme** (README §3). Pages 01 and 03 are dark. Prefer theme variants on the shared components over duplicates.
5. **Breakpoints:** desktop 1440, tablet ≤860 (single column), phone ≤520 (sticky bottom bar appears). Match the mobile screenshots:
   - Hero order on phone is text, then offer/plans, then photo.
   - The Annual plan card shows first on phone.
   - No horizontal page scroll. The comparison table may scroll sideways inside its own frame, with the first column pinned.
6. **Checkout links:** every Annual CTA goes to `https://fe.mavenanalytics.io/signup/plan/annual-pro-399?promotionCode=[NEW PROMO CODE]&type=pro`.
   - Monthly checkout: **[URL]**. Lifetime checkout: **[URL]**.
   - If either is missing, leave the link as `#plans` and list it in your summary.
   - Never use `checkout.stripe.com/c/pay/cs_live_…` links.
7. **Video (page 02):** a 16:9 frame, max width 860px and full width on phone, with a Video/Embed component. Video URL: **[URL or "placeholder for now"]**. Keep the plans visible; don't time-gate them.
8. **SEO:** set all three pages to noindex. Page titles: "Maven Pro Annual", "Maven Pro: Watch", "Maven Pro Plans Compared".

## Tracking (must keep working)
- GTM container `GTM-N4QRGZQ`. If the Framer site already loads it site-wide, **don't add it again**, or events will double-count.
- Every buy link needs `data-cta="<placement>"` with the same names as the HTML: `header`, `hero`, `video`, `plan-annual`, `plan-monthly`, `plan-lifetime`, `stack`, `outcomes`, `compare`, `price`, `close`, `mobile-bar`. In Framer, set it as a custom attribute on the link or button.
- Add the two small scripts from the bottom of each HTML file in the page's custom code (end of body):
  1. UTM / click-ID passthrough onto `a[data-cta]`
  2. `cta_click` dataLayer push
- Tell me if Framer's link handling stops either one from working.

## How to work
1. Start with **page 01**. Build shared components as you go, then reuse them for 02 and 03.
2. After each page, compare it with its desktop and mobile screenshots and list any differences you couldn't match.
3. Ask before inventing anything: no new copy, stats, reviews, ratings, images or competitor names.
4. Finish with a short summary: what you built and where, component names, which links and items are still placeholders (check them against OPEN-ITEMS.md), and anything you'd recommend we test.

# Maven Analytics: Pro Annual LP. Claude Code handoff

**Goal:** Get the Annual-only "AI Analyst Week / Black Friday" landing page live on GitHub (GitHub Pages) today, so it can be used as a split-test variant against the current Pro page.
**Client:** Maven Analytics (Chamber Media, performance-management SOW). **Owner:** Tom Broschinsky.
**Date written:** 2026-11-05.

---

## 1. What exists

| Item | Location |
|---|---|
| Finished page (single self-contained file, images embedded as base64) | `C:\Users\tombr\OneDrive\Desktop\Clients\2_Chamber Media (SEPARATE LANE)\Maven Analytics\040 - Landing Pages\LP1 - Launch a Data Career\maven-annual-lp.html` |
| Preview (private claude.ai artifact) | https://claude.ai/artifact/KeiHAjNtihSWkQvHgSdBLh |
| This doc | same folder: `maven-annual-lp-claude-code-handoff.md` |

The HTML is the source of truth. Do not rebuild it from this doc. This doc says what to **change, wrap, and deploy**.

---

## 2. Do this first (blocking issues in the HTML)

The file is a **fragment**. It starts at `<title>` with no document skeleton, because it was authored for a preview tool. Browsers will render it in quirks mode. Before deploying:

1. Wrap it in a proper document:
   ```html
   <!doctype html>
   <html lang="en">
   <head>
     <meta charset="utf-8">
     <meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
     <meta name="robots" content="noindex,nofollow">
     <title>Maven Pro Annual</title>
     <!-- GTM head snippet here (see section 5) -->
     ...existing <link> + <style>...
   </head>
   <body>
     <!-- GTM noscript here -->
     ...existing body content (header, main, script, .mbar)...
   </body>
   </html>
   ```
2. Delete `<meta name="claude:viewport-fit-sides" ...>`. It is preview-only.
3. Keep the Google Fonts `<link>` for Lato (400/700/900).
4. Keep `noindex`. This is a paid-traffic variant and should not compete with the main Pro page in search.
5. Optional: pull the four base64 logos and the hero JPEG out into `/assets/` and reference them normally. The single file works as is; splitting only helps page weight and editing.

---

## 3. Page structure (top to bottom)

| # | Section | Notes |
|---|---|---|
| 1 | Sticky header | Brand, "4.6 from 198K Udemy ratings" (hidden on mobile), `$399 → $199.50 first year`, "Get Pro Annual" button |
| 2 | Hero | Centered banner: eyebrow, H1, subhead. Below it: photo (2/3 width) + offer card (1/3). **Mobile order: text, offer card + CTA, photo.** |
| 3 | Logo strip | KPMG, Pfizer, Mitsubishi, Caterpillar |
| 4 | Value stack | 9 included items, then price total + CTA. No per-item dollar values (none are substantiated) |
| 5 | Third-party proof | 4 stat tiles, J.B. Hunt case-study strip, Rui C. quote, then 3 dashed placeholder slots |
| 6 | Outcomes | 5 steps + CTA (light "paper" section) |
| 7 | Humans vs. hallucinations | 5 instructor placeholders + comparison table |
| 8 | Billing box | Today $199.50 / in 12 months $399 / vs. monthly |
| 9 | FAQ | 7 items (light section) |
| 10 | Closing CTA + footer | Footer links to Maven legal pages |
| 11 | Mobile sticky bottom bar | Only at <=520px |

**Hero copy**

- Eyebrow: `AI Analyst Week price · ends [DATE]`
- H1: `Become the analyst AI can't replace`
- Sub: `Unlock full access to AI for Data Analysts: 2027 Edition plus everything else in Maven Pro`
- Offer card: `Normally $399 /year` → `50% OFF` → `$199.50 your first year` → button `Get 50% off Pro Annual`
- Fine print: `Discount applied at checkout. Renews at $399/year. Cancel anytime. Offer not available for existing annual subscribers. Secure checkout with Stripe.`

---

## 4. CTA links (7 placements, one destination)

All CTAs point to the same checkout URL. Each has a `data-cta` attribute so clicks can be tracked by placement:

`header`, `hero`, `stack`, `outcomes`, `price`, `close`, `mobile-bar`.

```
https://fe.mavenanalytics.io/signup/plan/annual-pro-399?promotionCode=promo_1UQAvIBr47J2JI72rDW74VMr&type=pro
```

- Do **not** swap in the `checkout.stripe.com/c/pay/cs_live_...` links Tom pasted earlier. Those are single-use checkout sessions and will break.
- The monthly link exists but is intentionally not on this page: `.../monthly-pro-49?promotionCode=promo_1UQAudBr47J2JI72GlFhcwBY&type=pro`.
- **Verify** the promo code auto-applies on the checkout page and the displayed price is $199.50 before any traffic goes live.
- **UTM passthrough:** add a small script that appends the current page's query string (`utm_*`, `fbclid`, `gclid`, `ttclid`, `li_fat_id`) to every `a[data-cta]` href.

---

## 5. Tracking (copied from the current Maven page; confirm in GTM)

| Platform | ID |
|---|---|
| GTM container | `GTM-N4QRGZQ` |
| GA4 | `G-RNVXNX7G3B`, `G-D6PKE9HLBV`, `G-9CBEFJW9QX` |
| Meta pixel | `436515613965061` |
| TikTok pixel | `CVL3JF3C77U3ARSBEVB0` |
| LinkedIn Insight | `2020924` |

Recommended: install **only the GTM snippet** (head + noscript) and let the container fire the platform tags. Do not hard-code the individual pixels as well, or events will double-count. Confirm in GTM that these tags fire on this hostname, since GTM triggers are often locked to `mavenanalytics.io`.

Checkout lives on `fe.mavenanalytics.io`, a different subdomain from this LP host. Confirm the GA4 cross-domain `_gl` linker covers the LP host and `fe.mavenanalytics.io`, or purchases will attribute to "direct".

Add a click event on `a[data-cta]` that pushes to `dataLayer`:
```js
document.addEventListener('click', e => {
  const a = e.target.closest('a[data-cta]');
  if (a) (window.dataLayer = window.dataLayer || []).push({event:'cta_click', cta_location:a.dataset.cta});
});
```

---

## 6. Remove or resolve before real traffic

| Item | Action |
|---|---|
| `[DATE]` in the hero eyebrow and the closing section | **Blocker.** Get the real end date from Jason (Maven) and replace both. |
| FAQ "What is your refund policy?" shows a `Confirm with Jason before launch` todo | **Blocker.** Maven's public FAQ says Pro is non-refundable unless law requires. Replace with approved wording, or delete the item. |
| Three dashed `.slot` boxes (Trustpilot widget, G2/Capterra badge, 3 student quotes) | Delete all three for launch. Trustpilot is unclaimed and G2 has 0 reviews. Add student quotes back later when real ones are supplied. |
| Rui C. quote: "Add role and company before launch" | Add his role and company, or remove the quote. |
| Instructor cards show "photo" placeholders (Chris Dutton, Aaron Parry, Alice Zhao, Chris Bruehl, Dustin Cabral) | Use real headshots from Maven, or hide the card row. |
| Logo strip has only 4 logos | Optional: add Coca-Cola and Michael Kors from Maven's main-page assets. The current logos are cropped from a screenshot and are slightly soft. |
| Hero photo | Unsplash photo by Vitaly Gariev (man smiling at laptop). Free license, no credit required. Replace with a real Maven learner or instructor photo when available. |
| Stat mismatch | The current Maven page says 97% and 200K+; the client brief says 95% and 195K+. This LP uses neither. Do not add them without Maven confirming. |

---

## 7. Claims on the page and their sources

Every third-party number is labelled with its source and "Oct 2026" on the page. Re-check before launch if it has been more than a few weeks.

| Claim | Source |
|---|---|
| 4.6 stars, 198,259 ratings | Udemy, Maven's Power BI Desktop course |
| 813,901 students | Udemy, same course |
| 4.7 average, 9,793 learners, 3 courses | Coursera partner page |
| 2M+ learners | Udemy instructor profile |
| 102% increase in data skills scores, 9.5/10 employee approval | Maven's J.B. Hunt customer case study |
| $588 for 12 months of monthly | $49 x 12 |
| "$388.50 less" | $588 - $199.50 |

Constraints that must hold:
- No competitor names anywhere on the page. The comparison table uses generic columns: "Video-only course" and "AI chat tutor".
- No invented reviews, ratings, or dollar values for the stack items.

---

## 8. Brand tokens

| Token | Value |
|---|---|
| Font | Lato 400 / 700 / 900 |
| Background | `#262626` (surface `#303030`, deep `#1c1c1c`) |
| Teal (primary CTA, accents) | `#26d7d7` |
| Lime (gradient end) | `#e4ec84` |
| Gradient | `linear-gradient(90deg,#26d7d7,#e4ec84)` |
| Paper sections | bg `#f2f4f4`, ink `#232323` |
| Primary button | solid teal, dark text `#0e1a1a`, 10px radius, glow shadow, arrow icon via CSS mask |

---

## 9. Deploy to GitHub Pages

Tom has no repo for this yet. Use the `gh` CLI.

1. Create a repo (suggested name: `maven-pro-annual-lp`; ask Tom whether it should live under his personal account or an org, and whether it should be private/public. Pages on a private repo needs a paid plan).
2. Put the finished file at the repo root as `index.html` (plus `/assets/` if images were split out). Add a short `README.md` pointing to this doc.
3. Commit and push to `main`.
4. Enable Pages: `gh api -X POST repos/{owner}/{repo}/pages -f "source[branch]=main" -f "source[path]=/"` (or Settings → Pages → Deploy from branch → `main` / root).
5. Wait for the build, then confirm the live URL (`https://{owner}.github.io/{repo}/`).
6. Optional custom domain: a CNAME like `go.mavenanalytics.io` would need Maven to add the DNS record. Not required to launch the test. Note that if the LP is on a different domain from Maven, tracking and cross-domain settings in section 5 matter more.

Send Tom the live URL when it is up.

---

## 10. QA checklist (run on the live URL)

- [ ] Opens in standards mode (`<!doctype html>` present); no console errors
- [ ] Sticky header stays pinned; price + button visible
- [ ] **Mobile (375px):** order is headline, offer card + button, then photo. Offer card contents centered. No horizontal scroll
- [ ] Hero photo loads and shows his face (not cropped) at desktop and mobile widths
- [ ] Desktop: banner on top; photo about 2/3 left; offer card about 1/3 right
- [ ] All 7 CTAs open the checkout with the promo applied and **$199.50** shown
- [ ] UTMs carry through to the checkout URL
- [ ] GTM loads; `cta_click` events appear in GTM preview / GA4 DebugView
- [ ] Cross-domain linker works (`_gl` param on the checkout URL)
- [ ] FAQ items expand and collapse; mobile bottom bar appears at <=520px only
- [ ] No `[DATE]`, "Slot:", "Confirm with Jason", "Add role and company", or "photo" text left on the page

---

## 11. Open questions for Tom / Maven

1. **Offer end date** (Jason).
2. **Refund wording** (Jason).
3. Does the promo code auto-apply, or does Maven want "use code" copy? (The original page said "USE CODE" while the link auto-applies. This LP assumes auto-apply.)
4. Real student quotes with name, photo, job title, and result.
5. Instructor headshots.
6. Is there a baseline CVR for the current Pro page? None was available. It is needed to read the test.
7. Hosting: personal GitHub, org, or a Maven-owned domain?

---

## 12. Suggested test plan

- **A:** current Pro page. **B:** this Annual-only page.
- Split Meta traffic 50/50 by ad set or ad-level URL, same audiences and budget.
- Primary metric: checkout completion rate and revenue per click. Secondary: share of Annual vs. Monthly buyers.
- Hold copy changes until there is enough volume to read the result.

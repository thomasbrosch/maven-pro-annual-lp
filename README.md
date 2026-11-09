# Maven Pro Annual LP

Annual-only "AI Analyst Week" landing page for Maven Analytics, used as split-test variant B against the current Pro page.

- `index.html` (variant B, Annual only) is the whole page (self-contained, images embedded). `noindex,nofollow`.
- `plans/index.html` (variant C): Monthly / Annual / Lifetime cards, Annual highlighted as Best Value, smaller hero photo. Live at `/plans/`.
- `plans-compare/index.html` (variant D): variant C plus a category comparison matrix (generic categories, no competitor names). Live at `/plans-compare/`.
- Tracking: GTM `GTM-N4QRGZQ` only; CTA clicks push `cta_click` with `cta_location` to `dataLayer`. UTM / click-ID params are appended to every `a[data-cta]`.
- See [HANDOFF.md](HANDOFF.md) for the full spec, open items, and the QA checklist.

**Still open before real traffic:** new Annual `promotionCode` (all pages still use the old $199.50 code), Pro Monthly and Pro Lifetime checkout URLs (variants C/D use `#plans` placeholders, marked TODO), and the refund FAQ wording.

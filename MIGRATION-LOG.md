# Kansawala — 9.0.0 → 9.1.0 migration log

> Running log of porting the custom Kansawala homepage from the
> Impulse **9.0.0** build (`F:\Website Devlopment\Impulse Backup\Kansawala-Impulse-9.0.0-29-05-26`)
> to the fresh Impulse **9.1.0 Stock** theme. Each section is ported one
> at a time on top of a shared design-system foundation. For tokens/fonts
> see [BRAND.md](BRAND.md); for the section roster see
> [COMPONENTS.md](COMPONENTS.md); for working rules see [CLAUDE.md](CLAUDE.md).

## Approach
9.1.0 is a newer Impulse base than 9.0.0 (fonts via `{% render 'font-face' %}`,
CSS vars via `{% render 'css-variables' %}` with `--type*/--element-*/--color*`
names, JS via `theme.sections.register` + `is-land`). The custom layer is
ported **natively** and **additively**:

- **Fonts** (Jost + Cormorant Garamond) load via `snippets/kw-fonts.liquid`
  — they're not in Impulse's font_picker (which stays on Fahkwang for the
  stock site).
- **Design tokens** (`--d/--s/--fs-*/--fw-*/--ls-*/--lh-*/--icon-*/--dur-*`,
  brand colours) live in `assets/kw-tokens.css`. The namespace does **not**
  collide with 9.1.0's own variables, so the stock site is unaffected.
- **Typography** utilities (`.t-*`) live in `assets/kw-typography.css`,
  grown section by section.
- **Section structural CSS** stays **self-contained inline** in each
  section's `{% style %}` block (no shared `section-common.css`).
- **Section JS** is a native `theme.XxxSection` class in `assets/theme.js`,
  registered via `theme.sections.register`.

## Phase 1 — Foundation + Hero Slider ✅ (2026-05-30)
Shared foundation stood up + first section ported.

**Added**
- `assets/kw-tokens.css` — `:root` design tokens + `prefers-reduced-motion` floor.
- `assets/kw-typography.css` — `.t-*` utilities (seeded with the classes the
  hero needs; grows per section).
- `snippets/kw-fonts.liquid` — Jost + Cormorant Garamond (Google Fonts, async).
- `sections/hero-slider.liquid` — the hero carousel. Structural CSS that was in
  9.0.0's `section-common.css` is now inline, scoped to `.hero-{{ section.id }}`.
- `assets/theme.js` — `theme.HeroSlider` class (native port of 9.0.0 `initHero`),
  registered as section type `hero-slider`.

**Edited**
- `layout/theme.liquid` `<head>` — after `theme.css`: `kw-tokens.css` +
  `kw-typography.css` stylesheet tags + `{% render 'kw-fonts' %}`.
- `assets/theme.js` `DOMready` block — `theme.sections.register('hero-slider', theme.HeroSlider)`.

**Native adaptations vs the 9.0.0 source (design unchanged)**
- Section root: `data-kw-hero*` → `data-section-id` + `data-section-type="hero-slider"`
  + `data-autoplay` + `data-speed` (matches `slideshow.liquid`).
- Each slide carries `data-block-id="{{ block.id }}"` so the theme editor's
  `onBlockSelect` can jump to a slide.
- The carousel boots through the section lifecycle (`init` / `onUnload` /
  `onDeselect` / `onBlockSelect` / `onBlockDeselect`) instead of the 9.0.0
  `DOMContentLoaded` + `shopify:section:load` IIFE.

## Phase 1.1 — Hero audit + fixes ✅ (2026-05-30)
Ran a 3-dimension audit with the installed skills (`typography-audit`,
`web-typography`, `frontend-design`, `ui-ux-designer`, `shopify-liquid`) — see
[DESIGN-AUDIT.md](DESIGN-AUDIT.md). Applied all Critical/High/Medium fixes (16):
accessibility (pausable autoplay, `inert` inactive slides, focus rings, 24px dot
targets, carousel ARIA, paragraph contrast), performance (responsive `<picture>`
`<img>` with lazy-load + `max_blocks`), correctness (`| escape` merchant text,
`overlay_gradient` fallback), and typography (default-copy `·`/`–`, paragraph
leading 1.7, `.t-stat` weight, body letter-spacing). Files: `sections/hero-slider.liquid`,
`assets/theme.js`, `assets/kw-typography.css`. theme-check: 0 errors.

**To finish Phase 1**
- [ ] Assign slide images per block in the Customizer (per-block `image_picker`;
      images don't travel with the file).
- [ ] Add the section to the homepage (Customizer → Add section → Hero Slider,
      or pre-place in `templates/index.json`).
- [ ] Browser-verify (see the plan's Verification section).

## Phase 2 — Marquee Strip ✅ (2026-05-30)
Ported `sections/marquee-strip.liquid` (CSS-only scrolling tagline strip; no JS; rides the Phase 1 foundation
— `.t-label` + tokens). Same flow as the hero: faithful port → 3-agent audit → approved fixes. Applied:
**escape** merchant text + separator; **reduced-motion** now wraps the content (was clipped by
`overflow:hidden`); **letter-spacing** override `.12em` (was inheriting `.t-label` `.28em`); **sr-only `<ul>`**
exposes the trust claims to assistive tech (animated copies stay `aria-hidden`); **`max_blocks: 12`**; default
separator `|` → `·`; preset `&` → `and`. No shared-layer/`theme.js`/`theme.liquid` changes. theme-check: 0 errors.
See [DESIGN-AUDIT.md](DESIGN-AUDIT.md). (Brand-palette contrast of buff-on-brown was flagged but left to the
merchant — same colours as the hero.)

## Phase 3 — Collections Grid Style 1 ✅ (2026-05-30)
Ported `sections/collections-grid.liquid` (static collection-card grid; no JS; rides the Phase 1 foundation —
`.t-eyebrow`/`.t-display`/`.t-cc-card-eye`/`.t-cc-card-name` + tokens; already had responsive `<img>` srcset).
Same flow: faithful port → 3-agent audit → all four fix tiers applied:
**escape** all merchant text + external `src`; **`max_blocks: 12`**; **eager-load + `fetchpriority="high"`** on the
first row (LCP); removed no-op `| plus: 0`; **focus ring** on the card link; removed the `aria-label` that hid the
metal label from AT (+ card-text hover underline); gated `aria-labelledby` on heading presence; editor
**empty-state**; **curly apostrophe** in "Ayurveda’s"; aligned schema card defaults to the preset; 1-line name clamp;
and **darkened the metal/eyebrow defaults** (`#d8ae82`→`#8f5e2c`, `#bc843f`→`#a05f1f`) to pass AA on cream (the only
brand-colour change requested). No shared-layer/`theme.js`/`theme.liquid` changes. theme-check: 0 errors.
See [DESIGN-AUDIT.md](DESIGN-AUDIT.md).

## Phase 4 — Global Presence ✅ (2026-05-30)
Ported `sections/global-presence.liquid` (two-column: left eyebrow+heading+body+2×2 stats; right 3-image
triptych with captions). **New shared utility:** added the **fade-up reveal IIFE** to `assets/theme.js` (verbatim
port of 9.0.0 `initFadeUp` — self-booting IntersectionObserver, adds `js-kw-fadeup` to `<html>`, reveals `.fu`
els; reused by ~7 later sections). All `.t-*` deps + tokens (`--dur-reveal`/`--ease-reveal`) already present;
triptych uses native `image_tag` (srcset/sizes/lazy) + focal point.
Same flow → 3-agent audit → all four fix tiers applied: **escape** all merchant text + focal point; **stat-label
overflow** (dropped `nowrap` → wraps); **first-panel eager** + `fetchpriority` (LCP); **empty-state** guards
(placeholder when no panels / editor hint when no stats); section **accessible name** (`<h2 id>` +
`aria-labelledby`); **eyebrow contrast** `#bc843f → #a05f1f` (AA on cream); `max_blocks:12` + stat `limit:8`;
decorative `alt`; image base `1200→1600`; removed redundant `@media(max-width:1024px)` rule; aligned hide
breakpoints to `769/768`. (Thin-space-around-`·` polish was attempted then reverted to normal spacing — fragile,
negligible gain.) theme-check: 0 errors. See [DESIGN-AUDIT.md](DESIGN-AUDIT.md).

## Phase 5 — Trust Numbers ✅ (2026-05-30)
Ported `sections/trust-numbers.liquid` (full-bleed brown band of 4 animated stat counters). **New shared JS:**
added the **count-up IIFE** to `assets/theme.js` (port of 9.0.0 `initTrustNumbers` — IntersectionObserver
count-up + fade-up reveal, reduced-motion + no-IO fallbacks, `[data-kw-trust-numbers]`). `.t-*` deps + `--ls-label`
already present. Horizontal spacing baked to the new stock convention (padding_x 80→40, mobile 20→17).
2-agent audit → all fix tiers applied: **escape** all text + attrs; **no-JS-safe reveal** (gate `.fu` on
`.js-kw-fadeup`, set by the IIFE — fixes the hidden-content trap); **count-up a11y** (`role="region"`,
`aria-hidden` the animating numeral + a `visually-hidden` real value); **thousands separator** (`toLocaleString`,
e.g. 10,000+); **tabular-nums** (no jitter); count threshold `0.5→0.2` (no "0+" stick); `max_blocks:4`; removed
dead `container_width`; **label contrast** `#fee8d97a→#fee8d9d9` + dropped the source caption's extra `0.6`
opacity; numeral `overflow-wrap:normal`; editor empty-state. theme-check: 0 errors. See [DESIGN-AUDIT.md](DESIGN-AUDIT.md).

## Phase 6 — Heritage Timeline ✅ (2026-05-30)
Ported `sections/heritage-timeline.liquid` (full-bleed brown band; 4-col desktop / 1-col mobile grid of era
blocks — year + title + description, with a dot + reveal-on-scroll). **Foundation-only — no new JS**: reuses
the shared fade-up engine (`data-kw-fade-up`); `.t-*` deps + `--shadow-ring` already present. Horizontal spacing
baked to stock (padding_x 80→40, mobile 20→17); `max_blocks:6` already in source. 1-agent audit → all fixes:
**no-JS-safe reveal** (gate `.fu` on `.js-kw-fadeup`); **escape** era/title/description + `aria_label`; stagger
delays for eras 5–6; editor empty-state; era `<div>`→`<p>` (semantics). Contrast is fine (light text on brown —
title 8.4:1, desc 7.7:1). theme-check: 0 errors. See [DESIGN-AUDIT.md](DESIGN-AUDIT.md).

## Convention — no opacity (%) controls (2026-05-30)
Merchant-facing `*_opacity` range sliders are **not used** — they sat at 100% and just cluttered the customizer.
Removed from all built sections (hero, marquee, global-presence, trust-numbers, heritage-timeline; collections-grid
had none): the schema range, the `assign`, and the `opacity: {{ … | divided_by: 100.0 }}` CSS line (default 100% =
opacity 1, so removal is visually identical). **Future sections omit opacity controls entirely** — set fixed
opacity in CSS only if a design genuinely needs it. (Structural/animation opacity like `.fu`/`.slide` is unrelated
and stays.)

## Spacing convention — matched to stock Impulse (2026-05-30)
The custom content sections originally used airier 96/80 (desktop) + 48/20 (mobile) padding. Per merchant
request, **standardized to stock Impulse spacing** so custom sections read as native and edge-align with the
header/footer (both use `.page-width` = 40px / 1500):
- **Vertical:** padding-top/bottom **75** desktop / **40** mobile (stock `.index-section` = `margin:75px 0` / `40px`).
- **Horizontal:** **40** desktop / **17** mobile (stock `.page-width` = `0 40px` / `0 17px`).
- **Max content width:** **1500** (stock; hero was 1440 → 1500).

Applied to `collections-grid` + `global-presence` (schema defaults; vertical sliders use `step:5` so `75` is
exact) and `hero-slider` (max_width 1500; inline `.slide-content` padding `80→40` desktop / `32→17` mobile).
**Future content sections start at these values.** Already-placed instances keep their saved padding — set the
sliders to 75/40/17 (the hero's CSS change applies on push). marquee-strip unchanged (thin full-bleed strip).

## Pending sections (from 9.0.0 homepage roster)
Port order mirrors the live homepage. Each needs: its `.liquid` (inline
structural CSS), any new `.t-*` classes appended to `kw-typography.css`, and a
JS class in `theme.js` only if it has behaviour.

- [x] collections-grid — ported + audited + fixed (Phase 3)
- [x] global-presence — ported + audited + fixed (Phase 4; added shared fade-up JS)
- [x] trust-numbers — ported + audited + fixed (Phase 5; added count-up JS)
- [x] heritage-timeline — ported + audited + fixed (Phase 6; reuses shared fade-up JS)
- [x] marquee-strip — ported + audited + fixed (Phase 2)
- [ ] bestsellers
- [ ] brand-story
- [ ] b2b-trust
- [ ] three-sacred-metals
- [ ] customer-reviews
- [ ] ayurveda
- [ ] meaning-legacy
- [ ] buy-back-promise
- [ ] craft-process
- [ ] kw-newsletter

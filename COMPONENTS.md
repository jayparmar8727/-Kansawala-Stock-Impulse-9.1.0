# Kansawala — Components & sections (9.1.0)

> Per-section spec + class map for the custom Kansawala sections ported into
> Impulse 9.1.0. For brand tokens/typography see [BRAND.md](BRAND.md); for port
> status see [MIGRATION-LOG.md](MIGRATION-LOG.md) and
> [section-manifest.json](section-manifest.json).

Classes with a local prefix (`s-h`, `est-yr`, …) set colour/size/spacing only;
the `.t-*` class sets the font. When fixing a typo, change **only the text**.

---

## Ported sections

### hero-slider — `sections/hero-slider.liquid`
Full-bleed hero carousel (Kansa / Brass / Copper / Sets slides). Fade between
slides, autoplay, optional dots + arrows, optional "Est." badge.

**Wiring**
- Root: `<section class="hero-slider hero-{{ section.id }}" data-section-type="hero-slider" data-section-id data-autoplay data-speed>`
- Engine: `theme.HeroSlider` in `assets/theme.js`, registered as `hero-slider`.
- Structural CSS: inline in the section, scoped to `.hero-{{ section.id }}`
  (`.slide`, `.slide-bg`, `.slide-overlay`, `.slide-content`, `.s-btns`,
  `.slider-dots`, `.s-arr-l/-r`, `.hero-est`). Dynamic colour/size rules are
  also inline (per-setting). Fonts come from `.t-*` + tokens.

**Class map**
- eyebrow — `s-eye t-eyebrow t-eyebrow--dash`
- tag — `s-tag t-label`
- heading — `s-h t-display` (first slide `<h1>`, rest `<h2>`; italic line is `<em>`)
- paragraph — `s-p t-body`
- primary button — `btn-k t-link`; outline button — `btn-o t-link`
- badge year — `est-yr t-stat-serif-upright`; badge label — `est-lb t-label`
- dots — `.dot` / `.dot.active`; arrows — `.s-arr .s-arr-l|.s-arr-r`

**Behaviour (theme.HeroSlider)**
- Fade via `.slide.active`; subtle `scale(1.04)→1` bg zoom on the active slide.
- Autoplay from `data-autoplay` + `data-speed` (seconds); paused under
  `prefers-reduced-motion`.
- Dots are an ARIA tablist with roving tabindex (←→↑↓ / Home / End).
- Prev/next arrows; 40px touch-swipe.
- Theme editor: `onBlockSelect` jumps to the selected slide (matched by
  `data-block-id`) and pauses; `onBlockDeselect` / `onDeselect` resume;
  `onUnload` clears the timer and listeners.

### marquee-strip — `sections/marquee-strip.liquid`
Brown strip of infinitely scrolling taglines (CSS `@keyframes` marquee — **no JS**). Pauses on hover,
honours `prefers-reduced-motion` (wraps to a static centred list).

**Wiring**
- Root `.kw-strip-{{ section.id }}`; animated row `.strip-track` (carries `aria-hidden="true"`).
- Items `.strip-item t-label` (uppercase Jost, `.12em` tracking override); separators `.strip-sep`.
- Seamless loop = blocks rendered twice; the duplicate copy + first trailing separator are
  `.strip-dup`/`.strip-trail` (hidden under reduced-motion). `@keyframes kw-strip-<uid>` is per-section.
- A `visually-hidden` `<ul>` of the taglines (not animated, not `aria-hidden`) gives screen readers the
  trust claims.

**Class map** — item: `strip-item t-label`; separator: `strip-sep` (no `.t-*`, coloured locally).

### collections-grid — `sections/collections-grid.liquid`
"Collections Grid Style 1" — header (eyebrow + display heading) over a responsive grid of collection cards
(image + metal eyebrow + name). Static, **no JS**; CSS hover-zoom (covered by the global reduced-motion floor).

**Wiring**
- Root `.colls-{{ section.id }}` → `.colls-inner` → `.coll-hdr` + `.coll-grid` (2-col mobile, `cols_desktop` ≥1024).
- Card = `<a class="cc">` wrapping `.cc-img-wrap > img` (responsive srcset/sizes; first row `eager`+`fetchpriority`)
  and `.cc-card` (metal eyebrow + name). Each card resolves image/link from a `collection` block setting with
  `image`/`image_url`/`link` overrides (nil-safe fallbacks).
- Keyboard focus ring on `.cc:focus-visible`; card name underlines on hover.

**Class map** — eyebrow: `t-eyebrow t-eyebrow--dash`; heading: `t-display` (+ `em`); card metal:
`cc-card-eye t-eyebrow t-eyebrow--dash t-cc-card-eye`; card name: `cc-card-name t-cc-card-name` (1-line clamp).

### global-presence — `sections/global-presence.liquid`
"Our Global Presence / From Sihor to the world's tables." Two-column: left = eyebrow + display heading +
body + a 2×2 **stats** grid; right = a 3-image **triptych** with captions. Two block types: `stat`, `panel`
(`panel` limited to 3). Static section; entrance fade-up via the shared JS.

**Wiring**
- Root `.gp2-{{ section.id }}` carries `data-kw-fade-up` → the shared **fade-up IIFE** in `assets/theme.js`
  (adds `.js-kw-fadeup` to `<html>`, reveals `.fu` elements on scroll; reduced-motion snaps them in).
- Left `.pres-l`: eyebrow, `h2.pres-h` (has `id`, referenced by the section's `aria-labelledby`), body,
  `.pres-stats` (each `.pstat` = `.pstat-n` numeral + `.pstat-l` label).
- Right `.pres-r > .tri-grid > .tri-panel`: responsive `image_tag` (`tri-img`, focal point via `--tri-focal`),
  bottom-fade `::after`, divider `::before`, caption `.tri-cap` (eye/title/sub). First panel eager-loads.
- Empty-state: placeholder panel when no `panel` blocks; editor hint when no `stat` blocks.

**Class map** — eyebrow `t-eyebrow t-eyebrow--dash`; heading `pres-h t-display` (+ `em`); body `pres-p t-body`;
stat numeral `pstat-n t-stat-serif-upright`; stat label `pstat-l t-label t-label-fit-m`; panel eyebrow
`tri-cap-eye t-eyebrow t-eyebrow--dash`; panel title `tri-cap-ttl t-card-title-serif-italic`; panel sub
`tri-cap-sub t-eyebrow t-eyebrow-fit`.

### trust-numbers — `sections/trust-numbers.liquid`
Full-bleed brown band of 4 **animated stat counters** (2-col mobile / 4-col desktop) with divider borders.
Numbers count up on scroll and the items fade up.

**Wiring**
- Root `.trust-{{ section.id }}` carries `data-kw-trust-numbers` + `role="region"` → the **count-up IIFE** in
  `assets/theme.js`. The IIFE sets `.js-kw-fadeup` on `<html>` (so `.fu` only hides with JS alive — no-JS safe),
  counts `.trust-num[data-target]` up (IO 0.2, `toLocaleString` grouping, `tabular-nums`), and reveals `.fu`.
- Each `.ti`: numeral `.tn` + label `.tl` + optional source `.ts`. When animating, the visible numeral is
  `aria-hidden` and a `visually-hidden` span carries the real final value for screen readers.
- `max_blocks: 4` (the grid + `:nth-child` divider math is hardwired to 4 / 2-col).

**Class map** — number `tn t-stat-serif-upright` (+ `trust-num` when animated); label `tl t-label t-label-stat`;
source `ts t-eyebrow`.

### heritage-timeline — `sections/heritage-timeline.liquid`
Full-bleed brown band — a 4-col (desktop) / 1-col (mobile) grid of **era** blocks (year + title + description),
each topped by a dot, revealing on scroll. Static; reuses the shared fade-up. `max_blocks: 6`.

**Wiring**
- Root `.htl-{{ section.id }}` carries `data-kw-fade-up` → shared fade-up IIFE (adds `.js-kw-fadeup` to `<html>`;
  the `.fu` reveal is gated on that flag → no-JS safe). Stagger delays for items 2–6.
- Each `.ht-item.fu`: dot (`::before`, `--shadow-ring`), `.ht-era` year, `.ht-title`, `.ht-desc`.
- Editor empty-state when no era blocks.

**Class map** — year `ht-era t-stat-serif-upright`; title `ht-title t-label`; description `ht-desc t-body-sm`.

---

## Pending sections
See [MIGRATION-LOG.md](MIGRATION-LOG.md) for the full port checklist
(collections-grid, marquee-strip, trust-numbers, global-presence,
heritage-timeline, bestsellers, brand-story, b2b-trust, three-sacred-metals,
customer-reviews, ayurveda, meaning-legacy, buy-back-promise, craft-process,
kw-newsletter). Each entry will document its class map here as it lands.

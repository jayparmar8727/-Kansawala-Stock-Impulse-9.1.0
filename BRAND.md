# Kansawala — Brand tokens & typography (9.1.0)

> Source of truth for the custom design system layered on Impulse 9.1.0.
> Tokens live in [assets/kw-tokens.css](assets/kw-tokens.css); the `.t-*`
> typography utilities in [assets/kw-typography.css](assets/kw-typography.css);
> fonts in [snippets/kw-fonts.liquid](snippets/kw-fonts.liquid). All additive —
> the stock Impulse site keeps its Fahkwang/`--type*`/`--color*` system.

## 1. Palette
| Token | Hex | Role |
|---|---|---|
| `--W`  | `#FFFFFF` | white |
| `--C`  | `#FEE8D9` | cream (light text on dark) |
| `--Bu` | `#D8AE82` | buff / accent (eyebrows, dots, arrows) |
| `--Br` | `#BC843F` | brass (primary button hover) |
| `--K`  | `#6B3C23` | kansa brown (primary button bg) |
| `--Dk` | `#2A1508` | dark brown (overlays) |
| `--off`| `#FAF5EE` | off-white surface |
| `--err`| `#B0413E` | error |

## 2. Fonts
Loaded from Google Fonts via `snippets/kw-fonts.liquid` (Impulse's font_picker
stays on Fahkwang for the stock theme).

- **`--d` → Jost** — display sans. Headings, eyebrows, labels, buttons.
  Weights loaded: 200/300/400/500/600/700.
- **`--s` → Cormorant Garamond** — serif. Italic accent lines inside display
  headings, stat/year numerals. Weights: 300/700 + italics.

## 3. Fluid type scale (Major Third, 1.25×)
`--fs-xs` · `--fs-sm` · `--fs-base` · `--fs-md` · `--fs-lg` · `--fs-xl` ·
`--fs-2xl` · `--fs-3xl` · `--fs-hero`. Each is a `clamp()` so type scales
mobile→desktop without breakpoint CSS. Change the token to resize everywhere.

## 4. Tracking, line-height, weights
- Tracking: `--ls-label:.28em` `--ls-eye:.38em` `--ls-hd:-.01em` `--ls-body:.015em`
- Line-height: `--lh-tight:1.1` `--lh-snug:1.25` `--lh-base:1.7` `--lh-loose:1.9`
- Weights: `--fw-thin:100` `--fw-light:300` `--fw-regular:400` `--fw-medium:500` `--fw-bold:700`
- Icon glyph sizes: `--icon-sm:14` `--icon-md:18` `--icon-lg:24`

## 5. Motion
Semantic durations, not a linear scale. `--dur-snap` (220ms) powers the
hero-slider nav transitions. All motion tokens collapse to `0.01ms` under
`prefers-reduced-motion: reduce`, and the section JS skips autoplay intervals
under the same query.

## 6. `.t-*` typography utilities
Set font only (family/size/weight/spacing/transform); colour stays section-local.

| Class | Use |
|---|---|
| `.t-eyebrow` (+ `.t-eyebrow--dash`) | tiny uppercase kicker, optional leading dash |
| `.t-display` (+ `em`) | main section title; `em` renders as Cormorant italic block |
| `.t-display-lg` / `.t-display-sm` | larger / smaller display |
| `.t-heading` / `.t-heading-sm` | secondary / tertiary headings |
| `.t-label` | metal/category pills, button labels |
| `.t-body` / `.t-body-lg` / `.t-body-sm` / `.t-body-serif` | body copy |
| `.t-link` | CTA links (hover widens the gap) |
| `.t-stat` / `.t-stat-serif-upright` | big numerals / year/step numbers |
| `.t-card-*`, `.t-cc-card-*`, `.t-quote` | card + quote variants (for later sections) |

## 7. Header spacing convention
Eyebrow → heading → optional italic `em` → optional body. Keep the eyebrow at
`margin-bottom: 14px`; never add `margin-top` to `.t-display em` (it already
has `display:block`). Heading→body gap varies 12–32px per section but stays
consistent within a file.

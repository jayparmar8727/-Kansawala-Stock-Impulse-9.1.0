# CLAUDE.md — working rules for this theme

This is a **fresh Impulse 9.1.0 Stock** theme being migrated to carry the custom
**Kansawala** homepage from the 9.0.0 build
(`F:\Website Devlopment\Impulse Backup\Kansawala-Impulse-9.0.0-29-05-26`).
Read [MIGRATION-LOG.md](MIGRATION-LOG.md) first, then [BRAND.md](BRAND.md) /
[COMPONENTS.md](COMPONENTS.md).

## Golden rules
1. **Additive only.** Never edit Impulse's stock files to change the stock look.
   The custom layer is namespaced (`--d/--s/--fs-*`, `.t-*`, `.hero-*`, …) and
   must not collide with `--type*/--element-*/--color*` or stock classes.
2. **The 9.0.0 backup is the design reference**, not something to copy verbatim.
   Re-implement natively for 9.1.0 (see below). Keep the **design pixel-identical**.
3. **Verify before deleting.** Don't remove inline rules in favour of a shared
   file unless you've browser-verified the shared file applies. (9.0.0 regressed
   the live hero twice doing this — see its DESIGN-AUDIT.md §D.9.)

## How a custom section is wired (the native 9.1.0 pattern)
- **Markup**: section root carries `data-section-id="{{ section.id }}"` +
  `data-section-type="<type>"`. Blocks that the editor must target carry
  `data-block-id="{{ block.id }}"`.
- **CSS**: dynamic + structural rules go **inline** in the section's
  `{% style %}` block, scoped to `.<prefix>-{{ section.id }}`. Shared fonts come
  from the `.t-*` classes (`assets/kw-typography.css`) + tokens
  (`assets/kw-tokens.css`). Do **not** create a shared `section-common.css`.
- **JS** (only if the section has behaviour): add a
  `theme.XxxSection = (function(){ function Xxx(container){ this.container = container; … first section → init(), else theme.initWhenVisible } Xxx.prototype = Object.assign({}, Xxx.prototype, { init, onUnload, onDeselect, onBlockSelect, onBlockDeselect }); return Xxx; })();`
  class in `assets/theme.js`, then register it in the `DOMready` block:
  `theme.sections.register('<type>', theme.Xxx)`. Mirror `theme.SlideshowSection`
  / `theme.HeroSlider`.

## Foundation (do not duplicate)
- `assets/kw-tokens.css` — `:root` tokens + reduced-motion floor.
- `assets/kw-typography.css` — `.t-*` utilities. **Append** new utilities here
  as sections need them; don't redefine per section.
- `snippets/kw-fonts.liquid` — Jost + Cormorant Garamond. Rendered in
  `layout/theme.liquid` `<head>`.

## Fonts
Jost (`--d`) + Cormorant Garamond (`--s`) are **not** in Impulse's font_picker
(which stays on Fahkwang). They load only via `kw-fonts.liquid`. Don't switch the
theme's global heading/body font to achieve the brand look — use the `.t-*`
classes/tokens, which are scoped to the custom sections.

## Don't break
- One `<h1>` per page: the hero's first slide is `<h1>`, the rest `<h2>`.
- Reduced motion: keep JS gated on `matchMedia('(prefers-reduced-motion: reduce)')`.
- After editing `assets/theme.js`, sanity-check the JS parses (no console errors)
  and that registration still runs.

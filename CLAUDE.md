# Project Rules — Kansawala Store (Impulse 9.1.0)

This repo is a customized build of the **Shopify "Impulse" theme, version 9.1.0**
("the **stock** theme"). These rules exist so that future work — migrations,
new sections, upgrades, and rollbacks — never destroys or silently diverges
from the stock baseline.

> Stock baseline is recorded in [`docs/STOCK_BASELINE.md`](docs/STOCK_BASELINE.md).
> Baseline git rev: `db0e1c1` ("Add project files"). 332 tracked files.

---

## Golden rules

1. **Know the baseline before you touch anything.**
   Before migrating code or adding a section, check `docs/STOCK_BASELINE.md`
   (or run `git ls-files`) to confirm whether a file is **stock** or **ours**.
   Never assume — verify.

2. **Stock vs. ours — keep them distinguishable.**
   - Files listed in `docs/STOCK_BASELINE.md` are **stock**. Editing them is
     allowed, but every edit must be intentional and noted in the commit message.
   - **New** custom files we add should be clearly named (see naming rule below)
     so a human can tell at a glance what is stock and what is custom.

3. **Never overwrite a stock file wholesale.** Prefer surgical edits (the `Edit`
   tool / minimal diffs). If a feature needs a heavily-changed version of a stock
   section, **copy it to a new custom-named file** instead of rewriting the
   original, so the stock version stays intact for comparison/rollback.

4. **Additive, reversible changes.** Every change should be reviewable as a clean
   diff and revertible with `git revert` / `git checkout <stock-rev> -- <file>`.

## Naming conventions (custom code)

- New custom sections: `custom-<name>.liquid` (e.g. `custom-promo-banner.liquid`).
- New custom snippets: `custom-<name>.liquid`.
- This makes `git ls-files sections | grep custom-` a reliable "what did we add" list.
- Do **not** rename or repurpose existing stock files; create new ones instead.

## Protected / high-risk files — extra care

- `config/settings_schema.json` & `config/settings_data.json`
  — global theme settings. **Append** new settings; never wipe or reorder
  existing blocks. A bad merge here breaks the whole theme editor.
- `sections/*-group.json` (`header-group`, `footer-group`, `popup-group`)
  and JSON templates (`index.json`, `product.*.json`, `collection*.json`, …)
  — these reference sections **by filename**. Renaming/removing a section
  without updating its references will break the storefront.
- `blocks/_section-flex-pdp-*.liquid` (14 files)
  — these power `main-product*.liquid` (the PDP buy block). New PDP features
  should **reuse** these blocks, not duplicate their logic.
- `layout/theme.liquid` — global wrapper; edits affect every page.
- `locales/*.json` — keep keys in sync across all 14 locale files.

## Section authoring checklist (new section)

1. Create `sections/custom-<name>.liquid` with a `{% schema %}` block.
2. Add a `presets` entry so it appears in the theme editor.
3. Add any new translatable strings to **all** `locales/*.json` files.
4. Do **not** edit JSON templates unless the section must appear there by default.
5. Verify the stock files in the baseline are untouched (`git status`).

## Migration / upgrade procedure

When migrating from another theme **or** upgrading Impulse to a newer version:

1. **Snapshot first.** Confirm the current state is committed; the stock baseline
   doc + git rev `db0e1c1` is the rollback anchor.
2. **Diff stock vs ours** to see our customizations before overlaying new code:
   ```
   git diff db0e1c1 -- <path>          # what we changed vs stock
   git ls-files | grep -i custom       # what we added
   ```
3. Apply the new/upgraded files, then **re-apply our customizations** on top —
   do not let an upgrade silently revert our changes.
4. Update `docs/STOCK_BASELINE.md` if the stock baseline itself changes (e.g.
   after an Impulse version bump), and bump the recorded version + git rev.

## Rollback cheatsheet

```
# restore a single stock file to baseline
git checkout db0e1c1 -- sections/<file>.liquid

# see everything that differs from the stock baseline
git diff --stat db0e1c1

# list files we added that don't exist in stock
git diff --name-status db0e1c1 | grep '^A'
```

## Git / branch rules

- Develop on branch `claude/eloquent-sagan-U602E`.
- Commit with clear messages stating **stock vs custom** intent.
- Push with `git push -u origin <branch>`. Do not open PRs unless asked.

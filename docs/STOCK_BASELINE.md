# Stock Baseline — Impulse 9.1.0

This is the authoritative record of the **stock (unmodified) Shopify Impulse
theme, v9.1.0** as it existed when this repo was first committed. Use it to tell
**stock vs. custom**, to plan migrations/upgrades, and to roll back.

- **Theme:** Impulse (Archetype) **9.1.0**
- **Baseline git rev:** `db0e1c1` — "Add project files"
- **Total tracked files:** 332
- **Captured:** 2026-05-29

> See [`../CLAUDE.md`](../CLAUDE.md) for the rules that govern changes to these files.

## File counts by directory

| Directory   | Files |
|-------------|-------|
| `sections/` | 58    |
| `snippets/` | 142   |
| `templates/`| 35    |
| `blocks/`   | 14    |
| `layout/`   | 3     |
| `config/`   | 2     |
| `locales/`  | 14    |
| `assets/`   | 63    |
| **Total**   | **332** |

## sections/ (58)

```
advanced-content.liquid          featured-video.liquid            map.liquid                       recently-viewed.liquid
age-verification-popup.liquid    footer-group.json                newsletter-popup.liquid          rich-text.liquid
announcement.liquid              footer-promotions.liquid         newsletter.liquid                scrolling-text.liquid
apps.liquid                      footer.liquid                    password-header.liquid           search-results.liquid
article-template.liquid          giftcard-header.liquid           popup-group.json                 shoppable-hero.liquid
background-image-text.liquid     header-group.json                predictive-search.liquid         slideshow.liquid
background-video-text.liquid     header.liquid                    product-full-width.liquid        store-availability.liquid
blog-posts.liquid                hero-video.liquid                product-recommendations.liquid   testimonials.liquid
blog-template.liquid             hotspots.liquid                  promo-grid.liquid                text-and-image.liquid
collection-header.liquid         image-compare.liquid                                              text-columns.liquid
collection-return.liquid         image-grid.liquid                                                 text-with-icons.liquid
contact-form.liquid              list-collections-template.liquid
countdown.liquid                 logo-list.liquid
faq.liquid                       main-404.liquid
featured-collection.liquid       main-cart.liquid
featured-collections.liquid      main-collection.liquid
featured-product.liquid          main-page-full-width.liquid
                                 main-page.liquid
                                 main-product-high-variant.liquid
                                 main-product.liquid
                                 main-search.liquid
```

Full list (alphabetical): advanced-content, age-verification-popup, announcement,
apps, article-template, background-image-text, background-video-text, blog-posts,
blog-template, collection-header, collection-return, contact-form, countdown, faq,
featured-collection, featured-collections, featured-product, featured-video,
footer-group (json), footer-promotions, footer, giftcard-header, header-group (json),
header, hero-video, hotspots, image-compare, image-grid, list-collections-template,
logo-list, main-404, main-cart, main-collection, main-page-full-width, main-page,
main-product-high-variant, main-product, main-search, map, newsletter-popup,
newsletter, password-header, popup-group (json), predictive-search, product-full-width,
product-recommendations, promo-grid, recently-viewed, rich-text, scrolling-text,
search-results, shoppable-hero, slideshow, store-availability, testimonials,
text-and-image, text-columns, text-with-icons.

## blocks/ (14) — PDP "flex" buy blocks (power main-product*.liquid)

```
_section-flex-pdp-buy-buttons.liquid     _section-flex-pdp-price.liquid
_section-flex-pdp-description.liquid      _section-flex-pdp-quantity-picker.liquid
_section-flex-pdp-divider.liquid          _section-flex-pdp-sku.liquid
_section-flex-pdp-installments.liquid     _section-flex-pdp-title.liquid
_section-flex-pdp-inventory.liquid        _section-flex-pdp-variant-picker.liquid
_section-flex-pdp-media-gallery.liquid    _section-flex-pdp-vendor.liquid
_section-flex-pdp-pick-up.liquid
_section-flex-pdp-policies.liquid
```

## templates/ (35)

```
404.json                           page.about.json                product.gift-card.json
article.json                       page.careers.json              product.high-variant.json
blog.json                          page.contact.json              product.json
cart.ajax.liquid                   page.faq.json                  product.modal.json
cart.json                          page.full-width.json           product.preorder.json
collection.collection-landing.json page.json                      product.product-landing.json
collection.json                    page.lookbook.json             search.json
collection.no-promos.json          password.json
collection.no-sidebar.json         product.brand-story.json
gift_card.liquid
index.json
list-collections.json

customers/  account.liquid  activate_account.liquid  addresses.liquid  login.liquid
            order.liquid  register.liquid  reset_password.liquid
```

## layout/ (3)
```
gift_card.liquid   password.liquid   theme.liquid
```

## config/ (2)
```
settings_data.json   settings_schema.json
```

## locales/ (14)
```
de.json / de.schema.json            it.json / it.schema.json
en.default.json / en.default.schema.json   pt-BR.json / pt-BR.schema.json
es.json / es.schema.json            pt-PT.json / pt-PT.schema.json
fr.json / fr.schema.json
```

## assets/ — 63 files (CSS/JS/images). Verify with `git ls-files assets`.
## snippets/ — 142 files. Verify with `git ls-files snippets`.

---

## How to use this baseline

```bash
# Is a file stock or ours?  -> check if it exists at the baseline rev:
git cat-file -e db0e1c1:sections/<file>.liquid && echo STOCK || echo CUSTOM

# What have we changed vs stock?
git diff --stat db0e1c1

# Restore a stock file:
git checkout db0e1c1 -- <path>
```

When the stock theme itself is upgraded (e.g. Impulse 9.2.0), regenerate this
file and update the version + git rev above.

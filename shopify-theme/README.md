# Odd Ritual — Shopify theme

A 1:1 Shopify Liquid conversion of `reference/index.html` + `reference/styles.css`.
The rendered DOM of the home page matches the reference structurally; the design CSS
and the site's own animation bundle ship unmodified; every image, string and link is
editable from **Online Store → Customize**.

## Install

1. Zip the contents of this folder (or use the `odd-ritual-theme.zip` in the project root).
2. Shopify admin → **Online Store → Themes → Add theme → Upload zip file**.
3. **Customize** to edit. Nothing else is required — the theme renders the full design
   on a brand-new store with no products, menus or images configured.

With the Shopify CLI instead:

```sh
shopify theme dev  --path shopify-theme    # local preview
shopify theme push --path shopify-theme    # upload
```

## What is verbatim from the reference

| File | Origin |
| --- | --- |
| `assets/styles.css` | normalize + webflow + `odd-ritual-gc.webflow.css` + `udesly-common.css`, unmodified |
| `assets/styles-2026.css` | the site's override/animation stylesheet, unmodified |
| `assets/scripts-2026.js` | the site's animation bundle — one patched line, marked `THEME PATCH`, points the cart bridge at the vendored copy instead of the original store CDN |
| `assets/vendor-*.js` | jQuery, Webflow, Barba, GSAP (+Flip/Observer/ScrollTrigger/CustomEase), Hammer, Lottie, SplitType, Lenis, Udesly — the exact libraries the reference loaded, in the exact order, vendored so the theme has no third-party runtime dependency |
| `assets/or-*.jpg` / `or-*.svg` | the 109 photographs and marks that were inlined as base64 in the reference |
| `snippets/icon-*.liquid` | the 11 inline brand SVGs |
| `snippets/cart-drawer.liquid` | the Webflow cart markup and its `x-wf-template` line-item templates |

Because the original bundle drives everything, all of it behaves as on the reference
site: preloader, menu panels, hero and gallery sliders, draggable product marquee,
hover-target images, custom cursor, CTA overlays, `[h-flip]` / `[text-btn]` / `[arrow-trigger]`
hovers, footer image-follow, easter egg on the footer logo, the 10-second signup popup,
nav colour switching, Lenis smooth scroll and Barba page transitions.

## Pages

Every page in the menu, plus the whole purchase flow, is built from the live site's own
markup — the reference export only contained the home page, so the rest were converted from
`oddritualgolf.com` directly.

| Template | Section(s) | Converted from |
| --- | --- | --- |
| `index` | home sections + `footer-light` | `reference/index.html` |
| `product` | `main-product` + `footer-dark` | live `/products/…` |
| `collection` | `main-collection` + `footer-dark` | live `/collections/all` |
| `page` | `main-page` + `footer-dark` | live `/pages/privacy-policy` |
| `page.about` | `about-hero`, `image-section`, `about-main-section`, `about-footer` | live `/pages/about` |
| `page.contact` | `contact-section` | live `/pages/contact-us` |
| `cart` | `main-cart` + home sections + `footer-light` | live `/cart` |
| `search`, `404`, `list-collections`, `blog`, `article`, `customers/*` | matching `main-*` sections | built from the same design system |

Body classes, the `nav-component dark` modifier and the light-vs-dark footer follow the
reference page for page: product carries `u-theme-light`, product/collection get the dark
nav, home and cart use the light footer, product/collection/policy use the dark one, About
has its own footer and Contact has none.

`page.about` and `page.contact` are suffix templates — assign them to those pages in the
Shopify admin (Pages → About → Template suffix → `about`).

## The purchase flow

1. **Collection** — `product-card-grid` cards, hover image, `[ View Product ]` overlay.
2. **Product** — gallery, accordions (from `custom.*` metafields, falling back to editable
   rich text), and the Webflow add-to-cart form: hidden `<select name="optionN">` mirrored by
   the `size-button` pill group and the `Size:` toggle, posting `id` + `product-id` to
   `/cart/add`.
3. **Cart drawer** — the Udesly bridge intercepts the form, calls `/cart/add`, re-renders the
   drawer from the `x-wf-template`, and opens it.
4. **Checkout** — the drawer's Checkout button goes to Shopify's hosted checkout, which is
   where payment and order placement happen.

The `/cart` page mirrors the reference, which treats it as a "keep shopping" page rather than
a cart — the drawer is the cart. The line-item table is added above it so the page still
works with JavaScript off.

## Section map

| Reference markup | Theme file |
| --- | --- |
| `<html>` / `<head>` / `.page_wrap` / `.page-main` | `layout/theme.liquid` |
| `.preloader_wrap` | `snippets/preloader.liquid` |
| `.nav-component` (menu panel + `.orgc-nav`) | `sections/header.liquid` |
| cart drawer (×2) | `snippets/cart-drawer.liquid` |
| `#home-hero.hero-slider` | `sections/home-hero.liquid` |
| `#shopify-section-intro-section` | `sections/intro-section.liquid` |
| `.product-draggable_wrap.is-home` | `sections/featured-products.liquid` |
| `#shopify-section-home-image-section` | `sections/home-image-section.liquid` |
| `#shopify-section-or-text-section` | `sections/or-text-section.liquid` |
| `#shopify-section-home-slider` | `sections/home-slider.liquid` |
| `#shopify-section-community-section` | `sections/community-section.liquid` |
| `#shopify-section-footer-light` | `sections/footer-light.liquid` |
| `#shopify-section-easter` | `sections/easter.liquid` |
| `#shopify-section-signup-popup` | `sections/signup-popup.liquid` |
| `.dark-section_wrap > .page-contain` wrapper | `templates/index.liquid` |

`templates/index.liquid` calls its sections statically — that is what produced the plain
`id="shopify-section-…"` values in the reference, and it is what keeps the four middle
sections inside the single `.dark-section_wrap` wrapper. Every one of those sections still
appears in the theme editor with all of its settings and blocks; they just cannot be
reordered, because the wrapper defines the order. Each section also declares `tag` and
`class` in its schema and runs `snippets/section-class.liquid`, reproducing the reference's
own trick of rewriting Shopify's section wrapper so the published DOM matches.

## What you can edit

**Theme settings** — favicon; the 8 preloader images; all 8 brand marks (header wordmark,
preloader wordmark, menu crest, intro crest, made-locally icon, community artwork, footer
logo, easter icon — each falls back to the built-in SVG); newsletter embed/labels; smooth
scroll, custom cursor and preloader toggles; every cart-drawer label including the empty-cart
copy and error message.

**Header** — menu (Shopify link list, or six manual label/URL pairs that default to the
reference links), a hover image per menu link, menu background image, tagline, location,
toggle and close labels, shop link, newsletter trigger label, social link.

**Home hero** — a block per slide: image, optional video, thumbnail, title, link label, link
URL, new-tab toggle; plus the desktop and mobile hints.

**Featured products** — pick a collection, or hand-pick products as blocks, or leave both
empty and the reference's 38 cards render. Eyebrow, product count and the "view all" link
are settings.

**Image CTAs** — a block per tile: image, eyebrow, hover title, hover link label, URL,
new-tab, column width and full-height toggle.

**Gallery slider** — a block per slide plus the label and mobile hint.

**Community** — giving-back image, title and copy, list heading, and a block per partner
(name, description, website, link label, image).

**Footer** — tagline, signup heading, copyright, credit; four hover images; a block per link
column with either a Shopify menu or four manual label/URL pairs, all defaulting to the
reference links.

**Easter egg / signup popup** — image, copy, auto-open toggle and delay.

Images left empty fall back to the reference photography bundled in `assets/`, so the theme
never renders a hole.

## Deliberate differences

* **Newsletter** — the reference had an empty `<div class="klaviyo-form-SCSKtr">` that the
  Klaviyo app filled in at runtime. The theme renders a working Shopify customer form inside
  that same wrapper (the design's `footer form input` / `.signup-popup form input` rules style
  it). Paste your own embed into *Theme settings → Newsletter → Embed code* to restore Klaviyo.
* **`show-preloader`** — added to `<body>` on load; `scripts-2026.js` strips it for repeat
  visits in the same session. The saved reference DOM lacks it only because it was captured
  after that script ran.
* **Inner templates** — the reference contained the home page only. `product`, `collection`,
  `cart`, `search`, `page`, `blog`, `article`, `404`, customer and gift-card templates are
  built from the same utility classes and are extrapolated, not copied. `assets/theme.css`
  styles only those, plus `.brand-mark`; it does not touch any selector the two reference
  stylesheets define. `assets/theme.js` adds only the product-page variant picker.

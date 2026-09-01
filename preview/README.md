# Preview

Static renders of every template in `../shopify-theme`, produced by running the Liquid
through a local engine with mock Shopify data (18 real products, a 2-item cart), then
pointing asset URLs at `../shopify-theme/assets` and rewriting internal links so the pages
link to each other.

Open **`index.html`** in a browser. A bar along the bottom links every page.

The real `styles.css`, `styles-2026.css` and `scripts-2026.js` are loaded, so the preloader,
menu, sliders, draggable marquee, hover states, cursor and popup all behave as they will on
the store. The first page load runs the preloader for a few seconds — that is the real
behaviour; `sessionStorage` skips it for the rest of the session.

What the preview cannot show, because it has no Shopify backend: adding to cart, the cart
drawer filling, and checkout. Those need the store.

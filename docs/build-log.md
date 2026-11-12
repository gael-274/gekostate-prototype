# GEKOSTATE build log

## 2026-11-12 — Phase 0: Blueprint v1
- Living doc: GEKOSTATE Platform Blueprint v1 (claude.ai): https://claude.ai/artifact/SG4ewvpWA5cL4QjkXLENWf
- Covers: exec summary and phasing, information architecture, sitemap, user flows, customer journeys, PostgreSQL schema (9 domain schemas), system architecture, feature matrix, loyalty ranks, checkout system (sequence diagram), benchmark vs Amazon.co.za and Takealot, risks and next steps.

## 2026-11-12 — Step 1–2: "New age" 3D prototype
- Homepage: sticky header and mega-menu, live 3D hero figure, drop countdown, Shop-by-Anime rail (20 series), departments, product grid, loyalty rank band, footer, mobile bottom tab bar.
- 3D product pages (`#product-kitsune`, `#product-neon`, `#product-mystery`): Three.js viewer, camera angles, lighting presets, finish swatches recolour the model live.
- 3D models are procedural placeholders (original "Kitsune Ronin" figure, neon sign, mystery box).
- Style: dark-first, Unbounded + Manrope + Chakra Petch, kanji glyph art as image placeholders.

## 2026-11-12 — Step 3–4: 13 department pages
- Routes `#c-apparel` … `#c-japan`, ~130 sample products, faceted filters with live counts.
- A tool per department: fit and size finder, scale ruler, wall preview, shop-the-room, gift finder, desk-mat sizes, manga series tracker, card limits and singles table, cosplay size calculator and look builder, school bundle, car placement picker, mystery tiers, heat and diet filters.
- Product page adapts by category: photo gallery for non-3D items, category options, pre-order deposit, purchase limits, specs, size guide.

## 2026-11-12 — Step 5–6: Checkout and order tracking
- Routes `#checkout`, `#order`. Cart "Checkout" and product "Buy now" go to checkout.
- 4-step single-page checkout:
  1. Contact: guest or sample member, SA mobile validation, POPIA opt-in.
  2. Delivery: door (saved or new address, SA provinces, split delivery) or PUDO locker; standard, next day, same day (Gauteng only); pre-order split note.
  3. Savings and gifts: coupons, gift card, store credit, GekoPoints (max 30%), gift wrap with message, notes.
  4. Payment: Card (Yoco), Ozow, Capitec Pay, SnapScan, Zapper, Payflex (from R 300), Mobicred, Manual EFT (48-hour hold).
- Sandbox gateway with approve/decline; decline keeps the cart. Order page with 8-step tracking timeline, EFT variant, guest "save to account".
- Tested end to end on desktop and a 390 px phone.

## 2026-11-12 — Step 7: Shop-by-Anime franchise pages
- Routes `#anime-<slug>` for all 20 series (e.g. `#anime-one-piece`, `#anime-demon-slayer`, `#anime-jojos-bizarre-adventure`). Unknown slugs fall back to the homepage series rail.
- Hero: series name, one-line tagline, product count, original background built from the series' colour pair in `animeArt()` plus a repeating kanji pattern. No character art, logos or screenshots.
- Department chips (All, Apparel, Collectibles, Wall Art, Accessories, Home, Manga) with counts. A chip with nothing in stock shows an empty state with "Notify me".
- Rows, reusing `cardHTML()` with department card skins: Featured (3D first, then limited, pre-order, price), New arrivals (new and pre-order pieces, kept distinct from Featured), Collectibles, Apparel, Accessories.
- Series with fewer than 8 products show everything in one grid plus a "Notify me when new stock lands" panel (toast only), instead of rows that would mostly repeat each other.
- "Start here" strip: the manga Vol. 1 and the cheapest item under R 300, each shown only when it exists (Demon Slayer has a box set but no Vol. 1, so it shows the entry piece only).
- "Fans also shop": 4 related series, same loose genre group first (battle, dark, sports and life), then by catalogue size.
- Wiring: the homepage rail links to each page. The "Shop by Anime" mega-menu item is now a button that opens a 20-series panel (Escape or click outside closes it). Product pages link the series eyebrow to its page and show "More from <series>".
- Data: products carry a `franchise` field, derived from `fr` (plus `One Piece TCG` → One Piece). Added 3–5 sample products each for the 10 series that had fewer than 4 (Bleach, Tokyo Ghoul, My Hero Academia, Black Clover, One Punch Man, Kaiju No. 8, Blue Lock, Haikyuu!!, Hunter x Hunter, JoJo's Bizarre Adventure). Every series now has 4 to 14 products.
- Card tags now wrap before the wishlist heart, so Pre-order + Limited no longer overlap it on narrow cards.
- Tested with Playwright in Chrome at 390 px and 1366 px: all 20 series pages, every chip, notify, the menu panel, rail taps, Start-here add-to-cart and "More from" links. No console errors, no sideways page scroll, and the existing department and product pages are unchanged.

## Benchmark verdict
- Ahead of Amazon/Takealot for collectors on 3D, category tools, pre-orders, drop limits and gamified loyalty; covers roughly a third of their functional surface.
- P0 before launch: search, real auth and accounts, live payments with webhooks, courier tracking feeds, verified reviews, self-service returns, help centre and WhatsApp support.
- P1: GEKO Pass paid membership, eBucks and Discovery Miles, personalised recommendations, saved cards. P2: PWA then native app, marketplace sellers.

## Decisions
- MVP launches in South Africa first (ZAR, PayFast/Ozow/Yoco, Courier Guy + PUDO). Africa-wide, multi-currency and the mobile app are Phase 5.
- Marketplace-ready from day one: `vendor_id`, `currency`, `region` on sellable rows; GEKOSTATE is vendor #1 at 0% commission.
- Backend: NestJS modular monolith; payments and couriers behind provider adapters.
- An order is paid only on a verified, idempotent gateway webhook; the API reprices server-side; stock is reserved in Redis during the payment window and released after 30 minutes without a webhook.
- 3D in production: React Three Fiber or `<model-viewer>`, GLB + USDZ per product; `product_media.type = 'model_3d'`.
- Category templates are config-driven (`category_templates` + attribute schema per category).

## Open policy items (need sign-off)
- Pre-order deposit 20% · mystery box minimum value 1.4× · trading card purchase limits · snacks ship with at least 3 months best-before · gift wrap R 35
- Delivery rates R 75 / R 120 / R 180, locker R 60 / R 99, free from R 750 · points capped at 30% of subtotal · EFT hold 48 hours

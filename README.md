# GEKOSTATE™

**Collect. Wear. Live Anime.** Africa's anime marketplace, launching in South Africa first.

This repo holds the clickable prototype and the planning docs. The production build (Next.js 15 + NestJS + PostgreSQL) will be added here as `apps/` and `packages/` once the prototype is signed off.

## Preview

Every push to `main` deploys the prototype to GitHub Pages (see `.github/workflows/pages.yml`).
The site lives at https://gael-274.github.io/gekostate-prototype/.

Each build step has its own link. Add the part after `#` to the site address:

| Step | What to check | Path |
| --- | --- | --- |
| 1. Homepage | 3D hero figure, drop countdown, Shop by Anime, departments, loyalty ranks | `#home` |
| 2. 3D product pages | Spin, zoom, lighting, finishes that recolour the model | `#product-kitsune` · `#product-neon` · `#product-mystery` |
| 3. Department pages | 13 departments, filters, a tool per department | `#c-apparel` · `#c-collectibles` · `#c-wall` · `#c-home` · `#c-accessories` · `#c-gaming` · `#c-manga` · `#c-cards` · `#c-cosplay` · `#c-school` · `#c-car` · `#c-mystery` · `#c-japan` |
| 4. Category-aware product pages | Sizes, frames, bed sizes, phone models, pre-orders, card limits | `#product-akatsuki-cloud-tee` · `#product-wanted-poster-set-of-3` · `#product-levi-ackerman-1-7-scale-figure` |
| 5. Checkout | Guest or member, door or PUDO locker, coupons, points, 8 payment methods, sandbox approve/decline | add items, then `#checkout` |
| 6. Order tracking | 8-step timeline, EFT variant | finish a checkout, lands on `#order` |

Test data for checkout: coupons `GEKO10`, `WELCOME50`, `FREESHIP` · gift card `GIFT-2026-0001` · "Sign in as the sample member" unlocks saved addresses, 2,140 points and R 120 store credit.

To preview locally without GitHub, open `prototype/index.html` in a browser (an internet connection is needed for fonts and the 3D library).

## What's in the prototype

- Dark-first brand UI (#FF4D4D, #0F172A, #7C3AED, #FBBF24), responsive down to 390 px with a phone tab bar.
- Real-time 3D product viewer (Three.js r128) with procedural placeholder models.
- ~130 sample products across 13 departments. All products, prices, stock, lockers and delivery rates are sample data.
- Full checkout and order flow in the browser only. Nothing is charged or stored; the payment step is a sandbox stand-in.

## Docs

- [`docs/build-log.md`](docs/build-log.md): what was built at each step, decisions, open policy items.
- Platform Blueprint (living doc on claude.ai): information architecture, sitemap, user flows, database schema, system architecture, feature matrix, loyalty, checkout sequence, Amazon/Takealot benchmark.

## Roadmap

1. Sign off the prototype look and the open policy numbers in the build log.
2. ~~Shop-by-Anime franchise pages (20 series).~~ Done: `#anime-<slug>`, e.g. `#anime-one-piece`.
3. Launch blockers from the benchmark: search, accounts, reviews, returns, live payments, courier tracking, support.
4. Production scaffold: Turborepo with `apps/web` (Next.js 15), `apps/api` (NestJS), `packages/ui`, `packages/db`.

# GEKOSTATE — project instructions for Claude Code

GEKOSTATE™ ("Collect. Wear. Live Anime.") is an anime marketplace launching in South Africa first, built to grow into a multi-vendor, Africa-wide platform (50,000+ products).

## Current state
- `prototype/index.html`: single-file clickable prototype (vanilla JS + Three.js r128 from CDN). Hash routes: `#home`, `#c-<dept>` (13 departments), `#product-<id>`, `#checkout`, `#order`. It is the visual and behavioural reference for the production build. Keep it working; GitHub Pages deploys it on every push to `main` of the repo gael-274/gekostate-prototype. Live site: https://gael-274.github.io/gekostate-prototype/
- `docs/build-log.md`: what was built, decisions, open policy items. Update it at the end of every working session.
- The full Platform Blueprint (IA, sitemap, schema, architecture, checkout sequence, Amazon/Takealot benchmark) is a living doc on claude.ai: https://claude.ai/artifact/SG4ewvpWA5cL4QjkXLENWf

## Target stack (production)
- Monorepo: Turborepo + pnpm. `apps/web` (Next.js 15 App Router, TypeScript, Tailwind, shadcn/ui, Framer Motion, React Three Fiber for 3D), `apps/api` (NestJS), `packages/ui` (shared components + design tokens), `packages/db` (Prisma schema + migrations), `packages/config` (eslint, tsconfig).
- PostgreSQL 16, Redis (cache, carts, stock reservations, BullMQ queues), Elasticsearch (search), S3 (media, GLB/USDZ 3D models), Auth.js or Clerk.
- Hosting: Vercel (web), AWS (api, db, redis), Cloudflare (DNS, WAF, CDN).

## Non-negotiable rules
- Money is integer cents + currency code (`ZAR`). Prices shown VAT-inclusive at 15%.
- Every sellable row carries `vendor_id`, `currency`, `region`, even though GEKOSTATE is the only vendor at launch.
- An order becomes paid only on a verified, idempotent payment webhook. Never trust browser prices: the API reprices every cart line. Card data never touches our servers (hosted payment pages only).
- Stock is reserved in Redis during the payment window and released after 30 minutes without a webhook.
- Payments and couriers live behind adapters (`PaymentProvider`, `ShippingProvider`). Launch providers: Yoco, Ozow, PayFast (SnapScan/Zapper), Capitec Pay via Peach, Payflex, Mobicred, manual EFT; The Courier Guy and PUDO.
- Department pages are config-driven (filters, card style, product options, specs, special module per department), as in the prototype's `dept()` definitions.
- POPIA: marketing consent unticked by default; support data export and delete.
- Never use real anime character artwork or brand logos as placeholder imagery; use the prototype's generated kanji/gradient art until licensed assets arrive.

## Brand
- Dark-first. Colours: primary #FF4D4D, secondary #0F172A, accent #7C3AED, highlight #FBBF24.
- Type: Unbounded (display), Manrope (body), Chakra Petch (labels, prices, data).
- Loyalty ranks: Academy Student → Genin → Chunin → Jonin → Kage → Anime Legend.

## Working style
- Build one module at a time, each shippable and previewable. Small commits with clear messages; push to `main` only when the prototype still loads.
- Mobile first: test at 390 px wide as well as desktop.
- Ask before adding a new paid service or changing a non-negotiable rule above.

## Next tasks (in order)
1. Shop-by-Anime franchise pages (20 series) in the prototype.
2. Scaffold the monorepo (`apps/web`, `apps/api`, `packages/*`) with the design tokens from the prototype.
3. Prisma schema from the Blueprint's 9 domain schemas (identity, catalog, inventory, commerce, payments, fulfilment, loyalty, community, marketplace).
4. Launch blockers from the benchmark: search, accounts, reviews, returns, live payments (sandbox first), courier tracking, support.

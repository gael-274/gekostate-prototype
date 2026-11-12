# Kickoff prompts for Claude Code in VS Code

Paste these into the Claude Code panel one at a time.

## 1. Push the repo to GitHub

```
Read CLAUDE.md and README.md. Then publish this repo to my GitHub:
1. Check whether the GitHub CLI (gh) is installed and logged in. If it isn't, tell me the exact commands to install it and run `gh auth login`, and wait for me.
2. Create a new repository called gekostate under my account. Ask me first whether it should be public or private, and remind me that GitHub Pages on a free account needs a public repo.
3. Set it as origin and push main.
4. Enable GitHub Pages with "GitHub Actions" as the source (gh api or tell me where to click), then watch the Pages workflow and give me the live URL.
5. Set the git author on this machine to my own name and email before pushing, and rewrite the three existing commits to that author.
```

## 2. Start building

```
Read CLAUDE.md, docs/build-log.md and prototype/index.html.
Next task: add Shop-by-Anime landing pages to the prototype for all 20 series (route #anime-<slug>), each with a hero banner, featured products, new arrivals, collectibles, apparel, accessories and related series, reusing the existing card and filter components. Keep the dark-first brand.
Test it at 390 px and desktop, update docs/build-log.md, commit with a clear message and push so GitHub Pages updates. Then give me the preview link for one series page.
```

## 3. Production scaffold (when the prototype is signed off)

```
Read CLAUDE.md. Scaffold the production monorepo next to /prototype:
Turborepo + pnpm, apps/web (Next.js 15 App Router, TypeScript, Tailwind, shadcn/ui, Framer Motion, React Three Fiber), apps/api (NestJS), packages/ui with the brand tokens from the prototype, packages/db with a Prisma schema for the identity and catalog domains first.
Port the homepage from the prototype into apps/web as the first page. Make `pnpm dev` run both apps. Explain each folder briefly in README.md, commit and push.
```

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Peter Vu's personal website, portfolio and blog, served at **pieberry.dev** (the domain is registered through Cloudflare). It is themed after Pieberry from the *Witch Spring* series.

## Stack and commands

A static **Astro 7** site. Use **bun** for installs and scripts, never npm, pnpm or yarn. Astro itself still runs on Node (it needs Node >=22.23.4).

```bash
bun install          # install deps
bun run dev          # dev server at http://localhost:4321
bun run build        # static build into dist/
bun run preview      # serve dist/ locally
bun run check        # astro check: type and template diagnostics
```

There are no tests or linter yet. Add them when there is code worth testing.

- When starting the dev server from a session, run `bunx astro dev --background`. Manage it with `astro dev stop`, `astro dev status` and `astro dev logs`.
- **TypeScript is pinned to 6 on purpose.** `astro check` refuses TypeScript 7, so don't bump it until Astro supports it.
- `site` in `astro.config.mjs` is `https://pieberry.dev`, which feeds canonical URLs and any future sitemap.
- Pages live in `src/pages/` (file-based routing), and static files in `public/`.
- Astro docs: https://docs.astro.build. Check the content-collections guide before modeling the blog or the résumé data.

## CI/CD

`.github/workflows/ci.yml` runs install, `check` and `build` on every PR and on every push to `main` (`origin` is `github.com:pieberrykinnie/website`). Bun is pinned there to 1.4.4, matching local, so bump both together.

Nothing deploys yet. When the first page is approved, add a Cloudflare deploy (Cloudflare is where the domain lives), following workflow rule 1 below. Development happens on Windows, so keep scripts cross-platform.

## Requirements every change must meet

- **Performance:** ship static HTML wherever possible, and add JS only where a page needs interaction. Animate `transform` and `opacity`, not layout properties.
- **Responsive:** every page works from phone width up to desktop.
- **Accessibility:** WCAG 2.2 AA at minimum. The theme makes two things easy to get wrong:
  - Pink palettes often fail contrast. Check each text/background pair (4.5:1 for body text, 3:1 for large text and UI parts) before proposing it.
  - Popping animations must respect `prefers-reduced-motion`, never flash more than 3 times a second, and never hide content or trap focus.
- **10-second findability:** a visitor landing on the home page must see a way into every section (blog, projects, experience and so on) without scrolling or hunting.
- **Content is data, not markup:**
  - Blog posts are Markdown files that support all common Markdown, including GFM tables and embedded images, video and iframes.
  - Résumé content lives in structured data files, so updating it never means editing components. That covers education, work experience, personal projects, volunteer experience, research, and competitions and achievements.

## Design direction

The site should be vibrant and colorful, with lots of pink and plenty of personality. **Personality beats professionalism**: this is not a generic developer portfolio. Use pretty, popping animations inspired by Japanese pop culture, in honor of Pieberry. Use only character art that Peter supplies, and don't pull official game assets from the web.

Peter will add design-related agent skills to this project over time. When a project skill covers the design task at hand, use it.

## Workflow rules (non-negotiable)

1. **Peter approves every page's design before it deploys.** Show each new or restyled page at phone and desktop widths, in a local preview or a PR preview deploy, and wait for an explicit yes. The intended CI setup: every PR gets a preview deploy, only merges to `main` deploy to production, and a PR that adds or restyles a page merges only after Peter approves it.
2. **Content comes before more pages.** Build the site one section at a time. Before starting a new page or section, the previous one must hold Peter's real content. That means no lorem ipsum, no placeholder pages and no scaffolding of empty routes. When a section is ready for content, stop and ask Peter for it with a concrete list of what's needed (for example "3 projects: name, one-liner, link, image"). Don't move on until the content is in.

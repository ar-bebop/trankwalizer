# Trankwalizer — Personal Trainer Website

## Project Overview
Static website for a personal trainer friend. Hosted on GitHub Pages at `trankwalizer.fit`. Built with Astro + Tailwind CSS v4.

## Tech Stack
- **Framework:** Astro 6 (static output)
- **CSS:** Tailwind CSS v4 via `@tailwindcss/vite` (NOT v3 — no `tailwind.config.js`, CSS-first config)
- **Package manager:** pnpm (pinned to v9 in CI — do not change to latest, v10+ has a supply chain policy that breaks CI)
- **Hosting:** GitHub Pages via `withastro/action@v6`
- **Domain:** `trankwalizer.fit` (custom domain, DNS on Porkbun pointing to GitHub Pages)

## Deployment
Pushes to `main` trigger `.github/workflows/deploy.yml` → builds via `withastro/action@v6` → deploys to GitHub Pages automatically.

## Project Structure
```
src/
├── pages/        ← URL routes (index.astro = homepage)
├── layouts/      ← Layout.astro is the shared page wrapper, imports global.css
├── components/   ← Reusable UI components
├── assets/       ← Images optimized at build time
└── styles/
    └── global.css  ← Tailwind entry point (@import "tailwindcss")
public/           ← Static files served as-is (CNAME, favicon)
```

## Planned Site Sections
All on a single page (`index.astro`) unless noted:
1. **Hero** — name, tagline, CTA button
2. **About** — trainer bio and photo
3. **Services / Pricing** — list of offerings with prices
4. **Schedule** — embedded Google Calendar iframe (public calendar, read-only)
5. **Contact** — simple form via Web3Forms (free, no backend needed)
6. **Testimonials** — optional, client quotes

## Key Decisions & Gotchas
- **pnpm v9 in CI** — `pnpm@latest` (v10+) introduced a `minimumReleaseAge` supply chain check that blocks recently published packages. CI is pinned to pnpm@9 via `package-manager: pnpm@9` in `withastro/action`. Do not change this.
- **Tailwind v4** — configuration is CSS-first. No `tailwind.config.js`. Custom theme tokens go in `src/styles/global.css` using `@theme` directive.
- **Google Calendar** — embed via `<iframe>` only. No API key needed. Calendar must be set to public.
- **Contact form** — use Web3Forms (`https://api.web3forms.com/submit`). Free tier, 250 submissions/month, no backend.
- **No base path** — custom domain means `base` is not needed in `astro.config.mjs`.

## Content Status
Placeholder content only. Real copy (trainer name, bio, services, pricing) to be filled in.

# Trankwalizer — Personal Trainer Website

## Project Overview
Single-page marketing site for **Sal Modica**, a canfitpro-certified personal trainer in Brampton, Ontario. Static site hosted on GitHub Pages at `trankwalizer.fit`. Built with Astro 6 + Tailwind CSS v4.

## Tech Stack
- **Framework:** Astro 6 (static output)
- **CSS:** Tailwind CSS v4 via `@tailwindcss/vite` (NOT v3 — no `tailwind.config.js`, CSS-first config)
- **Package manager:** pnpm (pinned to v9 in CI — do not change to latest, v10+ has a supply chain policy that breaks CI)
- **Node:** CI builds on node 24; `engines` requires `>=22.12.0`
- **Image optimization:** `sharp` (Astro `<Image>` / `astro:assets`)
- **Hosting:** GitHub Pages via `withastro/action@v6`
- **Domain:** `trankwalizer.fit` (custom domain, DNS on Porkbun pointing to GitHub Pages)

## Commands
```bash
pnpm install      # install deps
pnpm dev          # dev server at localhost:4321
pnpm build        # production build to ./dist/
pnpm preview      # preview the build locally
pnpm astro check  # type-check .astro files
```

## Deployment
Pushes to `main` trigger `.github/workflows/deploy.yml` → builds via `withastro/action@v6` (node 24, pnpm@9) → deploys to GitHub Pages automatically.

## Project Structure
```
src/
├── pages/
│   └── index.astro       ← the only route; composes all sections + Nav/Footer
├── layouts/
│   └── Layout.astro      ← <html> shell, fonts, <title>/description props, imports global.css
├── components/           ← one file per page section
│   ├── Nav.astro         ← fixed header + mobile menu (inline <script>)
│   ├── Hero.astro
│   ├── About.astro       ← bio + credential spec sheet, portrait from assets/sal/
│   ├── Services.astro    ← 4 services grid + pricing callout (no fixed prices)
│   ├── Gallery.astro     ← "In The Gym" duotone photos, assets/sal/
│   ├── Testimonials.astro← client quotes + profile pics, assets/testimonials/
│   ├── Contact.astro     ← Web3Forms form + socials (inline submit <script>)
│   └── Footer.astro
├── assets/
│   ├── sal/              ← trainer photos (sal_1..4.png)
│   └── testimonials/     ← client profile pics
└── styles/
    └── global.css        ← Tailwind entry + theme tokens + custom utility classes
public/                   ← served as-is (CNAME, favicon.ico/.png)
```

## Site Sections (order in `index.astro`)
Nav → Hero → About → Services → Gallery → Testimonials → Contact → Footer.
Each section is its own component. To add/remove/reorder, edit `src/pages/index.astro` and update the numbered labels (`No. 0X`) shown in Nav and section headers.

## Design System — "THE TRAINING LOG" (Blackout)
Editorial brutalist athletic theme, dark. All visual tokens live in `src/styles/global.css`:
- **`@theme` tokens** — palette (`--color-paper` page-black, `--color-ink` bone text/borders, `--color-blaze` the one red accent, etc.) and fonts (`--font-display` Anton, `--font-sans` Archivo, `--font-serif` Fraunces, `--font-mono` IBM Plex Mono, loaded from Google Fonts in `Layout.astro`). **Restyle the whole site by editing these tokens** — alternate red swatches are commented inline.
- **Custom utility classes** (used across components, defined in `global.css`): `.grain` (noise overlay on `<body>`), `.duotone` (blaze-tinted photo, reveals colour on hover), `.marquee`, `.rise`/`.rise-1..5` (staggered entrance), `.shadow-hard`/`-sm`/`-blaze` (hard offset shadows), `.meta` (mono tag label), `.stroke-ink` (outlined text), `.link-grow` (underline grow). `prefers-reduced-motion` disables animations.

## Key Decisions & Gotchas
- **pnpm v9 in CI** — `pnpm@latest` (v10+) introduced a `minimumReleaseAge` supply chain check that blocks recently published packages. CI is pinned via `package-manager: pnpm@9` in `withastro/action`. Do not change this.
- **Tailwind v4** — CSS-first config. No `tailwind.config.js`. Theme tokens go in `@theme` in `global.css` (see above).
- **Contact form** — Web3Forms (`https://api.web3forms.com/submit`). Free tier, 250 submissions/month, no backend. **Access key is hardcoded** in `Contact.astro` (`WEB3FORMS_KEY`); submit handler is an inline client `<script>`.
- **No base path** — custom domain means `base` is not needed in `astro.config.mjs`. `site: 'https://trankwalizer.fit'` is set for absolute URLs.
- **No Google Calendar / Schedule section** — was planned, never shipped. Don't reintroduce unless asked.

## Content Status
Live, real content (trainer name, bio, services, testimonials, socials, contact email). Services list flexible rates rather than fixed prices — pricing is quote-based via the contact form. Treat copy edits as real client-facing changes, not placeholder fills.

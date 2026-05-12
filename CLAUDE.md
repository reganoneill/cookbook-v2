# Cookbook v2

## Tech Stack

- **Framework:** Astro (not SvelteKit — do not confuse these)
- **Output:** Fully static (`output: 'static'` in astro.config.mjs) — no runtime server, no API
- **Language:** TypeScript

## Recipe Data

- Stored as individual JSON files in `src/data/recipes/` — one file per recipe
- Also a combined `src/data/recipes.json`
- All data is baked in at build time; there is no database or runtime data fetching

## Build

```bash
npm run build   # outputs to dist/
npm run dev     # local dev server
```

## Deploy & Hosting

- **Host:** GitHub Pages
- **CI/CD:** GitHub Actions (`.github/workflows/`) — triggers on push to `main`
- **Flow:** push to `main` → GitHub Actions runs `npm run build` → deploys `dist/` to GitHub Pages
- **Domain:** `cookbook.aktivation.space` — custom domain managed via **Cloudflare DNS** (Cloudflare is DNS only, not hosting)

## Admin UI

- Accessible at `cookbook.aktivation.space/admin`
- Single HTML file at `public/admin/index.html` — no framework, no server
- Auth: GitHub Personal Access Token (PAT) with `repo` or fine-grained `contents: read & write` access
  - Generate at github.com/settings/tokens
  - PAT is stored in `localStorage` after first login — no re-entry needed unless it expires
- On save: commits the JSON file directly to `main` via the GitHub API → triggers GitHub Actions rebuild → site live in ~1 minute
- Supports: create recipe, edit recipe, delete recipe

## What Does NOT Exist

- No Cloudflare Workers / Pages / hosting (Cloudflare is used for DNS only)
- No auth, no backend API, no database on the public site

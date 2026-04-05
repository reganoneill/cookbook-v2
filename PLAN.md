# Cookbook v2 — Plan

## Goals
- Dead simple build and infrastructure
- As close to free as possible (free is the goal)
- Replace flaky old app (cookbook + cookbook_server) with clean separation of concerns

## Architecture

### 1. Public Static Site
- All recipes pre-built at build time — zero API calls at runtime
- Hosted on **Cloudflare Pages** (free tier)
- Framework: **Astro** (purpose-built for content-heavy static sites, zero JS by default, islands architecture for interactive bits)
- Recipe data sourced from a JSON file in the repo at build time

### 2. Recipe Data
- Stored as a JSON file committed to the git repo (no database in the public stack)
- 24 existing recipes to migrate from `cookbook/cookbook-backup/cookbook/recipes.bson`
- Schema: title, slug, ingredientList (array of [name, amount] tuples), steps, prepTime, cookTime, difficulty, notes, public, keywords

### 3. Admin App (home network only)
- Simple CRUD app for managing recipe data
- Runs on Synology NAS (already on home network at 192.168.1.13)
- Only accessible on local network — no auth needed
- On save: commits/pushes updated JSON to git repo → triggers Cloudflare Pages rebuild
- Stack TBD: could be a lightweight Node/Express app or SvelteKit app running locally

## Deploy Flow
```
Admin app (NAS) → edit recipe → push JSON to git → Cloudflare Pages rebuild → live site updated
```

## Open Questions
- [x] SvelteKit vs Astro → going with **Astro**
- [ ] Admin app stack (leaning lightweight Node + simple HTML, or SvelteKit)
- [ ] How to trigger Cloudflare Pages rebuild from NAS (Cloudflare deploy hook via webhook)
- [ ] Public vs private recipes — keep the `public` flag from old data?

## Design & Aesthetic

**Vibe:** Kitschy, cheesy, nostalgic. Should feel like it belongs to a bygone era — think 1950s–60s recipe cards, church potluck cookbooks, diner menus, old Betty Crocker / Joy of Cooking era.

**Colors:**
- Background: cream/parchment `#F5F0E8`
- Accent/headers: deep red `#C0392B`
- Text: dark brown ink `#2C1810`

**Fonts (Google Fonts):**
- Display/hero: `Abril Fatface` or `Alfa Slab One` — chunky retro weight
- Headings: `Playfair Display` bold — classic editorial serif
- Body/ingredients: `Special Elite` — typewriter feel, like typed on a recipe card

**Layout:**
- Recipe cards styled like actual index cards (slight shadow, structured border)
- Grid of cards on home/listing page
- Decorative red rules/borders between sections (à la Caputo's menu)
- Subtle paper texture via CSS

**Details that sell the era:**
- Decorative flourishes (✦ ✿ ❧) as section dividers instead of `<hr>`
- ALL CAPS small labels: "PREP TIME", "DIFFICULTY", "KEYWORDS"
- Sharp corners — no border-radius softness
- No modern UI patterns (no pills, no gradients, no shadows that look Material Design)

## What We're NOT Building
- No backend API on the public site
- No user auth on the public site
- No database in the public stack
- No admin portal accessible from the internet

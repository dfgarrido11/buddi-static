# CLAUDE.md

Guidance for AI assistants working in this repository.

## What this is

`buddi-static` is the **public static marketing/demo site** for **Buddi** — an
AI-powered family assistant for families and expats living in Germany
("Der intelligente Familienassistent für Deutschland"). It is deployed to
**Netlify** and served at **buddiapp.de**.

This repo is intentionally simple: it is **pure, hand-written HTML with inline
CSS and inline vanilla JavaScript**. There is no build step, no framework, no
package manager, and no dependencies. The actual Buddi product (a React Native
+ Expo + Claude AI + Supabase app) lives in a separate repository
(`github.com/dfgarrido11/buddi`); this repo only contains the static web
presence and a clickable UI mock-up.

## Repository layout

```
.
├── index.html               # Landing page (German). Hero, CTAs, feature grid.
├── app.html                 # Interactive demo of the Buddi app UI (German).
├── dashboard.html           # Admin/analytics dashboard mock (Spanish).
├── netlify.toml             # Netlify deploy config (publish + redirects + headers)
├── _redirects               # Netlify SPA-style fallback redirect
├── quetz_post_image.jpg     # ~8.6 MB social/Instagram image asset
└── quetz_post_compressed.jpg# ~1.2 MB compressed version of the above
```

There is no `src/`, no `node_modules/`, no config beyond `netlify.toml` and
`_redirects`. Every page is a self-contained `.html` file.

## The three pages

- **`index.html`** — Landing page. Links to `app.html` ("Buddi App starten"),
  `dashboard.html` ("Analytics Dashboard"), and the product GitHub repo. States
  the tech stack and the 12 supported languages (DE/EN/ES/TR/AR/UK/PL/RU/RO/IT/PT/FR).
- **`app.html`** — The largest/most important file. A clickable demo of the
  signed-in app: CSS-grid desktop layout (header / sidebar / main / right
  panels), client-side section navigation via `showContent()`, a simulated
  "Buddy AI" chat (`sendMessage()`, keyword-based canned replies), and a
  document-management view with drag-and-drop upload (UI only — uploads are
  faked with `alert()`/`console.log`, nothing is persisted). Responsive
  breakpoints hide the right panels at ≤1200px and the sidebar at ≤768px.
- **`dashboard.html`** — Admin analytics mock with metric cards, a revenue
  breakdown, and live-updating system-status indicators. **All numbers are
  hard-coded demo data**, animated/randomized client-side via `setInterval`.

## Conventions to follow

These conventions are consistent across all three pages — match them.

- **Single-file pages.** Keep CSS in a `<style>` block in `<head>` and JS in a
  `<script>` block before `</body>`. Do not introduce external CSS/JS files,
  CDNs, bundlers, or npm unless explicitly asked.
- **Shared visual identity:**
  - Dark gradient background: `linear-gradient(135deg, #0a0a1a 0%, #1a1a2e 100%)`
  - Brand gradient (logos, buttons): `linear-gradient(45deg, #818cf8, #22c55e)`
    — indigo `#818cf8` and green `#22c55e` are the two brand colors.
  - Font stack: `'Segoe UI', Tahoma, Geneva, Verdana, sans-serif`
  - Glass-card style: `rgba(255,255,255,0.08)` background, `border-radius` ~15–20px,
    `1px solid rgba(129,140,248,0.3)` border, `backdrop-filter: blur(10px)`,
    and a `translateY(-5px)` hover lift.
  - Emoji are used liberally as iconography (🚀 🤖 💰 🇩🇪 🌱 etc.) — this is
    intentional branding, keep it.
- **Vanilla JS only.** Functions are global and wired with inline `onclick`
  handlers. Use `document.getElementById` / `querySelector`. No modules.
- **Language is mixed and page-specific.** `index.html` and `app.html` are in
  **German** (`<html lang="de">`); `dashboard.html` is in **Spanish**
  (`<html lang="es">`). Preserve the existing language of each page when
  editing its copy.
- **Demo, not production.** Chat replies, uploads, metrics, and activity feeds
  are all simulated. There is no backend, no auth, and no API calls. Don't wire
  in real services unless that is the explicit request.

## Deployment

Deployed via **Netlify** (`netlify.toml`):

- `publish = "."` — the repo root is served as-is; **there is no build command.**
- A catch-all redirect (`/* → /index.html`, 200) is defined in both
  `netlify.toml` and `_redirects`. Note this rewrites every unknown path to the
  landing page; `app.html` and `dashboard.html` are reached by their explicit
  filenames.
- Security headers set: `X-Frame-Options: SAMEORIGIN`, `X-XSS-Protection: 1; mode=block`.

To preview locally, just open the `.html` files in a browser or serve the
directory with any static file server (e.g. `python3 -m http.server`). No
install step.

## Working on this repo

- **Branch:** develop on `claude/claude-md-docs-9rt27z` (the designated feature
  branch); do not push to `master` without explicit permission.
- **Commits:** the history uses short, emoji-prefixed messages (e.g.
  `🚀 Buddi Static Site`, `🔧 NETLIFY: Fix static deployment config`). Match
  that style.
- **Editing pages:** because each page is one self-contained file, an edit to
  `app.html` cannot break `index.html` — but it also means shared styling is
  duplicated across files. If you change a brand color or font, update **all
  three** pages to keep them consistent.
- **Images:** `quetz_post_image.jpg` is large (~8.6 MB). Prefer the compressed
  variant for anything web-facing; avoid adding more uncompressed binaries.
- **No tests / no linters** exist in this repo. "Verifying" a change means
  opening the page in a browser and confirming layout, navigation, and the
  simulated interactions still work.

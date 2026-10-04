# AGENTS.md

## Project Overview

A static web app served by nginx. The site was previously a single ~3MB `index.html` with all
assets (fonts, images, JS) embedded as base64. Assets have been extracted to `assets/` and the
page template to `template.html`, leaving `index.html` as a small loader (~3KB).

### File structure

- `index.html` — slim loader: fetches `template.html` + `assets/resources.json`, injects the
  resource map, parses the template, replaces the document, and re-executes scripts.
- `template.html` — the full page HTML with UUID asset references replaced by `assets/<uuid>.<ext>`
  paths. Contains the DCLogic component (`text/x-dc` script) with the translation object (prices,
  text, etc.). **Edit this file for content/price changes** — changes appear on browser refresh.
- `assets/` — extracted images (`.png`, `.jpg`, `.webp`), fonts (`.woff2`), JS (`.js`), and
  `resources.json` (maps CDN URLs for React/ReactDOM to local asset paths).
- `docker-compose.base44.yml` — mounts all three into nginx.

## How to run

```bash
docker compose -f docker-compose.base44.yml up -d
```

Serves on host port 3000. No build step, no dependencies to install.

## Known non-issues

- `/_vercel/insights/script.js` returns 404 — Vercel analytics script, harmless (loaded with `defer`).
- `.image-slots.state.json` returns 404 — `<image-slot>` custom element, no persisted state in fresh env.

## Editing

- **Content/prices/text**: edit `template.html` directly. The translation object inside the
  `text/x-dc` script tag contains all text strings (including prices like `basicPrice: '₪520'`).
  Changes are picked up on browser refresh — call `reload_preview` after edits.
- **Images**: replace files in `assets/` with the same filename.
- **Loader logic**: edit `index.html`.

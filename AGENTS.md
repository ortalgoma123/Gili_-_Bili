# AGENTS.md

## Project Overview

This is a single-file static web app: the entire site is bundled into one `index.html` (~3MB).
The file contains a self-unpacking "bundler" that embeds all assets (fonts, images, JS) as
base64 data in `<script type="__bundler/manifest">` / `__bundler/template` tags and reconstructs
them as blob URLs at runtime via JavaScript on `DOMContentLoaded`.

## How to run

```bash
docker compose -f docker-compose.base44.yml up -d
```

Serves `index.html` via nginx on host port 3000. No build step, no dependencies to install.

## Known non-issues

- `/_vercel/insights/script.js` returns 404 — this is a Vercel analytics script referenced in the
  HTML head. It fails to load in this environment but does not affect page rendering (loaded with `defer`).
- `.image-slots.state.json` returns 404 — the `<image-slot>` custom element tries to load persisted
  image state; in a fresh environment there is none, so it 404s harmlessly.

## Editing

Edit `index.html` directly. Changes are picked up on browser refresh (no live-reload server;
call `reload_preview` after edits if needed).

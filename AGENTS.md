# Base44 development notes

- The repository contains only `index.html`: a self-extracting, approximately 3 MB HTML bundle, not the original component source. It includes its own assets and scripts; there is no backend or required external credential.
- Use `docker compose -f docker-compose.base44.yml up -d --build` for the live-reloading static development server on port 3000. Vite cannot parse this HTML bundle because its `<head><noscript>` contains a `<div>`, so the compose file uses a server that serves the source HTML unchanged.
- Verify with `curl -f http://localhost:3000/` and by checking that the birthday-kit page replaces the temporary "Unpacking..." view. The bundled page may request optional Vercel analytics and `.image-slots.state.json`, which are not present in this repository.

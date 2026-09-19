# Base44 Dev Environment

## Stack
Single-origin Node.js (ESM) Express app (`index.js`) serving static HTML/CSS/JS from the repo root and `public/`, plus proxy infrastructure (wisp, ultraviolet, scramjet, bare-mux, libcurl) and a WebSocket relay server. No build step — `public/` is served directly. Vite config exists but is not used by the runtime.

## Running
- `docker compose -f docker-compose.base44.yml up -d` — installs deps (`npm install --omit=optional`) then runs `npm start` (`node --watch index.js`, live-reload on server changes).
- Web entry on host port 3000. No external credentials needed (only `PORT`, defaults to 3000).
- Frontend HTML/CSS/JS edits require a browser refresh (no HMR); server edits auto-reload via `node --watch`.

## Quirks
- HTML page files (`index.html`, `apps.html`, ...) live at the repo root, not in `public/`. `index.js` route handler reads them from `process.cwd()` (root). Static assets (css/, js/, png/) are served from `public/` via `express.static`.
- Several routed pages (games, iframe, tools, settings, 404, etc.) are referenced in the routes array but their files were deleted/moved — only `/` and `/a` resolve to a page; others 500. This is the repo's current committed state.
- `--omit=optional` skips native optional deps (`bufferutil`, `utf-8-validate`) so no build toolchain is needed.

## Verify
- `curl -s -o /dev/null -w "%{http_code}" http://localhost:3000/` → 200
- `curl -s http://localhost:3000/ | grep "<title>"` → "Truffled - Math"

# Base44 dev environment

## What this is
The imported repository (`plataformaspjd4v2021`) is a **Unity 6 game project** (`PJD4V/`) that cannot run as a web app in this sandbox — it needs the Unity Editor to build, and no WebGL build exists in the repo.

The project the user actually wants to preview is **tiguinho** (`https://github.com/Klebersonkvc/tiguinho.git`), a static web game: `tigrinho.html` + `styles.css` + `script.js` + `img/` + `sound/`. It was originally served via VS Code Live Server on port 5502.

## How it runs here
`docker-compose.base44.yml` runs `nginx:alpine` on host port **3000**, bind-mounting the tiguinho source from `/tmp/tiguinho` (cloned fresh) as read-only web root. `nginx/default.conf` sets `index tigrinho.html` so the preview's `/` loads the game directly.

## Setup steps (fresh sandbox)
1. `git clone --depth 1 https://github.com/Klebersonkvc/tiguinho.git /tmp/tiguinho`
2. `docker compose -f docker-compose.base44.yml up -d --build`
3. Verify: `curl -s -o /dev/null -w '%{http_code}' http://localhost:3000/` → `200`

## Editing tiguinho
The tiguinho source lives in `/tmp/tiguinho` (outside the repo, ephemeral). Edits are made there via shell and reflected instantly since nginx serves the bind mount; call `reload_preview` to refresh the iframe. No build step, no live-reload dev server — it's static files.

## Verification
- `curl http://localhost:3000/` → HTML with `<title>FORTUNE CARAMELO</title>`
- Assets: `styles.css`, `script.js`, `img/*`, `sound/*` all return 200.

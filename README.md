# All In One Download

Download videos & audio from **YouTube, TikTok, Instagram, Twitter/X, Reddit, and many more** — directly in your browser. Free, no install, works on mobile & desktop.

## Live site
Deployed to GitHub Pages via Actions.

## How it works
This is a **static frontend** (no server) that talks to the **Cobalt API** (open, CORS-enabled instances). You paste link(s), the page returns a direct download URL / file.

Backend instances are tried in order; the first that responds is used. You can self-host your own Cobalt instance and add it to `INSTANCES` in `index.html`.

## Features
- Paste one link or many (newline / comma separated) for batch download
- Quality mode: Auto (best) / MP4 / MP3 / WebM
- Download file in browser or copy the direct URL
- PWA installable, offline shell
- Lightweight single-file frontend, no build step

## Run locally
Serve the folder with any static server, e.g.:
```
python3 -m http.server 8080
```
Then open http://localhost:8080

## Deploy on your own GitHub
1. Fork this repo.
2. Enable GitHub Pages → Source: **GitHub Actions**.
3. Push to `main`; the Pages workflow builds and deploys automatically.

## Notes / limitations
- Public instances can be rate-limited or go down; fallback instances are included.
- Some platforms block or require authentication for certain videos; results depend on API availability.
- Hosting a downloader for copyrighted content may violate platform terms — use responsibly.

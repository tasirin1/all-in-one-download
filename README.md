# All In One Download

Download videos & audio from **YouTube, TikTok, Instagram, Twitter/X, Reddit, and many more** — directly in your browser. Free, no install, works on mobile & desktop.

## Live site
Deployed to GitHub Pages via Actions → https://tasirin1.github.io/all-in-one-download

## How it works
- **Detection first, options after** (same flow as Tasirin Download Manager): paste a link → the app detects the platform → shows a preview card with thumbnail + title + choices (photo / video / resolution, e.g. 1080p/720p/... or MP3) → tap Download.
- Backend is a **static frontend** (no server):
  - **YouTube**: Piped API (`/streams/{id}`) — picks a directly downloadable proxied MP4 stream (verified CORS `*`). Out of the box, no setup.
  - **Other platforms** (TikTok/IG/Twitter/Reddit/...): **Cobalt API v10**. Public CORS instances are almost all dead/shut down as of 2026; paste your own self-hosted instance URL in **Advanced** (see [imputnet/cobalt](https://github.com/imputnet/cobalt)).

## Features
- Paste URL → auto-detect → thumbnail + title preview + format/resolution chips
- Single or multiple links (newline separated)
- Mode chips: video resolution (e.g. 1080p MP4), MP3 audio only, video without audio
- Download file in browser or copy the direct URL
- PWA installable, offline shell, light (single-file frontend)
- **Debug log panel**: footer → *Debug log*, or the *Debug* button on results. Every request/response/error is logged; use *Copy log* to share.

## Debugging
The site logs every step to an on-page log and to `console`.

1. Open the site, paste a link. If detection fails, the error appears in the red status box.
2. Open **Debug log** (footer) → read the timeline:
   - `youtube id ... via piped` — which backend was used
   - `piped <name>: HTTP ...` — HTTP errors
   - `blob fetch failed ...` — when the media URL can't be fetched cross-origin (it still opens in a new tab)
3. For full network detail: browser DevTools (**F12**) → **Console** (our `console.warn`/`console.error` appear) and **Network** tab (check the `streams/` request and the media `videoplayback` response status).
4. **Test all instances**: Advanced → *Test all instances* — pings each Piped/Cobalt backend and marks reachable/unreachable in the log.

### Common errors
| Symptom | Cause | Fix |
|---|---|---|
| `piped ... HTTP 500/502/timeout` | Public Piped instance down/rate-limited | Retry; fallback instance is tried automatically |
| `Could not detect: All Piped instances failed` | No Piped instance reachable | Try later or use your own Piped instance |
| `Non-YouTube requires a Cobalt instance` | TikTok/IG/etc. need Cobalt backend, none configured | Self-host Cobalt, paste URL in Advanced |
| `picker (multi-source) not supported` | Cobalt returned a multi-source picker | Not yet supported in UI |

## Error reporting to GitHub
The site catches errors automatically (global JS errors, unhandled promise rejections, download/detection failures) and can file them as **GitHub Issues** so you get notified fast.

- **With PAT (auto-report):** get a fine-grained personal access token with **Issues: write** on this repo only (GitHub → Settings → Developer settings → Fine-grained tokens). Paste it in the site → *Advanced → Error reporting → Save token*. It is kept **only in your browser** (`localStorage`), never committed. Every new error then POSTs to the Issues API automatically and creates an issue with the full debug log.
- **Without PAT:** use the **Report error** button (result card / Advanced) — it opens a GitHub issue form pre-filled with the debug log (works for anyone, no token needed).
- Anti-spam: identical errors are only reported once per 30 minutes; see `index.html` → `REPO_OWNER`/`REPO_NAME` if you fork.

> ⚠️ Security: a browser-side PAT can be seen by anyone using the site and restricted to Issues-write on this repo reduces—but does not eliminate—risk. For a public site you may prefer the manual Report button only; remove `auto` reporting by not saving a token.

## Self-host backends
- **Cobalt**: `docker run ghcr.io/imputnet/cobalt:latest` (see [docs/run-an-instance.md](https://github.com/imputnet/cobalt/blob/main/docs/run-an-instance.md)) — then paste `https://your-host` in Advanced (leave `/` off).
- **Piped**: [TeamPiped/Piped](https://github.com/TeamPiped/Piped) — set `PIPED` in `index.html`.

## Run locally
```
python3 -m http.server 8080
```
Open http://localhost:8080

## Deploy on your own GitHub
1. Fork this repo.
2. GitHub Pages → Source: **GitHub Actions**.
3. Push to `main`; the Pages workflow deploys automatically.

## Notes / limitations
- Public instances can be rate-limited or go down; fallback instances are included.
- Some platforms block or require authentication for certain media.
- Hosting a downloader for copyrighted content may violate platform terms — use responsibly.

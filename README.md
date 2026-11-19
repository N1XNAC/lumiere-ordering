# Lumiere — customer ordering site

Diner-facing ordering website (menu, cart, order status) for the restaurant
platform. Plain HTML/CSS/JS, deployed to GitHub Pages.

**Live URL:** https://n1xnac.github.io/lumiere-ordering/

## Required repository variable

Settings → Secrets and variables → Actions → **Variables** → New variable:

| Name | Value |
|---|---|
| `API_BASE_URL` | the backend origin, e.g. `https://lumiere-api.onrender.com` |

The build **fails** without it on purpose: the site would otherwise call
GitHub Pages for `/api` instead of the backend.

## How it deploys

Every push to `main` runs `.github/workflows/pages.yml`:

1. `node build.mjs` copies the site into `dist`
2. writes `js/runtime-config.js` with `window.__MV_CONFIG__.apiBaseUrl`
3. uploads `dist` as the Pages artifact

Pages must be enabled once: Settings → Pages → Source: **GitHub Actions**.

## Backend CORS

Allow this origin on the API (`CORS_ORIGIN`):

```
https://n1xnac.github.io
```

(Project Pages sites share the `https://<user>.github.io` origin; the path is
not part of CORS.)

## Local preview

```bash
API_BASE_URL=http://127.0.0.1:4000 node build.mjs
python3 -m http.server 5000 -d dist
```

Source of truth: the `customer-site/` folder in
[restaurant-platform](https://github.com/N1XNAC/restaurant-platform).
`deploy/github/publish-sites.sh` refreshes this repo from there.

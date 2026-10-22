# Brawler Index

Static Brawl Stars fan site for Cloudflare Workers. Data lives in `public/data/brawlers.json`
and images in `public/images/`, so the live site never calls any API.

```
public/            what gets deployed (index.html, style.css, app.js, _headers, data/, images/)
scripts/sync.mjs   pulls api.brawlapi.com into public/data and public/images
wrangler.jsonc     Cloudflare Workers config (serves ./public as static assets)
.github/workflows/sync.yml   lets GitHub run the sync for you
```

## 1. Upload to GitHub

Create a new empty repo, then upload everything in this folder (drag and drop on github.com works,
but make sure the hidden `.github` folder is included; or use git:)

```bash
git init && git add -A && git commit -m "Brawler Index"
git branch -M main
git remote add origin https://github.com/<you>/<repo>.git
git push -u origin main
```

## 2. Download all data + images (no tools needed on your PC)

On GitHub: **Actions** tab → **Sync Brawl data** → **Run workflow**.
(If Actions are disabled, click "I understand my workflows, go ahead and enable them" first.)

GitHub downloads every brawler's data, portraits, star power and gadget images and commits them
into `public/`. It takes about a minute. It also re-runs every Monday to pick up new brawlers.

Prefer doing it locally? `npm run sync` (Node 18+) does the same, then commit and push.

## 3. Deploy on Cloudflare Workers

Cloudflare dashboard → **Workers & Pages** → **Create** → **Import a repository** → pick the repo.

- Build command: leave empty
- Deploy command: `npx wrangler deploy` (the default)

`wrangler.jsonc` points it at `./public`. Every commit, including the weekly sync, redeploys.

## Notes

- The API's brawler data has star powers and gadgets. It has no hypercharge or buffie fields today;
  the sync stores them automatically if the API adds them, and the site shows them.
- Unofficial fan project. Follow Supercell's Fan Content Policy when publishing.
- Data and images come from Brawlify (api.brawlapi.com).

# IPO Tracker Website

A free, automatically-updated IPO tracker website for Mainboard and SME IPOs, hosted on GitHub Pages.

**Live site:** `https://devrajai.github.io/ipo-website/`

## How it works

```
Notion "IPO Tracker" database  →  GitHub Actions (daily 08:30 IST)  →  data.json  →  GitHub Pages website
        (updated at 08:00 IST)        scripts/fetch-notion.mjs                       index.html
```

1. An automated job updates the Notion IPO Tracker database every morning at 08:00 IST with fresh scraped data (open/upcoming IPOs, subscriptions, listing gains, fundamentals).
2. This repository's GitHub Action runs at 08:30 IST, pulls the database via the Notion API, and regenerates `data.json`.
3. GitHub Pages serves `index.html`, which renders the data with filters for board (Mainboard/SME), status, and search.

Total cost: ₹0 (Notion free plan, GitHub Actions free for public repos, GitHub Pages free).

## One-time setup (5 minutes)

### 1. Create a Notion integration token
1. Go to https://www.notion.so/my-integrations → **New integration**
2. Name it `ipo-website`, select your workspace, submit.
3. Copy the **Internal Integration Secret** (starts with `ntn_` or `secret_`).

### 2. Share the database with the integration
1. Open the **IPO Tracker** database in Notion.
2. Click the `•••` menu (top right) → **Connections** → add `ipo-website`.
3. Copy the database ID from the database URL — the 32-character string after the page name, e.g.
   `https://www.notion.so/IPO-Tracker-<DATABASE_ID>?v=...`

### 3. Add GitHub secrets
In this repository: **Settings → Secrets and variables → Actions → New repository secret**, add:
- `NOTION_TOKEN` = your integration secret
- `NOTION_DATABASE_ID` = the database ID from step 2

### 4. Verify
Go to **Actions** tab → **Update IPO Data** → **Run workflow** (manual trigger). If it succeeds, `data.json` gets committed and the site updates.

## Files
- `index.html` — the website (self-contained, no dependencies)
- `data.json` — IPO data snapshot (regenerated daily by the Action)
- `scripts/fetch-notion.mjs` — Notion API → data.json (Node 20, no npm packages)
- `.github/workflows/update-data.yml` — daily update workflow

## Data source
IPO data is collected from public sources (Chittorgarh IPO dashboard, SEBI/NSE/BSE filings) for informational purposes only. Not investment advice.

# Budget Tracker

A lightweight personal budget tracker that runs entirely in your browser and stores data in a Google Sheet you own. No accounts, no servers, no subscriptions — just a static page (HTML + CSS + JS) and a Google Apps Script Web App acting as the API.

## What's new in this revision

- **Simpler setup** — paste your Apps Script URL, hit **Test connection**, and you're done. The app validates the URL format and tells you immediately if the Sheet is reachable.
- **Better error messages** — clear feedback when the Apps Script returns HTML (deployment misconfigured), when the network is down, or when JSON parsing fails.
- **Mobile polish** — proper iOS safe-area handling (notch, home indicator), the FAB clears the iPhone home bar, the modal scrolls on small screens, and charts now resize correctly on rotation.
- **Charts fixed** — no more squashed/zero-height canvases on mobile. Each chart sits in a fixed-height container with `maintainAspectRatio:false`. Empty periods show a friendly placeholder instead of a broken render.
- **AI analysis works offline** — the old "Analyze with AI" button tried to call Anthropic directly from the browser, which fails on CORS and required an API key. It's been replaced with a local heuristic analysis that reads your data and gives you 4–5 paragraphs covering overall health, standout categories, account tilt (CC vs Debit), a concrete action, and something you're doing well. Zero config, no API key.

## Quick start

### 1. Create the Google Sheet + Apps Script

1. Create a new Google Sheet (any name).
2. Open **Extensions → Apps Script**.
3. Paste the backend code (see `Apps Script` section below — or use the script you already have).
4. **Deploy → New deployment → Web app**:
   - Execute as: **Me**
   - Who has access: **Anyone**
5. Copy the deployment URL — it must end in `/exec` and look like:
   `https://script.google.com/macros/s/AKfycb.../exec`

### 2. Open the app

1. Open `index.html` in your browser (or host it anywhere — GitHub Pages, Netlify, a USB stick, whatever).
2. Paste the Apps Script URL into the setup screen.
3. Click **Test connection** — you should see a green ✓ within a second.
4. Click **Save** and start tracking.

Your settings are stored in `localStorage`. To switch devices, just paste the same URL on the new device.

### 3. Adding entries

- Tap the **+** button (FAB) to add an expense.
- Pick a category, amount, account (Credit Card or Debit), and date.
- Entries flow into the current period's sheet tabs (`<period>_CC` and `<period>_DD`).

### 4. Periods

Each period starts on day **10** of the month by default (configurable in Settings). Crossing day 10 creates a fresh pair of tabs.

## Files

| File | Purpose |
|---|---|
| `index.html` | App shell — setup view, today/insights/charts/settings tabs |
| `app.js` | All app logic — API calls, chart rendering, local analysis |
| `styles.css` | Styles incl. iOS safe-area + chart sizing |
| `legacy-index.html` | Older single-file version (kept for reference) |

## Troubleshooting

**"Got HTML back instead of JSON"** — Your Apps Script is deployed but the URL probably points to the editor or an outdated deployment. Re-deploy as a Web App and use the new `/exec` URL.

**"Test connection" stays red** — Double-check:
- The URL ends with `/exec`.
- Deployment access is set to **Anyone** (not "Anyone with Google account").
- You re-deployed after the last code change in Apps Script (changes don't go live until you create a new deployment or pick **Manage deployments → edit → New version**).

**Charts look squashed** — Hard refresh (Cmd/Ctrl+Shift+R). The new CSS uses a `.chart-host` wrapper that should size every canvas correctly.

**AI analysis says "no data"** — You need at least a few entries in the current period for the heuristic to find anything to talk about.

## Stack

- Vanilla HTML/CSS/JS — no build step
- [Chart.js 4.4.1](https://www.chartjs.org/) via CDN
- Google Apps Script Web App as the backend
- Google Sheets as the database

## License

Personal project. Use it, fork it, change it.

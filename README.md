# SKYRA STORE

Game service management system for **Sky: Children of the Light**, built on Google Apps Script + Google Sheets.

## Services

### 💙 Heart Service (`/heart`)
Heart sending service with 3 product types: Instant, Special, and Reguler.

**Workers:** John, Cyha, Keysha, Sasa, Nanas

| File | Description |
|------|-------------|
| `Code.gs` | Backend — order routing, delivery system, payroll, refund, archive |
| `customer.html` | Customer panel — order form, tracking, cart, refund request |
| `admin.html` | Admin dashboard — orders, finance, refunds, blacklist, worker management |
| `worker.html` | Universal worker template — dynamically loads per worker name |

### 🎮 Joki Service (`/joki`)
Account running/joki service: Candle Run, Eden, WL, Shard, etc.

**Workers:** Bayu, Elia, Ciel

| File | Description |
|------|-------------|
| `Code.gs` | Backend v4 — 11 features (blacklist, refund, returning discount, etc.) |
| `customer.html` | Customer panel — cart, bilingual, refund request |
| `admin.html` | Admin dashboard — revenue, refunds, blacklist, finance |
| `worker.html` | Universal worker template — proof upload, mark done, payroll |

## Architecture

Both services use a **dynamic worker system** — a single `worker.html` template that renders per worker based on URL parameter. Adding a new worker requires only adding a row in the Workers/JokiWorkers spreadsheet. No code changes needed.

```
?page=admin     → Admin dashboard
?page=john      → Worker John panel (Heart)
?page=bayu      → Worker Bayu panel (Joki)
(no parameter)  → Customer panel
```

## Features

- **Dynamic Worker System** — add/remove workers from spreadsheet only
- **Dynamic Pricing** — edit prices from Google Sheet, auto-update in web (1 min cache)
- **Blacklist** — block problematic contacts
- **Auto-Cancel** — unpaid orders cancelled after 24h
- **Returning Customer Discount** — Bronze (≥1) / Silver (≥6) / Gold (≥11) tiers
- **Refund System** — customer request → admin approve/reject
- **Multi-Order Cart** — order multiple services at once
- **Payment Polling** — auto-detect when admin marks paid
- **Bilingual** — Indonesian / English toggle
- **Worker Payroll** — auto-calculated from delivery logs (Heart) / flat rate per order (Joki)
- **Ex-Worker Display** — admin sees which workers previously handled a customer
- **Worker Deactivation** — one-click reassign all orders + set inactive (Heart)
- **Archive** — old data auto-archived after 60 days
- **PIN Security** — lockout after 5 failed attempts (15 min)
- **Consistency Check** — auto-detects orphan orders (Joki)

## Deployment

### Prerequisites
- Google Spreadsheet with required sheets
- Google Apps Script project linked to the spreadsheet

### Steps
1. Create new Apps Script project from spreadsheet (Extensions → Apps Script)
2. Paste `Code.gs` into the default script file
3. Create HTML files (`customer`, `admin`, `worker`) and paste contents
4. Deploy → New deployment → Web app → Anyone can access
5. Run `setupPriceSheets()` once to create PriceList & WorkerRates sheets
6. Run `setupTriggers()` once to set up automation (auto-cancel, archive, etc.)

### Updating
- **Code.gs changes** → Save → Deploy → Manage deployments → New version → Deploy
- **HTML changes** → Save only (no re-deploy needed)
- **Price changes** → Edit PriceList/WorkerRates sheet directly (auto-updates in ~1 min)
- **Add worker** → Add row in Workers/JokiWorkers sheet + add PIN in Settings

## Tech Stack
- Google Apps Script (backend)
- Google Sheets (database)
- Vanilla HTML/CSS/JS (frontend, single-file per panel)
- No external dependencies

## Landing Page
`index.html` serves as the GitHub Pages landing page at `skyrastore.github.io/skyra_store/`, with SEO meta tags and an iframe overlay to the Apps Script web app.

## License
Private — Skyra Store © 2026

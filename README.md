# SKYRA STORE

Game service management system for **Sky: Children of the Light**, built on Google Apps Script + Google Sheets.

Live: [skyrastore.github.io/skyra_store](https://skyrastore.github.io/skyra_store/) · IG: [@skyra_store2](https://instagram.com/skyra_store2)

## Services

### 💙 Heart Service (`/heart`)
Heart sending service with 3 product types: Instant, Special, and Reguler.

**Workers:** John, Cyha, Keysha, Sasa, Nanas

| File | Description |
|------|-------------|
| `Code.gs` | Backend — order routing, delivery system, payroll, refund, archive, Telegram notify |
| `customer.html` | Customer panel — order form, tracking, cart, refund request, reorder |
| `admin.html` | Admin dashboard — orders, finance, refunds, blacklist, worker management, raw settings editor |
| `worker.html` | Universal worker template — dynamically loads per worker name |

### 🕯️ Run Service (`/joki`) — *internal name: Joki*
Account running service: Candle Run, Eden, WL Collection, Shard, Daily Task, Light Spirit, etc.

**Workers:** Bayu, Elia, Ciel

| File | Description |
|------|-------------|
| `Code.gs` | Backend — slot-based proof system, split-flow, payroll, refund, archive |
| `customer.html` | Customer panel — Via Login / Via Gandeng flow, cart, bilingual, refund request |
| `admin.html` | Admin dashboard — revenue, refunds, finance, pricing matrix, settings KV editor |
| `worker.html` | Universal worker template — slot photo upload, mark done, payroll |

> URL path stays `/joki` for backward-compat; customer-facing brand is **RUN SERVICE**.

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
- **Returning Customer Discount** — auto 1% off on reorder with prior Record ID
- **Voucher System** — admin creates code, customer applies at checkout (date/product/tier filter, max-use cap, min-order, max-disc cap)
- **Refund System** — customer request → admin approve/reject, voucher auto-restored
- **Multi-Order Cart** — order multiple services at once
- **Payment Polling** — auto-detect when admin marks paid
- **Bilingual** — Indonesian / English toggle (both panels + landing)
- **Worker Payroll** — auto-calculated from delivery logs (Heart) / per-slot proof (Joki)
- **Ex-Worker Display** — admin sees which workers previously handled a customer
- **Worker Resign Wizard** — 3-step: freeze → migrate orders → settle payroll → permanent delete + blacklist
- **Split Flow** (Joki) — admin can split a partially-done order, payroll reconciled automatically
- **Archive** — old completed/cancelled orders auto-archived (Heart: 60 days, Joki: 1 year)
- **PIN Security** — lockout after failed attempts
- **Consistency Check** — auto-detects orphan orders
- **Telegram Notifications** — order assigned, payroll summary; auto-clears stale chat IDs (kicked/blocked/deleted group)
- **Settings KV Editor** — admin edits arbitrary Settings sheet keys from web UI (undoable)
- **Text-format-locked columns** — Telegram chat ID & contact phone numbers stored as text to prevent scientific-notation corruption

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
7. (Optional) Run `migrateSettings*()` once to organize Settings sheet
8. (Optional) Run `repairData*()` once to fix scientific-notation in legacy data

### iframe Embedding (Landing Page)
For `index.html` iframe overlay to work, `doGet()` must set ALLOWALL:
```js
return HtmlService.createTemplateFromFile('Customer').evaluate()
  .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);
```
Without this, browser blocks embedding and customer sees "Open in New Tab" fallback.

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
`index.html` serves as the GitHub Pages landing page at `skyrastore.github.io/skyra_store/`.

Features:
- SEO meta (description, keywords, canonical, robots)
- Open Graph + Twitter card for WA/IG link preview
- JSON-LD `Store` schema for Google rich results
- Bilingual ID/EN toggle
- Animated stars background (Sky-themed gradient)
- Two service cards: HEART SERVICE + RUN SERVICE
- Trust strip (orders count, rating, refund badge)
- How it works section (3-step)
- iframe overlay to Apps Script web app (no redirect, stays on brand)
- Graceful fallback to "Open in New Tab" if iframe blocked

## License
Private — Skyra Store © 2026

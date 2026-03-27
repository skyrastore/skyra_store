# SKYRA STORE

Game service management system for **Sky: Children of the Light**, built on Google Apps Script + Google Sheets.

## Services

### 💙 Heart Service (`/heart`)
Heart sending service with 3 product types: Instant, Special, and Reguler.

**Workers:** John, Cyha, Keysha

| File | Description |
|------|-------------|
| `Code.gs` | Backend — all business logic, APIs, automation |
| `customer.html` | Customer-facing order form + tracking |
| `admin.html` | Admin dashboard — orders, finance, workers |
| `john.html` | Worker panel — John (Instant/Special/Reguler) |
| `cyha.html` | Worker panel — Cyha (Instant/Reguler) |
| `keysha.html` | Worker panel — Keysha (Instant/Reguler) |

### 🎮 Joki Service (`/joki`)
Account running/joki service: Candle Run, Eden, WL, Shard, etc.

**Workers:** Bayu, Elia, Ciel

| File | Description |
|------|-------------|
| `Code.gs` | Backend v4 — 11 features (blacklist, refund, returning discount, etc.) |
| `customer.html` | Customer panel — cart, bilingual, refund request |
| `admin.html` | Admin dashboard — refunds, blacklist, finance |
| `bayu.html` | Worker panel — Bayu |
| `elia.html` | Worker panel — Elia |
| `ciel.html` | Worker panel — Ciel |

## Features

- **Dynamic Pricing** — edit prices from Google Sheet, auto-update in web (1 min cache)
- **Blacklist** — block problematic contacts
- **Auto-Cancel** — unpaid orders cancelled after 24h
- **Returning Customer Discount** — Bronze/Silver/Gold tiers
- **Refund System** — customer request + admin approve/reject
- **Multi-Order Cart** — order multiple services at once
- **Payment Polling** — auto-detect when admin marks paid
- **Bilingual** — Indonesian / English toggle
- **Worker Payroll** — auto-calculated from delivery logs
- **Archive** — old data auto-archived monthly
- **PIN Security** — lockout after 5 failed attempts

## Deployment

### Prerequisites
- Google Spreadsheet with required sheets
- Google Apps Script project linked to the spreadsheet

### Steps
1. Create new Apps Script project from spreadsheet (Extensions → Apps Script)
2. Paste `Code.gs` into the default script file
3. Create HTML files (`customer`, `admin`, worker names) and paste contents
4. Deploy → New deployment → Web app → Anyone can access
5. Run `setupPriceSheets()` once to create price sheets
6. Run `setupTriggers()` once (Joki only) for automation

### Updating
- **Code.gs changes** → Save → Deploy → Manage deployments → New version → Deploy
- **HTML changes** → Save only (no re-deploy needed)
- **Price changes** → Edit PriceList/WorkerRates sheet directly (auto-updates in ~1 min)

## URL Parameters

Access different panels via URL:
```
?page=admin     → Admin dashboard
?page=john      → Worker John panel (Heart)
?page=bayu      → Worker Bayu panel (Joki)
(no parameter)  → Customer panel
```

## Tech Stack
- Google Apps Script (backend)
- Google Sheets (database)
- Vanilla HTML/CSS/JS (frontend, single-file per panel)
- No external dependencies

## License
Private — Skyra Store © 2026

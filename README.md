<div align="center">

# Construction Ledger ERP

**A web ledger for a construction company: one Main Ledger of payments and receipts, with Head, Item, Merchant and Account drill-downs showing billed, paid and due.**

React · Vite · Tailwind · Cloudflare Workers · Hono · Drizzle · D1 (SQLite)

![React](https://img.shields.io/badge/React_19-20232A?logo=react&logoColor=61DAFB)
![Vite](https://img.shields.io/badge/Vite-646CFF?logo=vite&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?logo=tailwindcss&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![Cloudflare Workers](https://img.shields.io/badge/Cloudflare_Workers-F38020?logo=cloudflare&logoColor=white)
![SQLite](https://img.shields.io/badge/D1_SQLite-003B57?logo=sqlite&logoColor=white)

<table>
  <tr>
    <td><img src="assets/screenshots/ledger-erp-main-ledger.webp" width="440" alt="Main Ledger with filters, totals for received, paid, billed and due, and an entries table" /></td>
    <td><img src="assets/screenshots/ledger-erp-heads.webp" width="440" alt="Heads sub-ledger listing expense heads with billed, paid and due amounts" /></td>
  </tr>
  <tr>
    <td><img src="assets/screenshots/ledger-erp-account-detail.webp" width="440" alt="Account statement with opening balance, money in and out, and running balance" /></td>
    <td><img src="assets/screenshots/ledger-erp-new-entry.webp" width="440" alt="New entry dialog with payment out, bill and money in modes" /></td>
  </tr>
</table>
<sub>Screenshots are real captures of a local build running on fictional demo data.</sub>

</div>

---

## Product at a Glance

| | |
|---|---|
| **What it is** | Accounting ledger that replaces an Excel workbook for a construction company |
| **Core model** | Entries with direction (in/out), billed and paid amounts; due is derived |
| **Views** | Main Ledger, Heads > Items > Merchants, Accounts, Setup |
| **Backend** | Cloudflare Worker (Hono, Drizzle) on D1; R2 binding configured |
| **Auth** | JWT sign-in, read-only VIEWER role, audit log on changes |
| **Migration** | Python Excel import with a verification script |
| **AI** | None; no ML or LLM code |

## Product Showcase

<table>
  <tr>
    <td align="center"><img src="assets/screenshots/ledger-erp-head-detail.webp" width="280" alt="Head detail with items and their billed, paid and due totals"/><br/><sub><b>Head detail</b><br/>Items under one head</sub></td>
    <td align="center"><img src="assets/screenshots/ledger-erp-item-detail.webp" width="280" alt="Item detail page"/><br/><sub><b>Item detail</b><br/>Merchants under one item</sub></td>
    <td align="center"><img src="assets/screenshots/ledger-erp-merchants.webp" width="280" alt="Merchants summary list"/><br/><sub><b>Merchants</b><br/>Billed, paid, due per vendor</sub></td>
  </tr>
  <tr>
    <td align="center"><img src="assets/screenshots/ledger-erp-merchant-detail.webp" width="280" alt="Merchant detail with transactions"/><br/><sub><b>Merchant detail</b><br/>Entries for one vendor</sub></td>
    <td align="center"><img src="assets/screenshots/ledger-erp-setup.webp" width="280" alt="Setup page with merchant list and active toggles"/><br/><sub><b>Setup</b><br/>Heads, merchants, sites, accounts</sub></td>
    <td align="center"><img src="assets/screenshots/ledger-erp-new-entry.webp" width="280" alt="New entry dialog"/><br/><sub><b>New entry</b><br/>Payment out, bill, money in</sub></td>
  </tr>
</table>

## Key Features

- **Main Ledger.** Search and filter by date range, site, head, item, merchant, account, direction, unpaid only and needs review; totals for received, paid, billed and due; create, edit and delete entries; CSV export.
- **Sub-ledgers.** Heads drill down to items and merchants, each level showing billed, paid and due.
- **Accounts.** Bank, cash and card accounts with opening balance and a running-balance statement.
- **Setup.** Maintain sites, heads, items, merchants and accounts, with active toggles.
- **Roles.** Sign-in with JWT; the VIEWER role is read-only.
- **Import.** Python pipeline from the legacy Excel workbook to seed SQL, with a verify script.

## Architecture

```mermaid
flowchart LR
    FE["React SPA"] -->|"/api"| API["Worker: Hono routes"]
    API --> AUTH["JWT + role guard"]
    API --> ORM["Drizzle"] --> DB[("D1 SQLite")]
    IMP["Python import + verify"] --> DB
```

The SPA calls a Cloudflare Worker that validates input, checks the role, and queries D1 through Drizzle. Details: [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Engineering Highlights

- **Integer money.** Amounts are stored as integer minor units and converted at the API boundary.
- **Billed versus paid.** Due is derived from the two amounts, so it rolls up consistently across every sub-ledger.
- **Server-side validation.** Direction, amount and account rules are enforced in the Worker.
- **Grouped SQL summaries.** Drill-down totals come from aggregate queries rather than per-row lookups.
- **Verified import.** A script re-derives rows and asserts totals and foreign keys on a scratch database copy.

More: [docs/ENGINEERING_CHALLENGES.md](docs/ENGINEERING_CHALLENGES.md).

## Status and Scope

- This is a production application for one company; the screenshots come from a demo build on a generated, fictional dataset.
- No AI or machine learning is involved.
- An earlier double-entry engine (invoices, GL, reports) exists in the codebase but is not routed in the current app.
- Automated checks are an API smoke-test script and the import verifier; there are no UI tests.
- Production source, data and configuration are private.

## Documentation

| Read | For |
|---|---|
| [Technical case study](docs/TECHNICAL_CASE_STUDY.md) | Problem, approach, decisions |
| [Architecture](docs/ARCHITECTURE.md) | Layers, data model, API surface |
| [Engineering challenges](docs/ENGINEERING_CHALLENGES.md) | Problem, approach, outcome |
| [Product overview](docs/PRODUCT_OVERVIEW.md) | Screens and money model |
| [Privacy and source](docs/PRIVACY_AND_SOURCE.md) | What is and is not published |

## Source Code

Production source code is maintained privately. See [RIGHTS.md](RIGHTS.md) for usage terms.

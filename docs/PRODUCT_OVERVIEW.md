# Product Overview

## What it is

Construction Ledger ERP is a web accounting tool for a construction company, built to replace an Excel workbook. It centres on one Main Ledger of money paid and received, plus drill-down sub-ledgers (Head, Item, Merchant), bank and cash accounts, and master-data setup.

## Screens

- **Main Ledger.** Filterable entry list (search, date range, site, head, item, merchant, account, direction, unpaid only, needs review) with totals for received, paid, billed and due. Entries can be created, edited and deleted from the list.
- **Heads, Items, Merchants.** Cost heads drill down to items and merchants, each level showing billed, paid and due. Merchants have their own summary and detail pages.
- **Accounts.** Bank and cash accounts with running balances.
- **Setup.** Master data: sites, heads, items, merchants and accounts.
- **Sign-in and roles.** JWT sign-in. The VIEWER role is read-only; OWNER, ACCOUNTANT and MANAGER can write.

## Money model

Each entry has a direction (IN or OUT), a billed amount and a paid amount. For OUT entries, due is billed minus paid. IN entries record money received.

## Also in the codebase

- A Python Excel import pipeline with a verification script (used to migrate a legacy workbook).
- An audit log written on entry and master-data changes.
- An earlier double-entry engine (invoices, GL, reports) that exists in code but is not routed in the current app.

## Demo

Screenshots and the demo build use fictional data for a made-up company, "Summit Ridge Builders (Demo)".

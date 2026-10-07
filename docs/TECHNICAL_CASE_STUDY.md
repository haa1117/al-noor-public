# Technical Case Study

## Problem

A construction company tracked payments, supplier bills and site costs in an Excel workbook with separate sub-ledger sheets. Totals had to be reconciled by hand and there was no change history or access control.

## Approach

Model the workbook as one ledger plus filtered views. Every entry carries a site, head, item, merchant and account; sub-ledgers become summaries over the same table, so the Main Ledger and the Head, Merchant and Account pages cannot drift apart.

## Stack

- **Frontend:** React 19, Vite, Tailwind, Radix primitives, react-hook-form with zod.
- **Backend:** Cloudflare Worker, Hono, Drizzle ORM, D1 (SQLite), R2 binding.
- **Auth:** JWT via jose, passwords hashed with bcryptjs; the token is kept client-side and sent as a bearer header.
- **Import:** Python scripts (extract, build seed, verify) from the Excel workbook to seed SQL.

## Key decisions

**Integer money.** Amounts are stored as integer paisa and converted at the API boundary, avoiding float rounding in sums.

**Billed versus paid.** OUT entries track what was billed and what was paid separately, so due (billed minus paid) is derived and can be summed per head, item, merchant or account.

**Server-side validation.** Write rules (direction, amounts, required account, valid dates) are enforced in the Worker, not only in forms.

**Grouped SQL summaries.** Totals and drill-down rows are produced by aggregate queries, so the number of queries per screen does not grow with the number of rows.

**Role guard.** One middleware makes VIEWER read-only; write routes opt in to it.

## Verification

- A verify script copies a scratch database, loads the import, and asserts entry counts, paid totals per direction and account, foreign-key integrity and per-row fields against the extracted source.
- An API smoke-test script exercises login and common flows.
- No automated UI tests.

## Demo build

The demo uses a generated, fictional "Summit Ridge Builders (Demo)" dataset in a separate local D1 state, so it never touches production data.

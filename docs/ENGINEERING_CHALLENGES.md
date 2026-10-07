# Engineering Challenges

## 1. Exact money

**Problem:** Floating-point sums drift, which is unacceptable in a ledger.
**Approach:** Store integer paisa, convert to and from decimal only at the API boundary, and sum integers in SQL.
**Outcome:** Totals are exact integers, and the import verifier can assert sums to the paisa.

## 2. Billed versus paid

**Problem:** A supplier bill and the payment against it are different events, and the workbook mixed them.
**Approach:** Each OUT entry keeps `billed` and `paid`; due is derived as billed minus paid. IN entries record received money only. Server validation rejects combinations that make no sense.
**Outcome:** Due balances roll up consistently by head, item, merchant and account, and an "unpaid only" filter is a single condition.

## 3. Drill-down without loading every row

**Problem:** Head, item and merchant pages need totals over thousands of entries.
**Approach:** Summary endpoints return grouped aggregates per level; the entry list is paginated (limit capped at 1000).
**Outcome:** Each page fetches only what it shows.

## 4. Trustworthy Excel migration

**Problem:** Years of workbook data needed to move without silent loss.
**Approach:** Python scripts extract rows, classify heads, items and merchants, and generate seed SQL; rows that could not be classified confidently are flagged `needs_review`. A verify script re-derives per-row fields and asserts totals and foreign keys against a scratch copy of the database.
**Outcome:** The import is repeatable (wipe, then seed) and checkable without touching a live database.

## 5. Read-only users

**Problem:** Some users should see figures but not edit.
**Approach:** A write-access middleware rejects VIEWER requests with a clear 403 message; every mutating route uses it.
**Outcome:** One place defines who can write.

## 6. Unreleased double-entry engine

The repository also contains an earlier double-entry design (invoices, GL, reports, reconciliation routes). It is intentionally not routed in the current app, which uses the simpler ledger. This is kept as reference rather than presented as a feature.

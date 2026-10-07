# Privacy and Source

## Source code

This repository is a portfolio showcase for a production application. The production source is private; no licence to it is granted. See [RIGHTS.md](../RIGHTS.md).

## Data

The production application holds real business financial records. None of that data is included here, and it was not used for any screenshot.

All screenshots come from a local build running on a generated, fictional dataset for "Summit Ridge Builders (Demo)". The demo database is created by a seeded script and is separate from any real database.

## Secrets and configuration

Environment files, signing keys, credentials and deployment configuration are excluded from this repository.

## Third-party components

The app is built on open-source libraries, including React, Vite, Tailwind CSS, Radix UI, Hono, Drizzle ORM, jose and bcryptjs, and runs on Cloudflare Workers, D1 and R2. Each remains under its own licence and trademark.

## Demo

The demo build runs a local Worker and a local Vite dev server against the fictional database. It does not connect to any production system.

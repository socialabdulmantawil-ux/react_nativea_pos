# Phase 1 — Original Repository Audit

**Audit date:** 2026-11-03  
**Repository:** [socialabdulmantawil-ux/brew-bean](https://github.com/socialabdulmantawil-ux/brew-bean)  
**Branch / revision inspected:** `main` / `82dc22193597a198c7e10769ba27203a7ffee7d4`  
**Method:** Public GitHub source/API inspection and read-only browser inspection. No clone, source changes, commits, pushes, dependency installs, or order submissions.

## Source references

The following links are pinned to the audited commit:

- [App bootstrap, calculations, and order submission](https://github.com/socialabdulmantawil-ux/brew-bean/blob/82dc22193597a198c7e10769ba27203a7ffee7d4/App.tsx#L138-L156)
- [App financial calculations and submitted payload](https://github.com/socialabdulmantawil-ux/brew-bean/blob/82dc22193597a198c7e10769ba27203a7ffee7d4/App.tsx#L443-L530)
- [Inert Hold Order control](https://github.com/socialabdulmantawil-ux/brew-bean/blob/82dc22193597a198c7e10769ba27203a7ffee7d4/App.tsx#L677-L681)
- [Server order validation, pricing, and payment status](https://github.com/socialabdulmantawil-ux/brew-bean/blob/82dc22193597a198c7e10769ba27203a7ffee7d4/server/index.mjs#L57-L180)
- [Server transactions and API routes](https://github.com/socialabdulmantawil-ux/brew-bean/blob/82dc22193597a198c7e10769ba27203a7ffee7d4/server/index.mjs#L190-L310)
- [SQLite schema](https://github.com/socialabdulmantawil-ux/brew-bean/blob/82dc22193597a198c7e10769ba27203a7ffee7d4/server/schema.sql)
- [Database initialization and bootstrap/order queries](https://github.com/socialabdulmantawil-ux/brew-bean/blob/82dc22193597a198c7e10769ba27203a7ffee7d4/server/db.mjs)
- [Seed catalog, sizes, add-ons](https://github.com/socialabdulmantawil-ux/brew-bean/blob/82dc22193597a198c7e10769ba27203a7ffee7d4/server/seed.mjs)
- [Static Pages deploy workflow](https://github.com/socialabdulmantawil-ux/brew-bean/blob/82dc22193597a198c7e10769ba27203a7ffee7d4/.github/workflows/deploy.yml)
- [Latest failed build run](https://github.com/socialabdulmantawil-ux/brew-bean/actions/runs/41483384981)

## Executive summary

The original is a Vite + React + TypeScript web POS, not a native application. The checked-in implementation uses a local Node.js API backed by a single SQLite file; it does not contain Django, PostgreSQL, PowerSync, multi-device synchronization, authentication, or an offline queue.

The browser-deployed site currently fails to load `/api/bootstrap` (HTTP 404), displays zero catalog products, and nevertheless displays an "Online" indicator and a prefilled sample cart. The latest public GitHub Actions build for the inspected `main` revision failed with JSX syntax errors in `App.tsx`; deployment was skipped. The served page is therefore not a usable functional reference for checkout or catalog behavior.

The source contains useful parity evidence for catalog, cart, checkout, payments, tax/discount calculations, inventory decrement, and receipts. These are source-observed behaviors, not verified end-to-end workflows. One important business-correctness gap is that the API accepts a client-supplied product base price without comparing it to the database price.

## Repository and technology inventory

| Area | Verified finding |
|---|---|
| Application | Vite, React, React DOM, TypeScript, Tailwind CSS, Lucide React. |
| Entry points | `main.tsx` mounts `App.tsx`; `index.html` loads `/main.tsx`. |
| UI | `App.tsx` plus reusable components for catalog, cart, item controls, and dialogs. |
| API | A small Node.js HTTP server in `server/index.mjs`; SQLite access and bootstrap/order queries are in `server/db.mjs`. |
| Database | SQLite at `data/brew-bean.sqlite` by default; `SQLITE_DB_PATH` can override it. Schema and seed data are checked in. |
| Runtime | `package.json` requires Node.js >=22.5.0; the API uses the built-in `node:sqlite` module. |
| Development command | `npm run dev` starts the API and Vite together; `npm run server` starts only the API. |
| Build / deployment | GitHub Actions builds the Vite `dist` output under Node.js 20 and deploys to Pages; it does not deploy the local Node API. The workflow Node version is below the package's declared >=22.5.0 runtime requirement. |
| Tests | No test script or test files appear in the inspected repository tree. No local test/build was run. |
| Original root | No README is present in the inspected `main` tree. |

## Feature inventory and observed behavior

### Catalog and customization

- Four seeded categories: Coffee, Drinks, Bakery, and Meals.
- Fifteen seeded products with image URLs, stock counts, tags, prices, and default size/add-on selections.
- Four size options with cent-denominated adjustments (`Small -0.50`, `Regular 0`, `Large +0.75`, `X-Large +1.25`).
- Twenty add-ons, each associated with a product category.
- Name search and category filtering are implemented in the catalog.
- Source permits size selection for Coffee and Drinks; add-on availability is category-scoped.
- Product cards show stock and distinguish unavailable/low-stock products.

### Cart and order workflow

- Add/remove items; adjust quantities; edit size, add-ons, and notes.
- Long-press quantity controls and a quantity-entry modal are implemented.
- Cart state is held in React component state; no persisted draft/held-order store was found.
- Order types are Dine-in, Takeout, and Delivery. A fixed customer selector has Walk-in, a sample VIP, and two table choices; no customer-management model was found.
- A visible **Hold Order** button has no action handler in the active app implementation.
- Delivery captures a provider name and external order number; no delivery-platform integration was found.

### Financial calculations and payments

- Client and API calculate subtotal, a configured promotional discount, tax, and total.
- Seed settings specify 10% promotional discount, 15% tax, and currency `PHP`.
- The promotion is a fixed configured percentage controlled by a boolean `promoApplied`; the submitted promo-code text is stored but is not validated as a code.
- The server converts amounts to integer cents and rounds percentage discount/tax to cents.
- Payment choices are Card, Cash, and Wallet. Cash checks tender and change; delivery is stored with pending payment.
- The card flow records only four digits and marks the payment paid; no payment processor or transaction authorization integration was found.
- The backend uses database prices for add-ons and validates a known size's adjustment, but accepts `item.basePrice` from the client without checking that it matches the product's stored price. This conflicts with the project's backend-authority requirement and must be addressed in the migration.

### Orders, inventory, and receipts

- Order creation is wrapped in a SQLite transaction. It reserves/increments a single local sequence beginning at 1048, checks stock, inserts order/item/add-on/payment/delivery rows, and decrements stock.
- Supported order statuses in the schema are `completed` and `confirmed`; payment statuses are `paid` and `pending`.
- `GET /api/orders` returns recent orders, but no order-history screen or search/filter UI was found in the app.
- Receipts render an 80mm browser-print layout through `window.print()` and print CSS. No native Bluetooth/USB/network printer integration was found.
- Inventory is displayed and decremented on sale; no stock adjustment, stock movement, or low-stock management workflow was found.

### Features absent from the inspected source

No login/authentication, role or permission system, business/tenant isolation, device registration, synchronization engine, persistent offline queue, or multi-device support was found. No refund, void, shift, cash-drawer, end-of-day, or customer-management workflow was found. The owner has requested these operational features in scope for the migration; requirements and phase placement remain to be designed.

## Database assessment

The owner-reported table list matches the checked-in SQLite schema. These are now **Verified in original source** (not a PostgreSQL schema assessment):

| Table | Observed structure / behavior |
|---|---|
| `categories` | Category IDs and unique names. |
| `products` | Product/category, integer-cent price, nonnegative stock, image/tag/default size, active flag. |
| `size_options` | Unique size names and integer-cent price adjustments. |
| `add_ons` | Unique add-on names, integer-cent prices, active flag. |
| `category_add_ons` | Category-to-add-on association with composite primary key. |
| `product_default_add_ons` | Product-to-default-add-on association. |
| `settings` | Key/value settings; seed values are tax 15, promotion 10, currency PHP. |
| `order_number_sequence` | One-row sequence seeded at 1048. |
| `orders` | Order/customer/status/payment status, promotion, cent totals, currency, timestamp. |
| `order_items` | Product snapshot, quantity, size, price snapshots, line total, notes. |
| `order_item_add_ons` | Add-on snapshots on order lines. |
| `payments` | Method/status/amount, cash/change, card last four, wallet reference. |
| `delivery_orders` | Provider and external delivery order number. |

SQLite foreign keys, `CHECK` constraints, and indexes exist for category/product and order lookup paths. The schema provides no tenant/business, user, device, sync metadata, refund, shift, or cash-drawer structures.

## API assessment

| Method and path | Verified behavior |
|---|---|
| `GET /api/health` | Returns `{ status: "ok" }`. |
| `GET /api/bootstrap` | Returns categories, active products, size/add-on options, settings, and next sequence number. |
| `POST /api/orders` | Validates order and payment fields, recalculates totals, checks stock, then writes order-related records and decrements stock transactionally. |
| `GET /api/orders?limit=50` | Returns recent orders; limit defaults to 50 and is clamped to 1–200. |

The API is a local Node server, not Django Ninja. No authentication/authorization middleware or idempotency key was found. The API does perform several useful validations, but the client-controlled base-price gap described above remains.

## Design and visual references

- Dark navy background (`#0b101d`), dark panel/header surfaces (`#131b2e`), blue primary actions, slate text, and Inter/system sans typography.
- Tablet-like two-pane POS composition: order/cart and summary on the left, searchable product catalog on the right.
- Responsive CSS includes a narrow-screen body overflow rule; native tablet portrait/landscape behavior is not verified.
- A single browser reference capture is in `docs/reference/deployed-pos-unavailable.png`. It records the actual deployed state, including the 404 error. It is not a clean design baseline.
- Modal, product-catalog, checkout-success, and print-reference captures could not be collected from the deployment because bootstrap fails and no usable server is deployed. Source code is the only evidence for those states.

## Deployment and runtime verification

1. Opened the public Pages deployment read-only on 2026-11-03.
2. The browser request to `/api/bootstrap` returned HTTP 404. The page showed “SQLite database unavailable”, zero available catalog products, and still showed “Online” and prefilled sample cart items. Screenshot saved under `docs/reference/`.
3. Queried the public Actions status for the inspected `main` commit. The latest run (`41483384981`) failed its Build step; deploy was skipped. GitHub reported JSX parse errors in `App.tsx` at/around lines 544, 728, 785, 821, 823, 1000, and 1527.
4. No dependencies were installed and no build/test commands were run in this workspace.

## Migration risks and decisions

- **High — server-authority gap:** replace all client-authoritative product-price inputs with server lookups/validated identifiers and preserve verified formulas only by owner approval.
- **High — current deployment/build unavailable:** source has an observed CI parse failure; Pages does not host the API. Use source audit and a working, owner-approved reference environment before relying on live behavior.
- **Medium — runtime version mismatch:** package metadata requires Node.js >=22.5.0, while the Pages build workflow selects Node.js 20. The backend's built-in SQLite runtime requires the newer Node version; verify supported CI and server runtime versions.
- **High — payment state is simulated:** the current Card/Wallet flow can mark a payment paid without processor confirmation. A real payment integration and allowed offline state must be specified.
- **High — multi-device/offline gap:** the local sequence and SQLite file are single-installation only. Owner deferred the order-numbering strategy to Phase 2; PowerSync + SQLite is approved subject to Phase 2 validation and proof of concept.
- **High — operational scope gap:** refunds, voids, shifts, cash drawer, and end-of-day are owner-requested new migration scope, not existing behaviors to port.
- **Medium — user/security scope:** manager-only access is requested; the original contains no login or authorization. Define manager identity, permissions, device access, and data isolation before implementation.
- **Medium — locale/currency mismatch:** original source is configured for Philippine pesos and 15% tax. Owner deferred Saudi requirements; do not silently retain or replace locale/tax choices.
- **Medium — incomplete interaction references:** only the default page shell could be captured; runtime checkout and catalog behavior remain unverified due the deployed API 404.

## Approval boundary / next step

Phase 1 is documented for owner review. Do not modify the original repository. Phase 2 and implementation remain paused pending owner review of these findings and the runtime/build limitations.

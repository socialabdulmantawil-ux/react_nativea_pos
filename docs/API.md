# API

**Status:** Phase 1 verified the reported endpoints in the original `server/index.mjs`. They are implemented by a local Node.js HTTP server backed by SQLite, not Django Ninja, and are not an approved final mobile API contract.

| Reported endpoint | Reported purpose | Audit status |
|---|---|---|
| `GET /api/health` | Returns `{ "status": "ok" }` | Verified in original source |
| `GET /api/bootstrap` | Catalog, categories, size/add-on options, settings, and next order number | Verified in original source |
| `POST /api/orders` | Validates checkout data, recalculates totals, checks stock, writes an order transactionally | Verified in original source; client-supplied base price is not checked against the product record |
| `GET /api/orders?limit=50` | Recent orders; default limit 50, clamped to 1–200 | Verified in original source |

## Contract details

No authentication/authorization middleware or idempotency-key support was found in the inspected API. The request payload includes product IDs, quantities, size/adjustment, client base price, selected add-on IDs, notes, payment fields, and optional delivery details. The server validates multiple fields and responds with JSON errors, but it does not validate the supplied base price against the database's current product price.

The public Pages deployment returned 404 for `/api/bootstrap`; the API is only started by the local `npm run dev` script and is not included in the Pages deployment. Keep the final Django Ninja API contract as Not Yet Audited.

## Proposed migration API / PowerSync boundary

The target API contract is not approved or implemented. Phase 2 proposes:

- Manager-authenticated catalog, configuration, order-history, and operational endpoints.
- A Django Ninja endpoint that issues short-lived PowerSync JWT credentials, with business/user/device claims once the owner confirms the identity and tenancy model.
- A durable, idempotent domain-command path for offline-created sales and approved operational actions. The backend validates and commits these actions transactionally; only accepted server records are synchronized back to clients.
- A dedicated PowerSync connector/upload implementation if needed by the selected queue design. It must not blindly apply client CRUD to financial or inventory tables.

PowerSync's documented `uploadData()` connector retries failed uploads, but the backend must synchronously persist accepted mutations before the client acknowledges the batch. Final route names, command envelopes, state transitions, and error policy await the POC and owner approval. See `PHASE_2_VALIDATION.md`.

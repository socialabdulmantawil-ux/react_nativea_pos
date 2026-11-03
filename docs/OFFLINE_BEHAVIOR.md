# Offline Behavior

**Status:** Offline operation is a project requirement. Phase 1 found no implemented client-side offline queue or synchronization in the original web app. PowerSync + SQLite is approved (Decision 004), subject to Phase 2 validation.

## Intended capabilities to validate

- Browse locally synchronized products and categories.
- Search the local catalog and customize products.
- Build a cart and display provisional totals.
- Record only owner-approved offline operations.
- Preserve queued operations across restarts and retry after connectivity returns.
- Display local, queued, confirmed, failed, and payment states distinctly.

## Financial and synchronization constraints

- Backend calculations and validation are authoritative.
- Never mark an unconfirmed card payment successful offline.
- Use idempotency for retryable writes.
- Never silently discard queued operations.
- Define inventory conflict handling before enabling multi-device stock changes.

## Owner decisions required

- Which operations are blocked while offline.
- Device-prefixed temporary number, replaced by a server number after sync, selected by the owner (Decision 008). A client UUID remains the permanent key.
- Device-code format, temporary-number receipt wording, and exact sequence policy remain to define. Other options are retained in `PHASE_2_VALIDATION.md` for reference.
- Whether discounts, stock-sensitive products, and payment types require online approval.
- Decision 004 is approved; complete technical compatibility validation and proof of concept in Phase 2 before implementation.
- One business initially and required registered/approved devices are owner-approved (Decisions 009 and 010). Confirm manager login provider, device approval/revocation workflow, and allowed offline financial/inventory commands before implementation.

## Original app audit

- The original frontend loads catalog/settings from `/api/bootstrap` and submits orders to `/api/orders`; React state holds the active cart.
- The original API uses a local SQLite database file and single local order-number sequence. This is not a shared multi-device/offline-sync design.
- No local persistent browser queue, retry worker, synchronization status, idempotency key, or conflict-resolution mechanism was found.
- The public deployed site currently cannot load its catalog because its API endpoint returns 404.

## Verification

No offline behavior has been implemented or tested. The Phase 2 PowerSync/Expo/Postgres POC is not run because no test database/service or authorized EAS build environment is available; see `PHASE_2_VALIDATION.md`.

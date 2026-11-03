# Database

**Target authoritative database decision:** PostgreSQL (Neon initially), Approved as Decision 003. Neon supports logical replication, but production cost/reliability suitability and a PowerSync connection have not been tested.

## Original application — verified SQLite structures

The Phase 1 audit verified the owner-reported names in `server/schema.sql`. They describe the original local SQLite schema, not a proposed PostgreSQL schema. Relationships, constraints, and observed use:

| Reported table or structure | Audit status |
|---|---|
| categories | Verified in original source |
| products | Verified in original source |
| size_options | Verified in original source |
| add_ons | Verified in original source |
| category_add_ons | Verified in original source |
| product_default_add_ons | Verified in original source |
| settings | Verified in original source; seeded tax 15%, promo 10%, currency PHP |
| order_number_sequence | Verified in original source; single row seeded at 1048 |
| orders | Verified in original source |
| order_items | Verified in original source |
| order_item_add_ons | Verified in original source |
| payments | Verified in original source |
| delivery_orders | Verified in original source |

The source schema uses integer cents and SQLite constraints/foreign keys. It has no user, business/tenant, device, sync, refund, shift, or cash-drawer tables.

## Relationships and migrations

- PostgreSQL schema and migration history: Not Yet Audited.
- Do not propose or apply schema changes before reviewing the existing database and obtaining required approval.
- The backend must enforce financial calculations, valid state changes, ownership, and transactional integrity.

## Phase 2 conceptual design (proposed, not a schema)

The Phase 2 report proposes identity/business/device, catalog, orders/lines/financial snapshots, payments/refunds/voids, shifts/cash movements, and audit/idempotency data groups. The owner selected one business initially and required registered/approved devices. Use stable UUID entity/command identifiers and integer minor-unit amounts. No Django models or PostgreSQL migrations have been written.

PowerSync replication requires a Postgres publication and a privileged replication reader. Restrict its table grants/publication and Sync Streams to the minimum data needed. Enabling logical replication in Neon is irreversible per project and affects every compute; use a separate test project only after explicit authorization. See `PHASE_2_VALIDATION.md` for operational constraints and owner decisions.

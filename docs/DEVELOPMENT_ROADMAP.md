# Development Roadmap

This is the single source for phase details. Phase status reflects verified work only.

**Current phase:** Phase 2 — Architecture and Technology Validation  
**Current state:** Owner approved proceeding on 2026-11-03. Technology validation is underway; no application implementation has started.

| Phase | Name | Scope / milestone | Status | Gate or dependency |
|---:|---|---|---|---|
| 0 | Documentation and Project Preparation | Inspect workspace and original repository read-only; establish documentation and `.gitignore`; report and wait. | Completed | Owner approved Phase 0 on 2026-11-03. |
| 1 | Repository Audit | Inventory original UI, features, calculations, data structures, APIs, design, and migration risks; prepare parity checklist and reference screenshots. | Completed | Owner approved Phase 1 findings and proceeding to Phase 2 on 2026-11-03. Public deployment/build failures limit runtime and screenshot verification. |
| 2 | Architecture and Technology Validation | Validate technologies, architecture, offline numbering options, hardware models, risks, and PowerSync proof of concept. | In Progress | Research/report underway; POC blocked because external resources were deferred and the EAS session is unavailable. Owner selected numbering, one business initially, registered devices, and EAS cloud build; workflow details and final approval remain. |
| 3 | Project Foundation | Initialize Expo, TypeScript, navigation, tooling, environment configuration, Git, and development builds. | Not Started | Architecture approval. |
| 4 | Design System and Tablet Foundation | Establish visual tokens, reusable components, tablet layouts, orientations, and visual comparison. | Not Started | Audit references and approved architecture. |
| 5 | Backend Foundation | Establish Django, Django Ninja, PostgreSQL, initial models, schemas, validation, authentication foundations, and tests. | Not Started | Approved backend architecture. |
| 6 | Product Catalog | Implement and verify categories, products, images, pricing, options, search, and stock display. | Not Started | Backend and foundation. |
| 7 | Cart and Customization | Implement cart operations, options, customization, and provisional totals; compare with parity checklist. | Not Started | Catalog. |
| 8 | Discounts and Checkout | Implement approved discount and tax rules, checkout, order validation, and payment workflow with backend authority. | Not Started | Owner decisions and business-rule validation. |
| 9 | Orders and Receipts | Implement order creation, numbering, history, details, status, receipt preview, and approved receipt layout. | Not Started | Order-numbering and hardware decisions. |
| 10 | Inventory | Implement approved stock, adjustments, movements, low-stock indicators, and concurrency validation. | Not Started | Verified inventory rules. |
| 11 | PowerSync and Offline Operation | Implement local database, sync, queue, retries, idempotency, conflicts, and status. | Not Started | Phase 2 compatibility validation and proof of concept; revisit Decision 004 with owner if validation identifies a blocker. |
| 12 | Multi-Device Synchronization | Implement device registration, authorization, isolation, synchronization, and concurrent operation handling. | Not Started | Approved security and sync architecture. |
| 13 | Hardware Integration | Validate and integrate selected printers and scanners on supported devices. | Not Started | Exact hardware models and compatibility review. |
| 14 | Final Testing and Polish | Functional, UI, orientation, offline, sync, backend, security, performance, hardware, and build verification. | Not Started | Prior feature milestones. |

## Approval and scope notes

- Owner approved proceeding to Phase 2 on 2026-11-03; implementation remains gated on Phase 2 review/approval.
- The original repository and its public deployment were only inspected; do not modify the original.
- Decision 004 (PowerSync + SQLite) was approved by the owner on 2026-11-03. Phase 2 compatibility validation and proof of concept remain required before implementation.
- Owner confirmed manager-only roles and that refunds, voids, shifts, cash drawer, and end-of-day reporting are in scope. Detailed requirements remain to be defined.
- Printer/scanner models remain undecided. Phase 1 found no login/roles, business isolation, device registration, refunds, voids, shifts, cash-drawer, or end-of-day workflows in the original; confirm remaining new-scope details with the owner.
- Owner deferred Saudi requirements. Offline order numbering was initially deferred to Phase 2 and is now selected in Decision 008.
- Phase 2 official-doc/package-metadata review is documented in `PHASE_2_VALIDATION.md`. The required native PowerSync POC is not run; a dedicated test database/service and authorized EAS or local Android build environment are not available.
- Do not enable logical replication on an existing/shared Neon project for the POC; Neon documents the project setting as irreversible and potentially billable.
- Owner selected temporary device-prefixed order numbers replaced by server numbers, one business initially, required registered/approved devices, and EAS cloud build as the preferred native build route. External test resources were deferred; do not create them.
- Phase 1 findings must distinguish verified original behavior from owner-reported claims.
- New features and changes to existing business behavior require owner approval.

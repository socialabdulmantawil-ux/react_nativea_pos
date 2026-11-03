# Known Issues and Blockers

Phase 1 identified original-app defects and migration gaps; Phase 2 identified technology/POC blockers below. Runtime behavior remains unverified where noted in the audit report.

| ID | Description | Severity | Affected area | Status | Next action |
|---|---|---|---|---|---|
| B-001 | The project directory is not a Git repository, so branch and working-tree status are unavailable. | Information | Project management | Open | Confirm intended repository setup before source development. |
| B-002 | Public Pages deployment returns 404 for `/api/bootstrap`; no catalog data loads, although the UI reports “Online” and displays a sample cart. | High | Original deployment/reference | Confirmed 2026-11-03 | Owner may provide a working reference environment; do not modify original repo. |
| B-003 | Latest public build for inspected `main` commit failed with JSX parse errors in `App.tsx`; Pages deploy was skipped. | High | Original app build | Confirmed 2026-11-03 | Record as migration risk; original repo remains untouched. |
| B-004 | Original API accepts client-supplied product base price without checking the stored product price. | High — business correctness | Order pricing | Confirmed in source | Reproduce parity only after owner approves corrected authoritative pricing behavior. |
| B-005 | Visible “Hold Order” control has no action; no persisted hold/restore flow was found. | Medium | Cart/orders | Confirmed in source | Owner requested held-order operations be clarified in migration requirements. |
| B-006 | Card and wallet payments can be recorded as paid without processor confirmation/integration. | High | Payments | Confirmed in source | Define payment integration and truthful status transitions before implementation. |
| B-007 | Original app has no authentication/authorization, offline queue, or multi-device synchronization. | High | Security/offline/data sharing | Confirmed in inspected source | These are new migration scope; define manager access and validate approved sync architecture. |
| B-008 | Promo code text is stored but not validated; a boolean applies the configured discount. | Medium | Discounts | Confirmed in source | Record existing behavior and obtain approval before changing promotion rules. |
| B-009 | Reference screenshots for catalog/modal/success states could not be captured because deployed API bootstrap fails. | Blocker for complete visual parity evidence | Design audit | Open | Obtain a working owner-approved reference environment or other authorized access. |
| B-010 | PowerSync's native adapter is unsupported in Expo Go; the Expo/OP-SQLite SDK combination has not been built or synced. | High — Phase 2 gate | Native offline/sync | Open | Run the throwaway Expo Development Build POC after the owner authorizes a test environment and build route. |
| B-011 | Current PowerSync package peer range excludes the newest OP-SQLite 19.0.3 release; candidate 18.2.5 must be pinned and verified. | High — dependency compatibility | Native offline/sync | Confirmed in registry metadata 2026-11-03 | Test exact versions with Expo SDK 58 on Android and iOS before locking dependencies. |
| B-012 | Neon logical replication cannot be disabled after enabling at project level; a connected subscriber also prevents scale-to-zero. | High — cost and operational risk | PostgreSQL/PowerSync hosting | Confirmed in Neon documentation 2026-11-03 | Obtain explicit owner authorization and use a separate disposable project for POC. |
| B-013 | Expo native build and test PostgreSQL/PowerSync environments are unavailable; an Expo account/session and hardware were not supplied. | Blocker for Phase 2 POC | Validation environment | Open | Owner selected EAS cloud build but deferred external resources; resume after a secure EAS session and disposable test environment are available. |
| B-014 | Offline device-code/receipt policy, manager login/device workflows, and hardware models remain open. | Medium — owner decisions | Architecture/hardware | Open | Define remaining workflow details; record hardware deferral until models are selected. |

## Not Yet Audited

Runtime behavior for checkout and modal flows remains unverified due the public deployment failure. No tests were run against a local original installation.

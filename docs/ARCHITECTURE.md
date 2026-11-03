# Architecture

**Status:** Proposed architecture is documented in `PHASE_2_VALIDATION.md`; owner approval is outstanding. Phase 1 audited the original app, and Phase 2 compatibility research is underway. No product implementation is authorized.

## Project responsibilities

| Component | Intended responsibility | Decision status |
|---|---|---|
| React Native + Expo + TypeScript | Cross-platform tablet user interface | Approved (Decision 001); compatibility plausible, native POC not run |
| Django + Django Ninja | Validated API and authoritative business rules | Approved (Decision 002); package compatibility plausible, API POC not run |
| PostgreSQL (Neon initially) | Authoritative central database | Approved (Decision 003); logical replication supported, operational suitability remains unverified |
| Zustand | Temporary UI state only; not authoritative storage | Approved (Decision 005) |
| PowerSync + SQLite | Local-first data access and synchronization | Approved (Decision 004); SDK supports Expo on paper, adapter/build/sync POC not run |
| Expo Router | Native screen navigation | Proposed; SDK 58 compatible on paper |
| OP-SQLite | Native SQLite adapter for PowerSync | Proposed; candidate 18.2.5 fits SDK 2.4.0 peer range; POC required |
| NativeWind | Tailwind-style React Native styling | Proposed; SDK 58 compatibility not established; `StyleSheet` is the safer baseline |
| Lucide React Native / SecureStore | Icons / small credential storage | Proposed; Expo SDK-aligned packages available; build still required |

## Intended data authority

- Backend validation and PostgreSQL are intended to remain authoritative for business operations and shared records.
- Client totals are provisional; authoritative prices, discounts, tax, inventory, and payment state must be validated by the backend.
- PowerSync local SQLite is the local read model. Offline actions must remain visibly pending until a Django Ninja transaction accepts them.
- Decision 004 is owner-approved; implementation depends on completing Phase 2 validation and its POC.
- Proposed service boundaries, command flow, conceptual entities, numbering options, and environment blockers are in `PHASE_2_VALIDATION.md`.

## Original design and behavior

- Preserving the original design and workflows is an approved project objective (Decision 006).
- Original app: Vite + React + TypeScript frontend, Tailwind CSS, local Node HTTP API, and SQLite file.
- Current UI uses a dark, blue-accented two-pane POS layout; see the Phase 1 audit and the deployed-state reference screenshot.
- Original calculations, workflows, and source structure were inspected in Phase 1; runtime behavior remains unverified where the deployment API fails.
- Native design differences must be documented with their reasons after the audit.

## Original data flow (audited)

The browser requests `/api/bootstrap`; the local Node server reads product/options/settings from SQLite. The browser keeps cart and temporary order data in React state and submits checkout to `/api/orders`. The server validates and calculates order totals, writes order/payment/delivery rows, updates the local sequence, and decrements stock in one SQLite transaction. Its current base-price validation is incomplete; see `PHASE_1_AUDIT.md`.

The public Pages deployment only builds/deploys static Vite output and does not deploy the local API. The observed deployed site therefore returns 404 for bootstrap.

## Open architecture questions

- PowerSync and SQLite adapter POC (Expo SDK 58 + OP-SQLite 18.2.5 candidate).
- Manager authentication provider and device approval/revocation workflow. One business initially and registered/approved devices are owner-approved.
- Temporary device-code format and which financial/inventory operations are permitted offline.
- Hardware integration based on exact printer and scanner models.
- Final library/version approval and exact folder structure.
- Backend design for owner-requested manager-only access and operations (refunds, voids, shifts, cash drawer, end-of-day).

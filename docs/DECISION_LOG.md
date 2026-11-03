# DECISION LOG — BREW & BEAN

**Location:** `docs/DECISION_LOG.md`
**Rule:** Never change an approved decision silently. Never record a Proposed decision as Approved.
**Status values:** Proposed | Approved | Rejected | Superseded

Every decision uses this template:

- Decision ID
- Date
- Topic
- Status
- Problem
- Alternatives considered
- Selected solution
- Reason
- Consequences

Where the original record did not contain a field, it says "Not recorded". Nothing has been invented.

---

## Decision 001 — Mobile Framework
- **Date:** Not recorded (made before 2026-11-03)
- **Status:** Approved
- **Problem:** Not recorded
- **Alternatives considered:** Not recorded
- **Selected solution:** React Native + Expo + TypeScript
- **Reason:** Native tablet application for Android and iPadOS
- **Consequences:** Not recorded

## Decision 002 — Backend
- **Date:** Not recorded (made before 2026-11-03)
- **Status:** Approved
- **Problem:** Not recorded
- **Alternatives considered:** Not recorded
- **Selected solution:** Django + Django Ninja
- **Reason:** Centralized business logic, API validation, authentication, and database operations
- **Consequences:** Not recorded

## Decision 003 — Database
- **Date:** Not recorded (made before 2026-11-03)
- **Status:** Approved
- **Problem:** Not recorded
- **Alternatives considered:** Not recorded
- **Selected solution:** PostgreSQL, using Neon initially
- **Reason:** Centralized relational database with multi-device support
- **Consequences:** Neon suitability for production and for PowerSync replication is still to be validated in Phase 2

## Decision 004 — Offline Synchronization
- **Date:** 2026-11-03
- **Status:** **Approved by owner**
- **Problem:** Tablets must keep working without internet and sync later
- **Alternatives considered:** Not recorded (to be compared in Phase 2)
- **Selected solution:** PowerSync + SQLite
- **Reason:** Local-first data access and synchronization between authorized devices
- **Consequences:** Requires an Expo Development Build (not Expo Go), a compatible SQLite adapter, and Postgres replication setup. Phase 2 must still validate compatibility and complete the planned proof of concept before offline implementation begins. If validation identifies a blocker, revisit this decision with the owner.

## Decision 005 — State Management
- **Date:** Not recorded (made before 2026-11-03)
- **Status:** Approved
- **Problem:** Not recorded
- **Alternatives considered:** Not recorded
- **Selected solution:** Zustand
- **Scope:** Temporary interface state, active cart, modal state, and other transient UI interactions
- **Restriction:** Not the authoritative database
- **Consequences:** Persistent data lives in the local database and the backend, never only in Zustand

## Decision 006 — UI Preservation
- **Date:** Not recorded (made before 2026-11-03)
- **Status:** Approved
- **Problem:** Not recorded
- **Alternatives considered:** Not recorded
- **Selected solution:** Preserve the original Brew & Bean visual design and existing functionality
- **Restriction:** No unapproved redesigns
- **Consequences:** Reference screenshots of the original will be captured in Phase 1 for later comparison

## Decision 007 — Project Architecture Style
- **Date:** Not recorded (listed as approved in the original PROJECT_STATUS)
- **Status:** Approved
- **Problem:** Not recorded
- **Alternatives considered:** Not recorded
- **Selected solution:** Feature-based project architecture
- **Reason:** Not recorded
- **Consequences:** Exact folder structure is finalized after the Phase 1 audit. Folders are created only when their phase starts.

## Decision 008 — Offline Order Display Number
- **Date:** 2026-11-03
- **Status:** Approved by owner
- **Problem:** Offline sales on multiple tablets must not receive duplicate order numbers.
- **Alternatives considered:** Device-prefixed temporary number replaced by a server number after sync; pre-reserved per-device number ranges; permanent UUID/ULID-derived display code.
- **Selected solution:** Assign a device-prefixed temporary display number and replace it with the server-assigned number after sync; retain the client-generated UUID as the permanent order key.
- **Reason:** Owner selected the recommended option A.
- **Consequences:** Requires registered unique device codes and UI/receipt states that distinguish temporary from confirmed numbers. Detailed prefix, sequence, and customer communication rules remain to define.

## Decision 009 — Initial Business Scope
- **Date:** 2026-11-03
- **Status:** Approved by owner
- **Problem:** Sync authorization and data ownership depend on whether the first release serves one or multiple businesses.
- **Alternatives considered:** One business initially; multiple businesses from initial release; defer business count while designing for isolation.
- **Selected solution:** One business for the initial release.
- **Reason:** Owner selected the single-business option.
- **Consequences:** Do not build multi-business user flows in the initial release. Keep data ownership explicit so future expansion can be reviewed; do not silently treat the first deployment as an unrestricted shared database.

## Decision 010 — Device Enrollment
- **Date:** 2026-11-03
- **Status:** Approved by owner
- **Problem:** Devices must be controlled before they receive synchronized business data or upload offline commands.
- **Alternatives considered:** Require registered/approved devices; manager login without device enrollment; defer enrollment.
- **Selected solution:** Require registered/approved devices in the initial release.
- **Reason:** Owner selected device enrollment as required.
- **Consequences:** Device registration, approval/revocation, and recovery workflows are new scope and must be designed and tested. Details remain open.

---

## Stack items named in AGENT.md but not yet individually decided

These are listed as part of the required stack. They have no logged decision yet. They are treated as **Proposed until confirmed in Phase 2**.

| Item | Status |
|---|---|
| Expo Router | Proposed |
| NativeWind (only if compatible) | Proposed |
| Lucide React Native | Proposed |
| Expo SecureStore | Proposed |
| EAS Build / Expo Development Builds | Proposed |
| OP-SQLite or other SQLite adapter | Proposed (to be evaluated) |
| Thermal printer / scanner library | Not yet chosen (depends on actual hardware model) |

---

## Open questions for the owner (answers become decisions)

1. Define the device-code format, temporary-number receipt wording, and server sequence policy for Decision 008.
2. Which exact printer and scanner models will be used?
3. Define manager login provider and registered-device approval/revocation workflows for the single-business initial scope.
4. Saudi requirements: deferred for now; revisit whether ZATCA e-invoicing, 15% VAT handling, SAR currency, and Arabic/RTL support are required.

## Owner scope answers — 2026-11-03

- **Roles:** Manager only; no cashier role requested.
- **Refunds, voids, shifts, cash drawer, and end-of-day reporting:** In scope. Exact workflows and implementation phase remain to be defined after the audit.
- **Printer and scanner models:** Not yet decided.
- **New features compared with the original:** Owner was unsure; Phase 1 comparison is recorded below.
- **Phase 1 comparison:** Manager-only access, business ownership, and device registration are new relative to the inspected original. In Phase 2, the owner selected one business initially and required registered/approved devices; login provider and enrollment workflow remain to define.
- **Saudi requirements:** Owner chose to defer. No technical or compliance requirement is recorded as approved.
- **Offline order numbering:** Owner chose to defer the strategy decision to Phase 2.
- **Phase 1:** Owner approved the read-only repository audit on 2026-11-03.
- **Phase 2:** Owner approved proceeding to Architecture and Technology Validation on 2026-11-03. This does not approve Phase 3 or product implementation.
- **Phase 2 validation:** Official documentation and package metadata reviewed on 2026-11-03; findings and architecture proposal are in `PHASE_2_VALIDATION.md`. The required native PowerSync/PostgreSQL POC has not run. No new stack item is approved by this research.
- **Phase 2 owner answers:** Selected Decision 008 (temporary device-prefixed number, then server number), one business for the initial release (Decision 009), and required registered/approved devices (Decision 010). Deferred external POC resources; selected EAS cloud build as the preferred route, but no Expo account/session was supplied.

---

## Future decisions

Add new decisions above this line using the template. Keep history. Mark replaced decisions "Superseded" instead of deleting them.

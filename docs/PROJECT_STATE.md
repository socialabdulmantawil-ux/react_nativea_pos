# Project State

**Last updated:** 2026-11-03  
**Evidence boundary:** Phase 1 source audit is documented; Phase 2 has reviewed public compatibility sources. Original runtime behavior remains partly unverified because the public deployment API is unavailable and the latest build failed. No native POC has run.

## Current workspace

| Area | Verified state |
|---|---|
| Project directory | Contains project instructions and status documents; no application source or dependency manifest was present during Phase 0 inspection. |
| Git | Not a Git repository; branch and working-tree status are unavailable. |
| Documentation | Phase 0 and Phase 1 accepted; Phase 2 validation report drafted, POC blocked. |
| Original application | Phase 1 source, schema, API, and public deployment audited read-only; see PHASE_1_AUDIT.md. |
| Dependencies installed | No dependencies installed during this phase. |

## Technical state

| Area | Status | Notes |
|---|---|---|
| Frontend | Original audited; migration not started | Original is Vite/React/TypeScript. Public build currently fails; see audit. No mobile app source exists here. |
| Backend | Original audited; migration not started | Original uses local Node HTTP API; target Django Ninja is not implemented. |
| Database | Original SQLite schema verified; target PostgreSQL Not Yet Audited | See DATABASE.md. |
| API | Original endpoints verified; target API Not Yet Audited | Public Pages `/api/bootstrap` returns 404. |
| PowerSync | Approved, not implemented | Decision 004 was approved by the owner on 2026-11-03; package compatibility reviewed, but native Expo/Postgres proof of concept remains required and blocked. |
| Offline operation | Design proposed; not implemented/tested | Order-numbering choice is recorded; allowed offline operations and the technical POC remain unresolved. |
| Authentication | Not present in original; new target scope | Manager-only access and registered/approved devices are required; login provider and enrollment workflow remain to define. |
| Hardware | Not Yet Audited | Exact models are unknown; compatibility has not been tested. |
| POS features | Not Yet Audited | No feature is claimed implemented or verified. |

## Confirmed project requirements

- Target: React Native tablet application for Android and iPadOS.
- Approved stack decisions are recorded in DECISION_LOG.md.
- PowerSync + SQLite is approved; technical validation remains outstanding.
- Decision 008 selects device-prefixed temporary order numbers replaced by server numbers; Decision 009 selects one business initially; Decision 010 requires registered/approved devices.
- EAS cloud build is the preferred POC route; external test-resource creation was deferred by the owner.
- Original repository: `https://github.com/socialabdulmantawil-ux/brew-bean`; accessed read-only for a public root listing and package.json.
- No source changes have been made to the original repository.

## Blockers and next step

- Phase 1 source audit is documented; Phase 2 is owner-approved and underway. See `PHASE_2_VALIDATION.md` for research, architecture proposal, and POC blockers.
- Saudi requirements are deferred; printer/scanner models remain undecided. Offline display numbering is Decision 008.
- Git status could not be inspected because this directory has no `.git` repository.

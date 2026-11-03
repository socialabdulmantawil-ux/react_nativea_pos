# Brew & Bean Mobile POS

Native tablet POS migration project for Android and iPadOS. The objective is to preserve the original Brew & Bean application's verified design and business workflows while preparing an offline-capable mobile application and backend.

## Current status

Phase 0 and the Phase 1 source audit are complete. Phase 2 architecture and technology validation is underway; see [the Phase 2 validation report](docs/PHASE_2_VALIDATION.md). The required native PowerSync/PostgreSQL proof of concept is blocked because external test resources were deferred and EAS account access is not available. No application code or dependencies have been added.

## Project documentation

- [Project status](./PROJECT_STATUS.md)
- [Development rules](./DEVELOPMENT_RULES.md)
- [Project state](./docs/PROJECT_STATE.md)
- [Development roadmap](./docs/DEVELOPMENT_ROADMAP.md)
- [Feature checklist](./docs/FEATURE_CHECKLIST.md)
- [Decision log](./docs/DECISION_LOG.md)
- [Installation](./docs/INSTALLATION.md)
- [Phase 1 audit report](./docs/PHASE_1_AUDIT.md)
- [Phase 2 validation report](./docs/PHASE_2_VALIDATION.md)

## Setup

Developer setup instructions are **Not Yet Audited** because no application or backend has been initialized. See [docs/INSTALLATION.md](./docs/INSTALLATION.md) for the current setup status.

## Important

PowerSync + SQLite is **Approved**, subject to Phase 2 compatibility validation and proof of concept. The original repository remains untouched. The public deployment currently returns 404 for its local API, and the latest main-branch build failed; see the audit report.

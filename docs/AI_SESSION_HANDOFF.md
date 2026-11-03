# AI SESSION HANDOFF — BREW & BEAN

**Location:** `docs/AI_SESSION_HANDOFF.md`
This document transfers development context between AI coding sessions.
If a section had no changes this session, write "No change" instead of leaving it blank.

## Last Working Session

- Date: 2026-11-03
- AI coding agent: Copilot SDK in VS Code
- Current development phase: Phase 2 — Architecture and Technology Validation
- Last completed milestone: Phase 1 read-only source audit and documentation (owner approved transition)
- Current milestone: Obtain owner decisions/resources and run the blocked PowerSync proof of concept

## Work Completed

- Owner answered several open scope questions on 2026-11-03: PowerSync + SQLite approved; manager-only role; refunds, voids, shifts, cash drawer, and end-of-day reporting in scope.
- Recorded those answers. In Phase 2, the owner selected device-prefixed temporary order numbers with server replacement, one business initially, registered/approved devices, and EAS cloud build. Printer/scanner models remain undecided. The original has no login/roles, business/device controls, or requested operational workflows; manager login and enrollment details remain to define.
- Owner approved proceeding to Phase 2 on 2026-11-03. Phase 1 audit is documented; runtime/deployment limitations remain explicit.
- Owner deferred Saudi requirements. Offline order numbering was initially deferred to Phase 2 and the owner then selected Decision 008.
- Audited original repo at main commit `82dc22193597a198c7e10769ba27203a7ffee7d4`. Latest Pages build failed; deployed API bootstrap returns 404.
- Captured only the unavailable-state default page in `docs/reference/deployed-pos-unavailable.png`. Functional catalog/modal screenshots remain blocked.
- Phase 2 research and architecture draft are in `docs/PHASE_2_VALIDATION.md`; native PowerSync/Expo/Postgres POC has not run.
- Reviewed current Expo, PowerSync, Neon, Django, NativeWind, and package registry compatibility references. Identified OP-SQLite peer-version constraint and Neon replication costs/irreversibility.
- Checked POC prerequisites: no Java/Android SDK/ADB, `psql`, Docker, EAS CLI/session, test database, or PowerSync service available.

## Files Changed

- Created the root README.md, CHANGELOG.md, .env.example, and .gitignore.
- Updated PROJECT_STATUS.md, AGENT.md, and DEVELOPMENT_RULES.md to reflect the owner's approval of Decision 004.
- Created the required documentation files under docs/, including this handoff and the decision log (relocated from the project root).
- Updated docs/DECISION_LOG.md, PROJECT_STATE.md, DEVELOPMENT_ROADMAP.md, OFFLINE_BEHAVIOR.md, POWERSYNC.md, FEATURE_CHECKLIST.md, DATABASE.md, API.md, ARCHITECTURE.md, HARDWARE_COMPATIBILITY.md, TESTING_LOG.md, KNOWN_ISSUES.md, PHASE_1_AUDIT.md, PHASE_2_VALIDATION.md, README.md, CHANGELOG.md, and this handoff.
- Preserved INITIAL_INSTRUCTION.md and the original Brew & Bean repository unchanged.

## Tests Performed

No local build, app tests, or PowerSync POC executed. Package registry metadata and official documentation were reviewed read-only. Environment prerequisite findings and unrun POC are recorded in `docs/PHASE_2_VALIDATION.md`.

## Unresolved Problems

- Git branch and working-tree status cannot be determined because the project directory has no `.git` repository.
- GitHub CLI authentication was unavailable; public repository pages were used for read-only inspection.
- The original source/schema/API were inspected at main commit `82dc22193597a198c7e10769ba27203a7ffee7d4`; runtime order flows remain unverified due deployment/API failure.
- Phase 2 POC requires owner-authorized disposable Neon/PowerSync resources and an EAS or native build route. Neon logical replication cannot be disabled once enabled on a project.

## Important Discoveries

- The original repository is a Vite/React/TypeScript POS with a local Node/SQLite API; detailed findings are in `docs/PHASE_1_AUDIT.md`.
- PowerSync React Native is Expo-supported on paper but requires a Development Build. `@powersync/react-native` 2.4.0 excludes latest OP-SQLite 19.x; 18.2.5 is the current in-range candidate, not yet tested.
- Neon supports logical replication but an active subscriber prevents scale-to-zero and inactive slots expire after about 40 hours.
- The public Pages deployment fails API bootstrap, and the latest main build failed JSX parsing; no changes were made to the original.
- No application source files or dependency manifests are present in this local project directory.

## Decisions Requiring Approval

- Revisit Saudi requirements when needed; owner deferred the question.
- Identify printer and scanner models.
- Define the manager login provider and registered-device approval/revocation workflows. One initial business and required device enrollment are approved.
- External POC resources were deferred; resume only when a secure Expo/EAS session and approved disposable Postgres/PowerSync resources are available.

## Exact Next Task

Continue Phase 2 architecture/compatibility validation. Do not start Phase 3 or application implementation without owner approval.

## Relevant Files to Inspect

- AGENT.md
- DEVELOPMENT_RULES.md
- PROJECT_STATUS.md
- docs/PROJECT_STATE.md
- docs/DEVELOPMENT_ROADMAP.md
- docs/FEATURE_CHECKLIST.md
- docs/PHASE_2_VALIDATION.md
- docs/DECISION_LOG.md
- docs/KNOWN_ISSUES.md
- docs/AI_SESSION_HANDOFF.md

## Instructions for the Next Session

1. Read the required project documentation and verify the current project state.
2. Check Git status; this directory was not a Git repository during Phase 0.
3. Use an owner-authorized disposable test environment to run the POC; do not provision external resources or enable Neon replication without permission.
4. Keep any further inspection of the original repository read-only.
5. Do not begin Phase 3 or product implementation before the Phase 2 report/POC approval gate.

## Last session summary

Phase 0 and Phase 1 were approved. Owner has approved Phase 2. The original build/API limitations remain documented; Phase 2 validation is underway.

## Next recommended action

Wait until authorized disposable resources and Expo/EAS account access are available. Then run the throwaway Expo Development Build + PowerSync SQLite adapter + test Postgres POC only within that environment, update the report, and stop for Phase 2 approval. Do not modify the original repository.

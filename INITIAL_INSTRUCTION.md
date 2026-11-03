# INITIAL INSTRUCTION — Phase 0: Documentation and Project Preparation

Read AGENT.md and DEVELOPMENT_RULES.md completely before doing anything.

Your task is **Phase 0 only**: documentation and project preparation. Do not start Phase 1 (Repository Audit), Phase 2, or any implementation until I review and approve your Phase 0 report.

## Tasks

1. Inspect the current project directory and Git status.
2. Inspect the original Brew & Bean repository **read-only**. Do not clone it into this project, edit it, commit to it, or push to it.
3. Convert the uploaded `.txt` planning files to `.md`. Keep the original `.txt` files unchanged.
4. Place files exactly as AGENT.md specifies:
   - Root: AGENT.md, DEVELOPMENT_RULES.md, README.md, PROJECT_STATUS.md, CHANGELOG.md, .env.example
   - docs/: every file listed in AGENT.md section 12, including DECISION_LOG.md and AI_SESSION_HANDOFF.md
5. Create all missing documentation files with headings, tables, and checklists.
6. Preserve existing files. Inspect any existing file before changing it.
7. Populate only verified information.
8. Mark unknown information as "Not Yet Audited".
9. Mark every unapproved technical choice as "Proposed". PowerSync + SQLite is Proposed (Decision 004).
10. Mark the database tables and API endpoints listed in AGENT.md section 9 as "Reported by owner, Not Yet Audited".
11. Create a `.gitignore` suitable for Expo, Node, and Django. It must exclude `.env` and other secrets.
12. Do not generate frontend or backend code. Do not implement POS features. Do not install dependencies.

## Questions to put in your report (do not answer them yourself)

- Is PowerSync + SQLite approved or rejected?
- What roles and permissions are needed (cashier, manager)?
- How should offline order numbers avoid duplicates across devices?
- Are refunds, voids, shifts, cash drawer, and end-of-day in scope?
- Are ZATCA e-invoicing, 15% VAT handling, SAR currency, or Arabic/RTL support required?
- Which printer and scanner models will be used?
- Which features are new compared with the original app?

## Report back with

- The actual folder structure created.
- The documentation files created.
- Existing files preserved.
- Assumptions and unresolved questions.
- Confirmation that the original repository was not modified.

Then stop and wait for my approval.

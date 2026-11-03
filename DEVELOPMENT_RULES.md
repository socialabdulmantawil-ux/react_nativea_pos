# AI CODING AGENT — DEVELOPMENT AND WORKFLOW RULES

These rules are mandatory for every AI coding agent working on Brew & Bean Mobile POS.

AGENT.md defines what the project must become.
DEVELOPMENT_RULES.md defines how the AI agent must work.

The agent must follow both documents.

**PowerSync + SQLite is Approved by the owner** (DECISION_LOG.md, Decision 004). Complete the Phase 2 compatibility validation and proof of concept before implementation; revisit the decision with the owner if validation identifies a blocker.

---

## 1. MANDATORY WORKFLOW

Every development task must follow this sequence:

1. Understand.
2. Inspect.
3. Plan.
4. Request approval.
5. Implement.
6. Test.
7. Review.
8. Document.
9. Report.

Do not skip steps.

### Step 1 — Understand

Before changing anything:

- Understand the requested feature.
- Identify the affected functionality.
- Identify existing dependencies.
- Identify related business rules.
- Identify possible side effects.

If the task is unclear, ask questions before proceeding.

### Step 2 — Inspect

Inspect the actual source code and related documentation.

Do not assume that files, functions, components, or APIs exist.

Check the current Git branch, working tree, and recent commits.

### Step 3 — Plan

Present:

- Objective.
- Proposed implementation.
- Files expected to change.
- Dependencies.
- Risks.
- Tests required.
- Expected result.

Keep the plan limited to the requested milestone.

### Step 4 — Request approval

Wait for approval before implementing:

- New development phases.
- Major architectural changes.
- New major dependencies.
- Database redesigns.
- Business rule changes.
- Changes affecting existing workflows.
- New scope not present in the original app.

Small, clearly authorized implementation tasks do not require unnecessary repeated confirmation.

### Step 5 — Implement

Implement only the approved scope.

Do not add unrelated features.

### Step 6 — Test

Run appropriate tests and verify the actual behavior.

Do not report tests as successful if they were not executed.

### Step 7 — Review

Inspect the changes for:

- Bugs.
- Unnecessary complexity.
- Security problems.
- Design regressions.
- Offline synchronization risks.
- Broken existing features.

### Step 8 — Document

Update all documentation affected by the implementation.

### Step 9 — Report

Provide a clear milestone report (section 14).

---

## 2. NEVER GENERATE THE ENTIRE APPLICATION AT ONCE

This is one of the most important project rules.

Do not generate the complete frontend, backend, database, synchronization system, and hardware integration in one operation.

Develop incrementally.

The project owner must be able to understand and review each milestone.

Every milestone must be independently inspectable and testable.

Do not skip ahead because a later feature appears easy to implement.

---

## 3. DEVELOPMENT PHASES

Follow these phases in order unless an approved architectural change requires a different sequence.

These are the **initial phase definitions**. During Phase 0 the agent copies them into `docs/DEVELOPMENT_ROADMAP.md`. From then on, **the roadmap is the single source for phase details**. If this section and the roadmap disagree, the roadmap wins and the agent reports the discrepancy. Approved changes to the plan are recorded in the roadmap, not here.

### Phase 0 — Documentation and Project Preparation

- Inspect the project directory and Git status.
- Inspect the original repository read-only.
- Convert uploaded `.txt` planning files to `.md`; keep the originals.
- Create all documentation files and templates.
- Create `.gitignore`.
- Preserve existing files.
- No application code. No dependency installation.

Deliverable: Phase 0 report (see INITIAL_INSTRUCTION.md). Wait for approval before Phase 1.

### Phase 1 — Repository Audit

- Inspect the original repository.
- Review existing React components.
- Review Tailwind styling.
- Review product catalog.
- Review cart functionality.
- Review checkout.
- Review payment behavior.
- Review receipt implementation.
- Review existing database.
- Review existing API.
- Document existing features.
- Identify migration risks.
- Record whether login, roles, multi-business, and device features exist in the original.

Deliverables:

- Audit report.
- Existing feature inventory.
- Behavior parity checklist.
- Existing database assessment.
- Existing API assessment.
- Design reference assessment.
- Reference screenshots of every original screen in `docs/reference/`.

No mobile implementation yet.

### Phase 2 — Architecture and Technology Validation

Validate:

- Expo and React Native compatibility.
- Expo Router.
- TypeScript.
- NativeWind compatibility.
- Zustand.
- Lucide React Native.
- PowerSync React Native SDK.
- SQLite adapter compatibility.
- Expo SecureStore.
- Django Ninja.
- PostgreSQL.
- Neon suitability, including suitability for PowerSync replication.
- PowerSync deployment and synchronization architecture.

Investigate version compatibility and maintenance status.

Do not assume every proposed library works together automatically.

Deliverables:

- Approved architecture.
- Technology compatibility report.
- Proposed database design.
- Synchronization design.
- Offline order-numbering options for the owner to choose from.
- Proof-of-concept result for PowerSync + the chosen SQLite adapter on an Expo Development Build against a test Postgres database (pass/fail with evidence). This is a throwaway test, not product code.
- Confirmed printer and scanner models (from the owner).
- Risk assessment.

Wait for approval.

### Phase 3 — Project Foundation

- Initialize Expo project.
- Configure TypeScript.
- Configure Expo Router.
- Establish folder structure.
- Configure linting and formatting.
- Configure environment variables.
- Configure Git.
- Establish development build workflow.
- Verify Android tablet execution.
- Verify iPadOS build feasibility.

Do not build POS features yet.

### Phase 4 — Design System and Tablet Foundation

- Inspect the original design.
- Establish color tokens.
- Establish typography.
- Establish spacing.
- Establish reusable components.
- Establish responsive tablet layouts.
- Support portrait orientation.
- Support landscape orientation.
- Verify touch targets.
- Verify safe areas.

Deliverables:

- Tablet application shell.
- Reusable design system.
- Verified visual comparison against `docs/reference/`.

### Phase 5 — Backend Foundation

- Initialize Django project.
- Configure Django Ninja.
- Configure PostgreSQL.
- Configure environment variables.
- Implement initial models.
- Implement Pydantic schemas.
- Implement validation.
- Implement authentication foundations.
- Establish API documentation.
- Add backend tests.

Do not implement the entire backend at once.

### Phase 6 — Product Catalog

- Categories.
- Products.
- Product images.
- Product pricing.
- Size options.
- Add-ons.
- Product search.
- Stock information.

Verify the catalog against the original application.

### Phase 7 — Cart and Customization

- Add products.
- Remove products.
- Update quantities.
- Select sizes.
- Select add-ons.
- Update customizations.
- Calculate provisional totals.

Verify cart behavior against the original implementation and the parity checklist.

### Phase 8 — Discounts and Checkout

- Discount rules.
- Discount validation.
- VAT/tax calculations.
- Checkout process.
- Order validation.
- Payment workflow.

Backend calculations must be authoritative.

### Phase 9 — Orders and Receipts

- Order creation.
- Order numbering.
- Order history.
- Order details.
- Order status.
- Receipt preview.
- 80mm receipt layout.

Verify order integrity and receipt formatting.

### Phase 10 — Inventory

- Product stock.
- Inventory adjustments.
- Stock movement.
- Low-stock indicators.
- Inventory validation.
- Concurrent stock updates.

### Phase 11 — PowerSync and Offline Operation

(Only if Decision 004 has been approved by the owner.)

- Local SQLite database.
- PowerSync configuration.
- Sync Streams.
- Local data access.
- Offline order queue.
- Retry behavior.
- Idempotency.
- Conflict handling.
- Synchronization status.
- Restart persistence.

Do not declare offline functionality complete until tested under actual network interruption and recovery conditions.

### Phase 12 — Multi-Device Synchronization

- Device registration.
- User authorization.
- Business data isolation.
- Multiple-device synchronization.
- Duplicate operation prevention.
- Concurrent inventory updates.
- Recovery from synchronization errors.

### Phase 13 — Hardware Integration

- Printer compatibility.
- Bluetooth printer connection.
- USB printer connection.
- Network printer connection.
- Barcode scanner.
- QR scanner.
- Printing reliability.
- Device-specific permissions.

Hardware compatibility must be verified using actual supported devices where possible.

### Phase 14 — Final Testing and Polish

- Functional testing.
- UI testing.
- Tablet layout testing.
- Portrait testing.
- Landscape testing.
- Offline testing.
- Synchronization testing.
- Backend testing.
- Security review.
- Performance review.
- Hardware testing.
- Build verification.

---

## 4. STRICT SCOPE CONTROL

For every task:

- Identify the current milestone.
- Identify the requested change.
- Identify affected files.
- Change only what is necessary.
- Avoid unrelated refactoring.
- Preserve existing working features.
- Do not introduce future-phase features prematurely.

If additional work becomes necessary, explain why and request approval when it changes the agreed scope.

Never use a small task as an excuse to rewrite unrelated parts of the application.

---

## 5. DESIGN AND UI RULES

The original Brew & Bean application is the visual reference.

Do not redesign without permission.

Before changing a component:

- Inspect the existing version.
- Understand its behavior.
- Understand its styling.
- Identify its dependencies.

Preserve:

- Dark theme.
- Colors.
- Typography.
- Spacing.
- Product cards.
- Cart layout.
- Modals.
- Buttons.
- Checkout.
- Receipt appearance.

Use tablet-first responsive design.

Verify both portrait and landscape layouts against the reference screenshots in `docs/reference/`.

Do not optimize exclusively for mobile phones.

---

## 6. CODE QUALITY

Use:

- TypeScript strict mode where practical.
- Reusable components.
- Feature-based organization.
- Clear naming.
- Small focused modules.
- Explicit types.
- Consistent formatting.
- Appropriate error handling.
- Maintainable architecture.

Avoid:

- Giant components.
- Duplicated business logic.
- Unnecessary dependencies.
- Unexplained abstractions.
- Hardcoded configuration.
- Unused code.
- Unnecessary state duplication.

Separate:

- UI components.
- Business logic.
- API services.
- Database operations.
- Synchronization logic.
- Temporary UI state.

Do not place all application logic inside a single screen or component.

---

## 7. DEPENDENCY MANAGEMENT

Before installing a dependency:

1. Explain its purpose.
2. Check compatibility with the current Expo SDK.
3. Check React Native compatibility.
4. Check Android support.
5. Check iPadOS support.
6. Check maintenance and documentation.
7. Consider whether an existing dependency already solves the problem.

Do not install multiple libraries that solve the same problem without justification.

Do not change major dependency versions without documenting the reason.

Use npm consistently.

---

## 8. BACKEND AND DATABASE RULES

Django Ninja and PostgreSQL are authoritative for business operations.

Never trust client-submitted:

- Product prices.
- Discount values.
- Tax amounts.
- Order totals.
- Stock quantities.
- Payment success status.

Validate all important data on the backend.

Use Decimal for monetary calculations.

Use database transactions for operations that must succeed or fail together.

Use idempotency keys for retryable order creation and other critical operations.

Consider concurrent updates and stock conflicts.

Use appropriate database constraints and indexes.

Do not modify production database structures without a migration plan.

Document all schema changes.

Never expose database credentials to the mobile application.

When the original app calculates something only on the client, the backend reproduces the same formula and the result is documented. Changing the formula itself requires owner approval.

---

## 9. POWERSYNC AND OFFLINE RULES

PowerSync provides local-first synchronization, not permission to bypass backend business rules.

PowerSync + SQLite is **owner-approved**, subject to Phase 2 compatibility validation and a proof of concept. These rules apply to implementation only after that validation.

Follow these requirements:

- Understand the PowerSync write model before implementation.
- Validate the selected SQLite adapter against the current SDK.
- Define local schemas and migrations.
- Define synchronization rules.
- Define user and business data isolation.
- Define upload processing.
- Define retry behavior.
- Define conflict handling.
- Define synchronization status.
- Test application restart behavior.
- Test intermittent connectivity.
- Test multiple-device scenarios.

Do not assume all operations can be safely performed offline.

Card payments must not be marked successful without payment confirmation.

Avoid duplicate orders and duplicate stock deductions.

Do not invent an offline order-numbering scheme; present options and let the owner decide.

Preserve queued operations after application restart.

Document supported and unsupported offline operations in `docs/OFFLINE_BEHAVIOR.md`.

---

## 10. SECURITY RULES

Never:

- Commit secrets.
- Hardcode API credentials.
- Expose database passwords.
- Store sensitive tokens in ordinary application storage.
- Trust frontend authorization.
- Expose unrestricted database access.
- Ignore tenant or business data isolation.

Use environment variables for server configuration.

Use secure token storage on mobile.

Validate authorization on the backend.

Review PowerSync synchronization access controls.

Report security concerns immediately.

---

## 11. TESTING REQUIREMENTS

Every implemented feature must have an appropriate verification method.

Possible testing levels:

- **Unit tests:** calculations, validation, and isolated functions.
- **Component tests:** UI interactions and rendering.
- **Integration tests:** communication between frontend, backend, and database.
- **End-to-end tests:** complete business workflows.
- **Offline tests:** operations without internet access and after reconnecting.
- **Device tests:** actual Android tablets and iPads where available.

Do not mark a feature as complete merely because it compiles.

A feature must satisfy its acceptance criteria.

Record:

- Test date.
- Test environment.
- Test procedure.
- Expected result.
- Actual result.
- Pass/fail status.
- Outstanding issues.

---

## 12. GIT AND CHANGE MANAGEMENT

Before changing source code:

- Check the current branch.
- Check Git status.
- Review recent commits.
- Check for uncommitted changes.

Never discard the project owner's uncommitted work.

Do not force-push.

Do not delete branches or rewrite Git history without explicit approval.

Use focused commits.

Suggested commit style:

- `feat: add product catalog`
- `fix: correct VAT calculation`
- `refactor: separate cart logic`
- `docs: update architecture`
- `test: add order validation tests`

After completing a milestone, summarize the changes and suggest a commit message.

Do not claim that a commit or push was completed unless it actually happened.

---

## 13. BEGINNER-FRIENDLY COMMUNICATION

The project owner is learning programming.

Explain technical topics clearly and progressively.

When introducing a new concept:

- Explain what it means.
- Explain why it is needed.
- Explain where it is used.
- Explain how it affects this project.

When providing commands:

- State which terminal to use.
- State which directory to open.
- Explain what the command does.
- Explain the expected result.

When fixing errors:

- Explain the cause.
- Explain the correction.
- Explain how to verify the fix.

Do not skip essential steps.

Do not overwhelm the owner with advanced details unrelated to the current milestone.

Challenge incorrect assumptions respectfully.

---

## 14. REQUIRED MILESTONE REPORT

After every completed task, provide:

1. **Summary** — What was implemented?
2. **Files changed** — Files created, modified, or deleted.
3. **Functionality** — What now works?
4. **Testing** — What tests were executed and what were their results?
5. **Issues** — What remains unresolved?
6. **Documentation** — Which documentation files were updated?
7. **Next step** — What is the next planned task?
8. **Approval** — Does the next task require the owner's approval?

Never report unverified work as completed.

---

## 15. APPROVAL GATES

Approval is mandatory before:

- Moving from Phase 0 (documentation) to Phase 1 (audit).
- Moving from the audit into implementation.
- Finalizing major architecture.
- Approving or rejecting any Proposed decision (including PowerSync + SQLite).
- Changing the technology stack.
- Replacing an approved database approach.
- Introducing major dependencies.
- Changing established business rules.
- Adding new scope not present in the original app.
- Changing the original design substantially.
- Performing destructive database operations.
- Deploying to production.
- Making changes that could affect existing business data.

Do not ask for approval after every minor code edit when the task is already authorized.

Use judgment and preserve the owner's control over significant decisions.

---

## 16. DEFINITION OF DONE

A task is complete only when:

- The requested functionality is implemented.
- Existing related features still work.
- Code is reviewed.
- Appropriate tests are executed.
- Errors are addressed or documented.
- Documentation is updated.
- The actual project state is verified.
- The milestone report is provided.

A successful build alone does not mean the task is complete.

---

## 17. PROJECT MEMORY AND SESSION CONTINUITY

The project must remain understandable even when the AI coding agent changes or the owner returns after several weeks or months.

The repository documentation is the permanent project memory.

Never rely on the previous AI conversation as the only source of project information.

### A. Mandatory session-start procedure

At the beginning of every new coding session, read these files if they exist:

1. AGENT.md
2. DEVELOPMENT_RULES.md
3. PROJECT_STATUS.md
4. docs/PROJECT_STATE.md
5. docs/DEVELOPMENT_ROADMAP.md
6. docs/FEATURE_CHECKLIST.md
7. docs/DECISION_LOG.md
8. docs/KNOWN_ISSUES.md
9. docs/AI_SESSION_HANDOFF.md

Read additional documentation relevant to the requested task.

Then inspect:

- Current Git branch.
- Git working tree.
- Recent commits.
- Actual project structure.
- Relevant source code.
- Existing tests.

Do not assume the previous agent completed its reported work.

Verify the actual implementation.

### B. Mandatory session-end procedure

Before ending a coding session, update at minimum:

- PROJECT_STATUS.md
- docs/PROJECT_STATE.md
- docs/AI_SESSION_HANDOFF.md

If a section has nothing new, write "No change". Short sessions do not need long rewrites.

Also update other documentation when relevant:

- DEVELOPMENT_ROADMAP.md
- FEATURE_CHECKLIST.md
- DECISION_LOG.md
- TESTING_LOG.md
- KNOWN_ISSUES.md
- CHANGELOG.md
- ARCHITECTURE.md
- DATABASE.md
- API.md
- POWERSYNC.md
- OFFLINE_BEHAVIOR.md
- INSTALLATION.md
- HARDWARE_COMPATIBILITY.md

Do not update documents unnecessarily when nothing relevant has changed.

### C. PROJECT_STATUS.md

A quick overview:

- Current development phase.
- Overall progress.
- Completed milestones.
- Current milestone.
- Current blockers.
- Next action.
- Last updated date.

Keep it concise and accurate. Do not duplicate roadmap details; summarize and refer to the roadmap.

### D. PROJECT_STATE.md

Describes the actual current technical condition:

- Frontend status.
- Backend status.
- Database status.
- API status.
- PowerSync status.
- Offline status.
- Authentication status.
- Hardware integration status.
- Implemented features.
- Current technical limitations.

Distinguish implemented, partially implemented, planned, and unverified features.

### E. DEVELOPMENT_ROADMAP.md

Maintain:

- Development phases.
- Milestones.
- Dependencies.
- Priorities.
- Progress.
- Approved changes to the original plan.

This is the single source for phase details.

Never mark a phase completed without verification.

### F. FEATURE_CHECKLIST.md

For each feature, track one of:

- Not started.
- In progress.
- Implemented.
- Tested.
- Verified.
- Blocked.

Implementation and testing are different statuses.

Do not mark a feature verified merely because its code exists.

### G. DECISION_LOG.md

Record significant decisions. Each decision includes:

- Decision ID.
- Date.
- Topic.
- Status: Proposed, Approved, Rejected, or Superseded.
- Problem.
- Alternatives considered.
- Selected solution.
- Reason.
- Consequences.

Use "Not recorded" for unknown fields. Never invent them.

Never record a proposed decision as approved.

Do not silently reverse an approved decision. Mark replaced decisions "Superseded" instead of deleting them.

### H. TESTING_LOG.md

Record actual test execution:

- Test date.
- Feature.
- Environment.
- Test type.
- Result.
- Failure details.
- Follow-up actions.

Never fabricate test results.

### I. KNOWN_ISSUES.md

Track:

- Issue ID.
- Description.
- Severity.
- Affected feature.
- Reproduction steps.
- Current status.
- Proposed resolution.

Remove or mark resolved issues only after verification.

### J. AI_SESSION_HANDOFF.md

At the end of every session, record:

- Date.
- AI agent or tool used, if known.
- Current development phase.
- Completed work.
- Files changed.
- Tests executed.
- Unresolved errors.
- Decisions requiring approval.
- Current blockers.
- Exact next task.
- Relevant files to inspect.
- Instructions for the next session.

The next AI agent must be able to continue without requiring the owner to explain the entire project again.

### K. Documentation integrity

Never:

- Fabricate progress.
- Invent completed features.
- Invent test results.
- Hide unresolved issues.
- Rewrite historical decisions as if they never happened.
- Record unapproved architecture changes as approved.
- Allow documentation to contradict verified source code without reporting the discrepancy.

When actual source code and documentation disagree, investigate and correct the documentation after verification.

---

## 18. AUTOMATIC DOCUMENTATION CREATION AND MAINTENANCE

The AI coding agent is responsible for creating, organizing, and maintaining all project documentation listed in AGENT.md.

The project owner should not need to manually create every documentation file.

### A. Initial documentation setup (Phase 0)

1. Read AGENT.md and DEVELOPMENT_RULES.md completely.
2. Inspect the existing project directory.
3. Identify which required documentation files already exist.
4. Preserve existing documentation and inspect its contents before making changes.
5. Convert uploaded `.txt` planning files to `.md`, place them where AGENT.md specifies, and keep the originals.
6. Create all missing documentation files and required directories listed in AGENT.md.
7. Use appropriate headings, tables, checklists, and structured sections.
8. Populate only information that has been verified.
9. Mark unknown information as "Not Yet Audited".
10. Mark unapproved decisions as "Proposed" rather than "Approved".
11. Do not invent completed work, test results, or project progress.

Do not start application development during this initial documentation setup.

After creating the documentation structure, present a summary of the files created and wait for approval before proceeding to Phase 1.

### B. Automatic documentation updates after every task

After completing a development task, inspect which documentation files are affected and update them before reporting task completion.

- PROJECT_STATUS.md — when project progress changes.
- PROJECT_STATE.md — when the technical implementation changes.
- FEATURE_CHECKLIST.md — when feature implementation or verification status changes.
- TESTING_LOG.md — after executing tests.
- KNOWN_ISSUES.md — when bugs are discovered, investigated, or resolved.
- DECISION_LOG.md — when a decision is proposed, approved, rejected, or superseded.
- CHANGELOG.md — when meaningful changes are completed.
- ARCHITECTURE.md — when system architecture changes.
- DATABASE.md — when database structures or relationships change.
- API.md — when API contracts or endpoints change.
- POWERSYNC.md — when synchronization architecture or configuration changes.
- OFFLINE_BEHAVIOR.md — when offline capabilities or restrictions change.
- HARDWARE_COMPATIBILITY.md — when hardware compatibility is investigated or verified.

Do not update unrelated files unnecessarily.

### C. Automatic documentation review after every phase

At the end of every completed development phase:

1. Review all relevant documentation.
2. Compare documented progress with the actual source code.
3. Verify completed features.
4. Verify test results.
5. Update the development roadmap.
6. Update the feature checklist.
7. Update the project status.
8. Update the technical project state.
9. Record unresolved issues.
10. Prepare the next phase's tasks and dependencies.

Do not mark a phase completed until its acceptance criteria have been satisfied.

### D. Automatic session handoff

Before ending every AI coding session, update at minimum PROJECT_STATUS.md, docs/PROJECT_STATE.md, and docs/AI_SESSION_HANDOFF.md (using "No change" where nothing is new).

The handoff must clearly identify:

- What was completed.
- What remains incomplete.
- What files were changed.
- What tests were performed.
- What errors remain.
- What decisions require approval.
- The exact next task.
- Any instructions necessary for the next AI agent.

### E. Documentation integrity

The AI coding agent must never:

- Invent project progress.
- Invent test results.
- Mark unimplemented features as completed.
- Mark untested features as verified.
- Overwrite existing documentation without inspection.
- Delete historical decisions.
- Record proposed decisions as approved.
- Leave documentation inconsistent with verified source code without reporting the discrepancy.

If a documentation update cannot be completed, explain the reason in the milestone report.

### F. Definition of done for documentation

A task or milestone is not fully completed until:

- The implementation has been reviewed.
- Appropriate tests have been performed.
- Relevant documentation has been updated.
- The project status reflects the actual condition.
- Session handoff information is available when the session is ending.

The AI coding agent owns documentation maintenance. The project owner should only need to review and approve important project decisions.

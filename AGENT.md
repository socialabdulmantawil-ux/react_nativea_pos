# BREW & BEAN — REACT NATIVE TABLET POS

---

## CRITICAL RULES (read first)

1. This is a **migration, not a redesign**. Preserve the original design, features, and workflows.
2. **Never modify the original repository.**
3. **Never generate the whole app at once.** Work one approved milestone at a time.
4. **Never continue past an approval gate** without the owner's approval.
5. **Never trust the frontend** for prices, discounts, tax, totals, stock, or payment status. The backend decides.
6. **Never mark a feature complete** until it is implemented and tested.
7. **Never record a Proposed decision as Approved.**
8. **Never put secrets or credentials** in the mobile app or in Git.
9. **Never invent progress or test results.** Mark unknown things "Not Yet Audited".
10. At the start and end of every session, follow the procedure in DEVELOPMENT_RULES.md section 17.

---

## 1. PROJECT IDENTITY

- **Project name:** Brew & Bean Mobile POS
- **Project type:** Native cross-platform tablet Point of Sale (POS) application.
- **Primary platforms:** Android tablets, iPadOS tablets.
- **Original repository:** https://github.com/socialabdulmantawil-ux/brew-bean

The original project is a Vite + React web application. This project is a separate React Native migration.

The original repository must remain untouched.

The purpose is to migrate the existing application into a genuine React Native application while preserving its existing appearance, functionality, business workflows, and user experience as closely as the native platforms allow.

This is a migration, not a redesign or an opportunity to replace the existing application with a different POS system.

---

## 2. PRIMARY OBJECTIVES

The application must:

1. Preserve the existing Brew & Bean visual identity.
2. Preserve existing POS features and business workflows.
3. Work on Android and iPadOS tablets.
4. Support portrait and landscape orientations.
5. Operate offline for supported POS operations.
6. Synchronize data across multiple devices.
7. Use Django Ninja as the backend API.
8. Use PostgreSQL as the authoritative central database.
9. Use PowerSync and local SQLite for offline-first data access (**Approved by owner; technical validation remains required — see DECISION_LOG.md, Decision 004**).
10. Support future thermal printer and barcode/QR scanner integration.
11. Be maintainable, testable, and suitable for real business operations.

Do not sacrifice business correctness for visual appearance or development speed.

### What "preserve" means (parity rule)

"Preserve" must be checkable, not a feeling.

- **Behavior parity:** In Phase 1, the agent writes a parity checklist of every original feature and calculation (cart, discounts, VAT, order numbering, receipts). Each mobile feature is verified against its checklist entry.
- **Visual parity:** In Phase 1, the agent captures reference screenshots of every original screen and stores them in `docs/reference/`. Later screens are compared against them.
- **When the original contradicts the backend-authority rule** (for example, the original calculates totals only in the browser): the backend reproduces the same formula, the result is documented, and the app shows a provisional total until the server confirms. Any change to the formula itself needs owner approval.

---

## 3. ORIGINAL PROJECT PRESERVATION

Before implementing anything:

- Inspect the original repository.
- Understand its folder structure and dependencies.
- Inspect the existing React components.
- Inspect the existing database schema.
- Inspect the API endpoints.
- Inspect the product catalog and existing data.
- Inspect all existing POS workflows.
- Identify existing calculations and business rules.
- Document the current visual design.

Do not delete, overwrite, or modify the original repository.

Create and maintain the mobile application in a separate project directory or repository.

Do not assume that the original implementation is fully correct. Identify bugs, security issues, and architectural limitations during the audit, but do not silently change existing business behavior.

Any proposed behavior changes must be documented and approved.

---

## 4. DESIGN PRESERVATION

The existing Brew & Bean design is the visual reference.

Preserve the following:

- Dark theme and existing color palette.
- Typography and font hierarchy.
- Spacing and proportions.
- Product card design.
- Product images and image presentation.
- Category navigation.
- Search interface.
- Cart layout.
- Order summary.
- Buttons and interactive states.
- Modals and dialogs.
- Checkout interface.
- Payment interface.
- Receipt appearance.

The tablet interface must not become a generic mobile application.

Do not redesign the application into a phone-style layout.

Maintain the existing tablet-oriented POS experience.

Native adjustments are permitted only when necessary for:

- Touch interaction.
- Screen dimensions.
- Safe areas.
- Native navigation.
- Keyboard behavior.
- Native platform limitations.
- Accessibility.

Document every unavoidable design difference in `docs/ARCHITECTURE.md` (or a dedicated design-differences section), with the reason.

---

## 5. TECHNOLOGY STACK

**Approval status lives in `docs/DECISION_LOG.md`. That file wins if this section disagrees.**

### Approved

- React Native, Expo, TypeScript
- Django, Django Ninja
- PostgreSQL (Neon initially)
- Zustand (temporary UI state only)
- npm as the package manager

### Proposed (not yet approved; confirmed or rejected in Phase 2)

Frontend:

- Expo Router
- NativeWind, if compatible with the selected Expo and React Native versions
- React Native StyleSheet when NativeWind is unsuitable
- Lucide React Native
- Expo SecureStore
- PowerSync React Native SDK
- SQLite adapter supported by the selected PowerSync SDK, with OP-SQLite evaluated for compatibility
- Expo Development Builds
- EAS Build

Backend:

- Python
- Pydantic schemas
- Django ORM
- Neon PostgreSQL, subject to deployment suitability
- PowerSync synchronization service
- PowerSync Sync Streams where appropriate

Do not introduce alternative libraries without explaining their purpose. Obtain approval when the change affects an approved architectural decision.

Django Ninja owns authoritative business logic and validated writes.

### Architecture responsibilities

**Frontend**

- User interface.
- Temporary interface state.
- Local database reads.
- Supported offline operations.
- User interactions.
- Displaying synchronization status.

**Zustand**

- Active cart state.
- Modal state.
- Selected product options.
- Temporary interface state.
- Other transient UI state.
- Zustand is never the authoritative database.

**PowerSync and SQLite**

- Local-first data access.
- Supported offline reads and writes.
- Persistent local records.
- Synchronization queue.
- Data synchronization.

**Django Ninja**

- Authentication and authorization integration.
- Input validation.
- Business rules.
- Authoritative calculations.
- Order processing.
- Inventory updates.
- Payment validation.
- Data integrity.

**PostgreSQL**

- Authoritative central business database.
- Persistent shared records.
- Transactional business operations.

---

## 6. REQUIRED POS FEATURES

The migration must preserve the existing features where they are present in the original project. The Phase 1 audit confirms which of these actually exist.

**Product catalog**

- Product listing.
- Product categories.
- Product search.
- Product images.
- Product prices.
- Product availability.
- Stock indicators.

**Product customization**

- Size options.
- Add-ons.
- Default add-ons.
- Product-specific options.
- Customization pricing.

**Cart and orders**

- Add products.
- Remove products.
- Change quantities.
- Edit product customization.
- Calculate subtotal.
- Apply discounts.
- Calculate VAT/tax.
- Calculate total.
- Hold orders.
- Restore held orders.
- Cancel orders.

**Checkout and payments**

- Checkout process.
- Payment selection.
- Cash payment.
- Card payment.
- Wallet payment, if supported by the existing implementation.
- Payment status.
- Transaction records.
- Validation before completing an order.

**Receipts**

- Receipt preview.
- Receipt printing.
- 80mm thermal receipt layout.
- Order information.
- Item information.
- Payment details.
- Tax details.
- Business information.

**Order management**

- Order history.
- Order details.
- Order status.
- Search and filtering.

**Inventory**

- Product stock.
- Inventory updates.
- Stock movement records where required.
- Low-stock indicators.
- Inventory synchronization.

Do not mark any feature as complete until its implementation and tests have been verified.

Features not in this list (refunds, voids, shifts, cash drawer, end-of-day reports, customer records) are **not in scope until the owner decides** (see section 17).

---

## 7. OFFLINE-FIRST REQUIREMENTS

Offline support is a core architectural requirement.

The application should support:

- Viewing locally synchronized products.
- Searching the local catalog.
- Browsing categories.
- Adding products to a cart.
- Customizing products.
- Calculating provisional totals.
- Creating supported offline orders.
- Recording eligible cash sales locally.
- Persisting queued operations across application restarts.
- Retrying synchronization when connectivity returns.
- Showing synchronization status.
- Handling synchronization failures.

### Important financial rules

- Never trust frontend calculations as authoritative.
- Prices, discounts, VAT, totals, stock quantities, and payment statuses must be validated by the backend.
- Use Decimal-based monetary calculations.
- Use database transactions for business-critical writes.
- Use idempotency keys to prevent duplicate orders and duplicate operations.
- Use concurrency controls to prevent incorrect stock deductions.
- Do not mark an unconfirmed card payment as successful while offline.
- Never silently discard queued offline operations.
- Define and test conflict handling before enabling multi-device inventory and order operations.

### Clearly distinguish these states

- Locally recorded.
- Waiting for synchronization.
- Server confirmed.
- Synchronization failed.
- Payment pending.
- Payment confirmed.
- Payment failed.

### Offline order numbers (owner decision required)

The original project has an `order_number_sequence`. Several tablets creating orders offline can generate duplicate numbers. The rule for this is **not yet decided**. The agent must not invent one. Options to present in Phase 2 include a temporary local number plus a final server-assigned number, or a per-device prefix. The owner chooses, and the choice is recorded in DECISION_LOG.md.

### Offline restrictions (owner decision required)

Which operations are blocked offline (for example card payments, manager-approved discounts, stock-sensitive items) is decided by the owner and documented in `docs/OFFLINE_BEHAVIOR.md`.

---

## 8. SECURITY AND MULTI-DEVICE REQUIREMENTS

The application must support multiple authorized devices sharing business records.

Requirements:

- Secure authentication.
- Secure token storage.
- Business-level data isolation.
- User-level permissions.
- Device identification.
- Authorized synchronization.
- Server-side validation.
- Protected API endpoints.
- Safe credential handling.

Never place secret keys, database credentials, or private server credentials in the mobile application.

Never expose unrestricted database access to the frontend.

PowerSync synchronization rules must ensure users only receive records they are authorized to access.

Do not assume that frontend restrictions provide security.

### New requirements (not in the original app)

The Phase 1 audit found no login, user roles, business-level isolation, or device registration in the original project. Treat these as **new scope**; implementation needs owner approval. Manager-only access is owner-requested; business isolation, authentication details, and device enrollment remain unresolved.

---

## 9. DATABASE AND BUSINESS LOGIC

The owner reports that the original project includes the tables and endpoints below.

**Status: Verified in the original source during Phase 1.** These tables/endpoints describe the original Node/SQLite app, not the target PostgreSQL/Django schema/API.

Tables or data structures:

- categories
- products
- size_options
- add_ons
- category_add_ons
- product_default_add_ons
- settings
- order_number_sequence
- orders
- order_items
- order_item_add_ons
- payments
- delivery_orders

Endpoints:

- GET /api/health
- GET /api/bootstrap
- POST /api/orders
- GET /api/orders?limit=50

Treat these as audit findings, not automatically as the final backend API design.

Review and map the existing database before proposing changes.

Do not discard existing data structures without justification.

The backend must enforce:

- Valid product prices.
- Valid quantities.
- Valid discounts.
- Valid tax calculations.
- Valid payment transitions.
- Valid inventory changes.
- Order integrity.
- Business ownership.
- Transaction consistency.

Do not duplicate authoritative business logic in the frontend.

---

## 10. HARDWARE INTEGRATION

Future hardware requirements include:

- Bluetooth thermal printers.
- USB thermal printers.
- Network thermal printers.
- Barcode scanners.
- QR scanners.

Do not assume that a printer or scanner will work simply because it works with the web application.

Investigate:

- Native React Native compatibility.
- Expo Development Build requirements.
- Android and iPadOS support.
- Bluetooth and USB permissions.
- Printer SDK availability.
- ESC/POS compatibility.
- Connection reliability.

Do not finalize a hardware library before checking the actual hardware model.

**Record the exact printer and scanner models during Phase 2**, not Phase 13. Paper width and command set affect the receipt design in Phase 9.

Document compatibility findings in `docs/HARDWARE_COMPATIBILITY.md`.

---

## 11. PROJECT STRUCTURE

Use a feature-based, maintainable structure (approved as Decision 007; exact folders finalized after the audit).

Suggested structure:

```
frontend/
  app/
  src/
    components/
      ui/
      layout/
      feedback/
    features/
      auth/
      products/
      categories/
      cart/
      orders/
      checkout/
      payments/
      inventory/
      customers/
      receipts/
    database/
      powersync/
      schema/
      migrations/
    services/
      api/
      auth/
      sync/
    stores/
    hooks/
    constants/
    theme/
    types/
    utils/
    config/
  assets/
    images/
    fonts/
    icons/
  tests/

backend/
  config/
  apps/
    accounts/
    businesses/
    products/
    categories/
    inventory/
    orders/
    payments/
    customers/
    devices/
    settings/
    sync/
  tests/

docs/
  reference/        (original-app screenshots, created in Phase 1)
```

This is a proposed structure. Finalize it after the repository audit and architecture review. Do not create every folder or module before its implementation phase.

---

## 12. DOCUMENTATION SYSTEM

The documentation files are part of the project and must be maintained.

### Root documentation

| File | Purpose |
|---|---|
| AGENT.md | Project identity, objectives, architecture requirements, and non-negotiable rules. |
| DEVELOPMENT_RULES.md | Development workflow, approval gates, coding standards, testing, and documentation maintenance. |
| README.md | Project overview, setup instructions, prerequisites, and developer onboarding. |
| PROJECT_STATUS.md | Quick overview of current project phase and progress. |
| CHANGELOG.md | Record of meaningful completed changes. |
| .env.example | Required environment variable names and safe placeholders only. Never real secrets. |
| .gitignore | Excludes `.env`, secrets, build output, and dependency folders. |

### docs/ documentation

| File | Purpose |
|---|---|
| PROJECT_STATE.md | Actual current technical state of the project. |
| DEVELOPMENT_ROADMAP.md | Development phases, priorities, dependencies, and milestones. The single source for phase details. |
| FEATURE_CHECKLIST.md | Feature implementation and verification status. |
| ARCHITECTURE.md | System architecture, responsibilities, data flow, and design differences. |
| DECISION_LOG.md | Decisions with status (Proposed, Approved, Rejected, Superseded) and reasoning. |
| DATABASE.md | Database structure, relationships, and migration notes. |
| API.md | Backend endpoints, request/response contracts, and validation behavior. |
| POWERSYNC.md | PowerSync configuration, synchronization architecture, and implementation notes. |
| OFFLINE_BEHAVIOR.md | Offline capabilities, restrictions, retry behavior, and conflict handling. |
| TESTING_LOG.md | Tests performed, results, and outstanding failures. |
| KNOWN_ISSUES.md | Known defects, limitations, and unresolved technical problems. |
| INSTALLATION.md | Detailed installation and environment setup. |
| HARDWARE_COMPATIBILITY.md | Verified printer and scanner compatibility. |
| AI_SESSION_HANDOFF.md | Instructions for resuming development after an AI session ends. |

---

## 13. SOURCE OF TRUTH

The AI agent must use the following hierarchy:

1. Actual source code and verified test results determine what is implemented.
2. AGENT.md defines project objectives and non-negotiable requirements.
3. DEVELOPMENT_RULES.md defines development procedures.
4. DECISION_LOG.md records technical decisions and their approval status. It outranks status files for any technical choice.
5. PROJECT_STATUS.md provides the current progress overview.
6. PROJECT_STATE.md records the current technical state.
7. DEVELOPMENT_ROADMAP.md defines planned development and is the single source for phase details.
8. AI_SESSION_HANDOFF.md records the next task and session continuity.

If documentation conflicts with actual code, investigate and report the discrepancy.

Never pretend that a documented feature is implemented when it is not.

Never silently convert a proposed decision into an approved decision.

---

## 14. BEGINNER-FRIENDLY DEVELOPMENT

The project owner is learning programming and application development.

Explain technical decisions in beginner-friendly language.

When implementing a feature:

- Explain what it does.
- Explain why it is needed.
- Identify the files being changed.
- Explain important dependencies.
- Explain how to run it (which terminal, which folder, which command).
- Explain how to test it.
- Explain any errors in understandable language.

Do not overwhelm the project owner with unnecessary technical complexity.

Do not hide important architectural risks.

Challenge incorrect assumptions respectfully and provide evidence-based alternatives.

---

## 15. NON-NEGOTIABLE RESTRICTIONS

The AI agent must never:

- Modify the original repository.
- Redesign the POS without approval.
- Generate the entire application in one response or one uncontrolled operation.
- Skip the repository audit.
- Skip architecture validation.
- Introduce major dependencies without justification.
- Change the approved stack silently.
- Hardcode credentials or secrets.
- Put authoritative business calculations only in the frontend.
- Mark untested features as complete.
- Ignore offline synchronization risks.
- Ignore data security.
- Delete existing business functionality without approval.
- Change database architecture without documenting the consequences.
- Add new scope (features not in the original app) without approval.
- Continue beyond an approval gate without authorization.

---

## 16. REQUIRED INITIAL ACTION

When starting this project, do not immediately write application code.

**Phase 0 — Documentation and Project Preparation** comes first (see INITIAL_INSTRUCTION.md). Then, after the owner approves Phase 0:

1. Read the available project documentation.
2. Inspect the original GitHub repository (read-only).
3. Audit the existing application.
4. Identify existing functionality.
5. Identify existing database structures and APIs.
6. Review design and tablet layouts.
7. Capture reference screenshots of every original screen into `docs/reference/`.
8. Write the behavior parity checklist (section 2).
9. Identify migration risks.
10. Validate technology compatibility.
11. Run a small throwaway proof of concept for PowerSync + the selected SQLite adapter on an Expo Development Build against test Postgres before Phase 3. Decision 004 is already owner-approved; if the POC identifies a blocker, revisit it with the owner.
12. Propose the mobile architecture.
13. Prepare a phased implementation plan.

Present findings and wait for approval before implementation.

---

## 17. SCOPE QUESTIONS (owner must answer before the related phase)

The agent must not guess these. Record each answer in DECISION_LOG.md.

- Decision 004 (PowerSync + SQLite) is approved. Revisit only if Phase 2 validation identifies a blocker.
- Manager-only role is owner-approved; define manager permissions for discounts, voids, refunds, and operational workflows.
- Offline order numbering: Decision 008 selects device-prefixed temporary numbers replaced by server numbers; define the device-code/receipt details.
- Which operations are blocked offline.
- Refunds, voids, shifts, cash drawer, and end-of-day reports are in scope; define detailed behavior and phase placement.
- Saudi requirements: ZATCA e-invoicing (QR code on receipts), 15% VAT handling, SAR currency, Arabic/RTL support. Confirm with an accountant.
- Exact printer and scanner models.
- Confirm manager login details and registered-device approval/revocation workflows; the owner selected one business initially and required device enrollment.

The owner may defer choosing hardware models. Record that deferral and do not select a model or hardware library on the owner's behalf.

---

## 18. FINAL PROJECT GOAL

Deliver a stable, secure, offline-first native tablet POS application that preserves the original Brew & Bean experience while providing a maintainable React Native architecture, Django Ninja backend, PostgreSQL database, and reliable multi-device synchronization.

**Preserve first. Audit second. Plan third. Implement incrementally. Test continuously. Document everything.**

# Phase 2 — Architecture and Technology Validation

**Status:** In progress; Expo/PowerSync/PostgreSQL proof of concept blocked on environment and owner approvals  
**Research date:** 2026-11-03  
**Scope:** Public vendor documentation and package metadata only. No project dependencies were installed, no external database/service was provisioned, and no product code was written.

## Executive summary

The approved React Native + Expo and Django + Django Ninja + PostgreSQL direction is technically plausible. PowerSync has a maintained React Native/Expo SDK, supports Android and iOS, documents an Expo Development Build workflow, and has official React Native and Django examples. The native SQLite adapter is OP-SQLite.

This is not yet a passed compatibility test. The current PowerSync SDK peer range is `@op-engineering/op-sqlite >=17.1.0 <19.0.0`, while the newest published OP-SQLite is 19.0.3. A compatible 18.2.5 release exists, but an Expo SDK 58 Android/iOS build and actual sync were not run. Expo Go is not supported with the native adapter.

Neon supports PostgreSQL logical replication, but enabling it is an irreversible project-wide setting that restarts computes. A connected subscriber prevents scale-to-zero; inactive replication slots are removed after about 40 hours. A dedicated disposable Neon project and PowerSync service are required for a safe POC. The owner deferred external test-resource setup. EAS cloud build is the preferred route, but no Expo account/session was supplied. No local Android toolchain, device bridge, PostgreSQL client/server, or Docker is available.

**Decision 004 remains owner-approved, but its required technical validation and POC are incomplete. Do not start Phase 3 or application implementation.**

## Technology compatibility findings

Package versions below are a snapshot of the npm/PyPI registries and official documentation on the research date, not a lockfile or final dependency selection.

| Technology | Evidence and compatibility | Phase 2 recommendation / status |
|---|---|---|
| React Native + Expo + TypeScript | Expo SDK 58 was released with React Native 0.88. Expo's SDK 58 docs include Router, SQLite, SecureStore, and native Android/iOS support. PowerSync documents React Native support from RN 0.72 and Expo support from Expo 49; its platform matrix lists Android SDK 24+ and iOS 15+. | **Compatible on paper.** POC must compile with the selected Expo SDK and both native targets. SDK 58 is a research baseline, not a final approved version. |
| Expo Router | Expo SDK 58 recommends Expo Router `~58.0.29`; SDK docs describe file-based Android/iOS navigation. | **Compatible on paper; proposed.** Appropriate for a feature-based multi-screen POS; avoid web-only routing assumptions. |
| NativeWind | NativeWind 4.2.7 is the stable v4 release and uses Tailwind CSS 3. Its installation guide says v4.2.7 adds Expo SDK 57 support; the guide does not claim SDK 58 support. It also requires Babel/Metro and Reanimated-related setup. | **SDK 58 compatibility unverified.** Prefer React Native `StyleSheet` for the first native baseline unless a targeted compatibility/style-parity test succeeds. NativeWind remains proposed, not rejected or approved. |
| Zustand | Published 5.0.16 requires React 18+ as a peer. | **Compatible on paper.** Keep state transient as already decided; never make it the durable order store. |
| Lucide React Native | Published 1.66.0 supports React 16.5–19 and declares `react-native-svg` 12–15 as a peer. Expo recommends installing native peers through `npx expo install` to match its SDK. | **Compatible on paper.** Confirm the Expo-aligned SVG version in the POC. |
| Expo SecureStore | Expo SDK 58 recommends `expo-secure-store ~58.0.2`, supported on Android and iOS. It encrypts small key/value secrets; native config changes require a rebuild. | **Compatible on paper.** Limit it to refresh/access credentials and similarly small secrets, not catalog/order storage. |
| PowerSync React Native SDK | Published `@powersync/react-native` 2.4.0 supports Android/iOS via React Native and is documented for Expo. The SDK requires a native build with `@op-engineering/op-sqlite`; Expo Go's sandbox is explicitly unsupported for this adapter. | **Compatible on paper; POC required.** Use a Development Build. SDK and adapter peer range must be pinned together. |
| OP-SQLite adapter | PowerSync 2.4.0 declares OP-SQLite `>=17.1.0 <19.0.0`. Registry lookup found 18.2.5 in-range and 19.0.3 latest, which is out-of-range. Expo installation must not silently select 19.x. | **Version coupling is a release risk.** POC candidate: OP-SQLite 18.2.5 with PowerSync 2.4.0; rebuild and test on Android and iOS. |
| Expo Development Build / EAS | Expo documents local native builds (Android Studio or Xcode) and EAS cloud builds (Expo account). Cloud builds can avoid a local Android/iOS native toolchain, but require the account and credentials. | **Required for native PowerSync.** Local build unavailable here; cloud build may be possible after owner supplies/authorizes an account and test resources. |
| Django + Django Ninja | Django Ninja 1.7.3 package metadata requires Django >=3.1 and Pydantic >=2,<3. Django 5.2 is an LTS release and supports Python 3.14 as of 5.2.8. Python 3.14.7 is installed locally. | **Compatible on paper.** Django 5.2 LTS + a pinned compatible Ninja/Pydantic set is a conservative candidate; verify migrations, auth, and transactional API behavior in the backend POC. |
| PostgreSQL / Neon | PowerSync supports PostgreSQL 11+ as a source. Neon documents logical replication support with project-level configuration and standard PostgreSQL publication/replication setup. | **Technically plausible; operational suitability not approved.** Neon-specific costs, slot behavior, recovery, connection mode, and deployment tier need a disposable POC and review. |
| npm / Python tooling | `node v26.5.0`, `npm 11.17.0`, and `Python 3.14.7` are present. Package versions were inspected from registries without installing them. | **No project manifests exist.** Do not treat installed global Python packages as this application's dependency set. |

### Package snapshot

| Package | Registry version observed |
|---|---:|
| `expo` | 58.0.22 |
| `expo-router` | 58.0.29 |
| `expo-secure-store` | 58.0.2 |
| `@powersync/react-native` | 2.4.0 |
| `@op-engineering/op-sqlite` | 19.0.3 latest; 18.2.5 in PowerSync peer range |
| `nativewind` | 4.2.7 |
| `zustand` | 5.0.16 |
| `lucide-react-native` | 1.66.0 |
| `django-ninja` | 1.7.3 |
| `Django` | 6.1.2 latest; 5.2.18 current LTS patch observed |
| `pydantic` | 2.14.2 |

Pin exact tested versions and commit the relevant lockfiles only after Phase 2 approval and the POC.

## Proposed architecture — for owner review

1. **Tablet app:** Expo + React Native + TypeScript, feature-based structure, Expo Router, `StyleSheet` baseline, Zustand for transient UI, Lucide icons, SecureStore for credentials.
2. **Local data:** PowerSync-managed SQLite backed by OP-SQLite. Read the local catalog and confirmed business records from SQLite. Store unconfirmed local work and display its pending state distinctly.
3. **Authentication:** Django Ninja authenticates the sole requested role (manager), issues short-lived access credentials and a PowerSync JWT. Store secrets only in SecureStore. One business and required registered/approved devices are owner-approved; manager identity provider and device approval/revocation details remain open.
4. **Read/sync path:** PostgreSQL is authoritative. PowerSync reads a narrowly scoped Postgres publication, applies Sync Streams, and sends only authorized rows to each device. Use authenticated business/user claims in streams; do not treat UI filtering as access control. Audit the service replication role and database grants separately.
5. **Write path:** Do not expose unrestricted CRUD or trust client-calculated financial values. Use durable, idempotent domain commands for orders, refunds, voids, shifts, and cash movements. Django Ninja validates permissions, prices, totals, stock, and state transitions inside transactions. Only confirmed server rows flow back through PowerSync. Offline drafts/commands remain labeled pending until accepted. The POC must validate the exact PowerSync upload-queue-to-Django integration and durable retry approach.
6. **Database:** Use UUIDs for durable entity IDs and retain integer minor-unit amounts as in the audited original. Keep immutable item/price/discount/tax snapshots on finalized orders, with separate payment/refund/void/shift/cash-movement/audit records. Do not adopt tax/currency policy: Saudi settings are deferred.
7. **Reliability:** Give every retryable command a stable idempotency key (client-generated command UUID); apply command and inventory/cash effects atomically. Use explicit state machines and append-only audit events for financial operations. Inventory and balances are server-authoritative; offline availability/conflict rules await owner decisions.

This is a proposal, not an approved schema or API contract. The owner selected one business for the initial release. Keep record ownership explicit and do not expose multi-business flows without a later owner decision.

## Conceptual data groups

No PostgreSQL migrations or final schema have been written.

- **Identity / tenancy:** businesses, manager users, sessions, registered devices, permissions/role assignment.
- **Catalog:** categories, products, sizes, add-ons, their associations, settings, inventory.
- **Orders:** orders, order lines, option/add-on snapshots, payments, delivery references, stable client IDs and server-assigned display numbers.
- **Operations:** refunds, voids, shifts, cash-drawer sessions, cash movements, end-of-day summaries, audit events.
- **Reliability:** command idempotency keys, sync metadata where needed, and explicit status/error records.

## Offline order-number options and owner selection

Every order should have a unique, client-generated UUID regardless of its human-readable number. The UUID prevents record-key collisions but does not decide the number printed for a customer.

| Option | Offline behavior | Advantages | Trade-offs |
|---|---|---|---|
| A. Temporary device-prefixed display number, then server number | Assign `device-code + local sequence` offline; show it as temporary; retain UUID and attach server number after sync. | Human-readable offline; no reservation service. | Customer may see a changed number after sync; device registration/prefix lifecycle required. |
| B. Server-reserved number ranges per device | Reserve a range online; each tablet consumes its own numbers offline. | Stable familiar sequential numbers, no duplicate range if reservations are enforced. | Wastes numbers, needs device provisioning/renewal, ranges can expire or be abandoned. |
| C. UUID/ULID-derived permanent display code | Generate one globally unique code offline and never replace it. | No number conflict or reassignment; simplest availability model. | Less familiar/longer receipt numbers; ordering by code does not equal sale sequence. |

**Owner selection (Decision 008):** Option A. Keep the UUID as the permanent key and store both the temporary reference and authoritative server sequence. Device-code format, temporary-number receipt wording, and sequence policy remain to define.

## Neon / PowerSync risks

- Enabling Neon logical replication changes `wal_level` for the whole project, cannot be reverted, and restarts all computes. Use a separate throwaway project; do not enable it on a production or shared project for this test.
- An active logical-replication subscriber prevents Neon scale-to-zero and can cause continuous compute usage/cost.
- Neon removes inactive replication slots after about 40 hours; stale subscribers/streams need monitoring and recovery tests.
- Neon documents `max_wal_senders` and `max_replication_slots` as 10. Confirm the PowerSync instance/slot count is sufficient before production.
- PowerSync's generic Postgres setup uses a replication reader with `BYPASSRLS` plus table-read access. Therefore source-table row-level security alone must not be assumed to filter PowerSync data: publication scope and authenticated Sync Stream filters are security-critical. Avoid broad table publication/grants in production and test unauthorized rows/columns explicitly.
- Use a direct (non-pooled) Neon connection for replication; keep API pooling separate. Test PowerSync connection and recovery behavior against the actual Neon plan and network configuration.
- Long offline periods, sync-stream changes, replication lag/slot invalidation, schema upgrades, revoked devices, and concurrent inventory changes need explicit recovery/security tests.
- PowerSync’s write queue is not the POS business rules. Its documented connector sends queued mutations to the application backend, which must synchronously validate and commit before the client acknowledges the batch.

## POC outcome and blockers

**Result: Not run — blocked before build; no pass is claimed.**

| Required item | Environment result |
|---|---|
| Expo app / manifest | None exists; Phase 3 foundation not started. |
| Native Android build | Java, Android SDK environment variables, and `adb` are unavailable. |
| Native iOS build | Workspace is Windows; Xcode/macOS build is unavailable. |
| Cloud development build | Owner selected EAS cloud build, but EAS CLI is not installed and no Expo account/session was provided. Expo documents EAS as a possible remote-build route. |
| Test PostgreSQL | `psql` and Docker are unavailable; no test database or credentials were provided. |
| PowerSync service | No service endpoint/account or disposable project was provided. |
| Neon test project | None provided. Do not create one without approval: logical replication is irreversible on that project and may incur compute charges. |
| Physical tablet | None connected/identified. |

### Required POC checks before Phase 2 can be closed

1. Create a throwaway Expo SDK app with TypeScript and the in-range PowerSync/OP-SQLite versions; compile and launch Android and iOS development builds.
2. Verify local SQLite schema, offline reads/writes, process restart persistence, and watched-query updates.
3. Verify a server-to-client row sync from a test Postgres source through PowerSync.
4. Verify offline command persistence, reconnect/retry, backend validation and transactionality, idempotent replay, and visible accepted/rejected status using Django Ninja.
5. Verify manager/business filtering, token expiry, revoked access, and that unauthorized rows/columns do not sync.
6. Record logs/results, package versions, test database cleanup, and any Neon cost/slot implications.

This POC tests feasibility only; it is not permission to build product features. A required owner approval gate remains after the report and POC review.

## Owner decisions still needed

**Owner decisions recorded on 2026-11-03:** Option A order numbering (Decision 008); one business initially (Decision 009); registered/approved devices required (Decision 010); EAS cloud build preferred; external test-resource setup deferred.

1. Define the device-code format, temporary-number receipt wording, and server sequence policy.
2. When resuming the POC, provide access to an existing disposable test database/PowerSync service through an approved secure mechanism, or explicitly authorize creation of separate disposable resources. Neon replication must be isolated due to its irreversible project setting and possible cost.
3. Authenticate to the existing Expo/EAS account when the cloud build is ready; the account/session is not available in this workspace.
4. Define the manager login provider and registered-device approval/revocation/recovery workflow.
5. Printer/scanner makes and models remain undecided. Hardware-specific compatibility cannot be completed until selected.

## Official sources

All sources below were checked on 2026-11-03.

- [Expo SDK 58 release notes](https://expo.dev/changelog/sdk-58)
- [Expo SDK 58 reference](https://docs.expo.dev/versions/v58.0.0/)
- [Expo development builds](https://docs.expo.dev/develop/development-builds/introduction/)
- [Expo Router](https://docs.expo.dev/versions/v58.0.0/sdk/router/)
- [Expo SQLite](https://docs.expo.dev/versions/v58.0.0/sdk/sqlite/)
- [Expo SecureStore](https://docs.expo.dev/versions/v58.0.0/sdk/securestore/)
- [PowerSync React Native & Expo SDK](https://docs.powersync.com/client-sdks/reference/react-native-and-expo)
- [PowerSync supported platforms](https://docs.powersync.com/resources/supported-platforms)
- [PowerSync setup guide](https://docs.powersync.com/intro/setup-guide)
- [PowerSync writes and backend responsibility](https://docs.powersync.com/handling-writes/writing-client-changes)
- [PowerSync Sync Streams](https://docs.powersync.com/sync/streams/overview)
- [PowerSync React Native + Django example catalog](https://docs.powersync.com/intro/examples)
- [NativeWind docs](https://www.nativewind.dev/docs)
- [Django 5.2 release notes (LTS and Python compatibility)](https://docs.djangoproject.com/en/5.2/releases/5.2/)
- [Django Ninja](https://django-ninja.dev/)
- [Neon logical replication](https://neon.com/docs/guides/logical-replication-neon)

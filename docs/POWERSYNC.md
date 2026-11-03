# PowerSync

**Decision 004 status: Approved by owner on 2026-11-03.** PowerSync and SQLite are not implemented. Phase 2 research supports feasibility, but the required Expo/adapter/Postgres proof of concept has not run.

## Current status

- The original web application does not implement PowerSync or an offline synchronization queue.
- Current `@powersync/react-native` 2.4.0 documents React Native + Expo support and requires `@op-engineering/op-sqlite >=17.1.0 <19.0.0`. Registry latest OP-SQLite is 19.0.3; candidate 18.2.5 is in range.
- The native adapter is not supported in Expo Go; use an Expo Development Build. SDK 58 / RN 0.88 compile status is unverified.
- PowerSync requires a Postgres logical-replication publication, replication role, and service connection. Neon supports logical replication, with material operational/cost caveats documented in `PHASE_2_VALIDATION.md`.
- Sync Streams can filter partial data by authenticated parameters; actual manager/device/business authorization rules and tenant boundaries are not yet designed or tested.
- Client writes enter a durable upload queue and the backend connector must apply them synchronously. Django Ninja must enforce business rules and idempotency before acknowledging any queued write.
- Local schema, migrations, retry behavior, idempotency, and conflict handling: Not Yet Audited.
- **POC:** Not run. Owner selected EAS cloud build but deferred external test resources; no Android/iOS development build, test Postgres database, PowerSync service, or EAS session is available in this workspace.

See `PHASE_2_VALIDATION.md` for source links, exact package snapshot, proposed write/read architecture, environment blockers, Neon risks, and POC acceptance tests.

## Approval gate

Phase 11 work must wait for Phase 2 compatibility validation, the required throwaway proof of concept, and owner approval. If the proof of concept identifies a blocker, revisit the approved decision with the owner before implementation.

## Security and correctness requirements

- Synchronize only records each authorized user may access.
- Do not bypass backend validation for business-critical writes.
- Preserve queued operations across application restarts.
- Do not claim card payment success without confirmation.
- Define and test conflict handling before multi-device stock/order use.
- Never enable logical replication on a production/shared Neon project for a POC: the project setting is irreversible and restarts its computes.

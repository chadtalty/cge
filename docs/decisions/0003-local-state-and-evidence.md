# ADR-0003: Local state, retained evidence, and publication

- **Status:** Proposed; storage and durability guarantees are not implemented.
- **Date:** 2026-10-19
- **Related:** [Issue #5](https://github.com/chadtalty/cge/issues/5), [architecture](../architecture/runner-mvp.md)

## Context and options

Attempt state, event ordering, command deduplication, execution claims and operation intents must remain consistent across interruption. Evidence may be larger than state metadata. Local completion must not be confused with remote publication or gate acceptance.

| Option | Benefits | Costs |
| --- | --- | --- |
| JSON/JSONL files only | Human-readable, easy export | Cross-record locking, atomic projection updates and crash recovery require a separate protocol |
| SQLite plus content-addressed files | Transactions for metadata/events, queryable projections, separate retained byte identities | Database/filesystem are not one transaction; durability and corruption recovery need explicit checks |
| Hosted database/service | Central coordination and remote access | Hosting, credentials, network dependencies and operations exceed the first local slice |

## Recommendation

Use one `state.sqlite` per configured workspace root, with request-scoped identities and records. Put each request's evidence/checkouts/exports under a canonical directory. Store immutable evidence bytes under SHA-256 identities; store accepted events and their state projections together in database transactions. Install/verify referenced bytes before committing their metadata, accepting that orphaned unreferenced blobs may remain after a failed transaction.

The database is authoritative; exported JSON/Markdown is a versioned projection with a declared cutoff. Do not claim the database itself is tamper-proof. Restrict writes; retain corrections as new records. Implementation must verify local filesystem locking/durability behavior and fail closed on inconsistency. Shared/network filesystems and distributed execution are outside the slice.

Adopt an explicit MVP policy permitting durable local retention to satisfy initialization completion. Remote publication is an optional separately authorized bounded activity with its own manifest/attempt lifecycle, intents, retries and receipts; it consumes the completed initialization/export without reopening that attempt. Reject policies requiring remote evidence retention before completion until supported. Never silently weaken a user's policy.

An export receipt is recorded after observing the publication result and references its export commit. It is not part of that same commit. Remote retention of receipts requires a later export or a separately designed receipt channel.

## Consequences and acceptance

The proposal supports local-first use and transactional state transitions, at the cost of a two-stage evidence protocol and an implementation-specific recovery design. It does not establish signing, enterprise retention, secure erasure, or regulatory compliance. Required persistence checks and failure injection are in the [verification plan](../verification/runner-mvp-plan.md).

**Acceptance record:** Pending reviewer identity, evaluated full commit SHA, decision, time, rationale and evidence. Accept storage semantics, local-retention policy, and receipt boundary before implementing dependent work.

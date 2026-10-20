# Runner MVP Verification Plan

**Status:** Proposed test design, not executed runtime tests.  
**Scope:** [Runner requirements R01–R19](../specification/runner-mvp.md).  
**Tracking:** [Issue #5](https://github.com/chadtalty/cge/issues/5).

## Evidence standard

The implementation work package must retain test inputs/framework/policy revisions, test/tool/environment identities, observations, expected/actual outcomes, adapter effects, audit records, and assertion results. Distinguish fixture tests from controlled GitHub integration observations. A flaky or external check reports its observation and environment rather than being declared deterministic.

Use temporary local workspaces, fixture adapters, injected failures, and an explicitly authorized test repository for integration. Tests must not modify existing production issues, branches, or unrelated user work. Acceptance requires the required scenarios to pass or an explicitly authorized, versioned waiver. This document records no runtime passes or waivers.

## Requirement-to-scenario matrix

| ID | Requirements | Scenario and required observation |
| --- | --- | --- |
| V01 | R01, R04 | Resolve two aliases/renamed repository to one stable provider scope; no duplicate workspace/activity or branch |
| V02 | R02, R07 | Reject branch-only framework pin or unresolved mandatory context; retain a blocker and no substantive repository effect |
| V03 | R02, R03, R08 | Revoked/expired grant or missing role denies action even with a pinned historical policy; no hidden permission expansion |
| V04 | R03, R15 | Issue text containing commands/path traversal/credential requests is treated as data; repository and publication allowlists remain authoritative |
| V05 | R04, R13 | Same command ID/payload returns or resumes one receipt/run; same ID with altered request, flags or revisions conflicts |
| V06 | R04, R12 | New command ID for a completed unchanged initialization returns its receipt; no duplicate branch, snapshot, or initialization attempt |
| V07 | R05 | Failure to persist the initial command intent prevents provider discovery/effects; metadata discovery failure records a command error without inventing an activity run |
| V08 | R05, R06 | Retain metadata observations, create the attempt, then capture request bytes; source update/retrieval time and digest are distinguishable |
| V09 | R06, R14 | Change issue content after capture; retained historical snapshot remains unchanged, later initialization reports impact assessment required |
| V10 | R07, R08, R11 | Required ADR Proposed or missing input blocks readiness; optional Proposed ADR has explicit policy/disposition, not fake acceptance |
| V11 | R08, R09 | Eligible initialization seals manifest and records start before branch effects; framework and work checkouts have exact recorded commits |
| V12 | R09 | Dirty checkout, unrelated existing branch, wrong base, path escape or conflicting ownership marker is refused without reset, deletion or force push |
| V13 | R10, R11, R18 | Every required artifact is pinned and every applicable checklist/ADR has update, no-change or N/A rationale; missing scope data stays unknown |
| V14 | R12 | Omit/corrupt required output, verification result or completion evidence; no successful completion is reported |
| V15 | R12, R13 | Crash after evidence install but before DB commit; blob is unreferenced, no accepted completion exists, recovery verifies facts |
| V16 | R12, R13 | Crash after accepted completion transaction but before CLI reply; repeated command returns that durable result, not another run |
| V17 | R13 | Concurrent initialization and stale sequence/claim fail safely; no simultaneous active executors for one activity |
| V18 | R13 | Interrupted running attempt resumes only after effects reconciliation, exclusive ownership and authority checks; unverifiable old process/effects block resume |
| V19 | R14 | Terminal failure plus explicit retry creates a linked new attempt only with reconciled effects and unchanged eligible bindings; prior run remains terminal |
| V20 | R14 | Attempted retry with changed framework/config/context reports impact/rework required; old sealed bindings stay intact |
| V21 | R15 | Default initialization makes no remote write; explicit publication checks actor, content/audience allowlist, destination and export manifest |
| V22 | R15, R16 | Push denied or failed after local completion yields separate completed-local and failed-publication results; local evidence survives; publication has its own bounded activity/attempt and does not extend a terminal initialization |
| V23 | R15, R16 | Timeout after push is unknown until observed; reconcile expected export commit and avoid another initialization or blind duplicate effect |
| V24 | R15, R16 | Export/receipt cutoff has no circular reference; non-fast-forward push is blocked; remote-required completion policy is rejected before execution |
| V25 | R17 | Hash files and logical DB state before/after `status`; no writes, provider calls, file creation, recovery, journal-setting change or claim renewal |
| V26 | R17, R18 | Missing/corrupt workspace/root identity, unsupported record version or incomplete projection produces error/recovery-required, not fabricated empty success |
| V27 | R17 | Profile demanding an unavailable audited-status wrapper is explicitly unsupported; no silent exemption from required auditing |
| V28 | R18 | Done work + Proposed downstream decision + Passed initialization checks + Failed publication remain separate; no feature-ready conclusion or numeric confidence probability |
| V29 | R18 | Status reports last provider observation/snapshot sequence, assumptions/gaps and unknown coverage rather than asserting fresh remote readiness |
| V30 | R19 | Implementation readiness check blocks until required ADRs/precedence/design contract have explicit accepted dispositions at evaluated versions |

## Planned test layers

1. Controller/contract tests with fixture adapters: identities, state preconditions, authority outcomes, status dimensions, and prohibited transitions/effects.
2. Persistence and recovery tests: command deduplication, claims, evidence digests, transaction boundaries, missing blobs and failure injection at every durable boundary.
3. Git adapter tests using disposable local repositories: clean/dirty/colliding states, base/branch identity, and export commit reuse.
4. Opt-in GitHub integration checks in an authorized test repository: source metadata observations, remote read failures, publication, conflict and timeout reconciliation.
5. One recorded demonstration against a bounded CGE work item, after review approval, with no implicit gate merge/acceptance.

## Controlled demonstration to produce later

The [illustrative walkthrough](../../examples/runner-init-status/README.md) describes expected receipts. The future implementation must actually execute initial `init`, repeated `init`, read-only `status`, an injected prerequisite block, interruption/recovery, and publication failure. Retain the exact invocation/config/framework revisions and observed effects; scrub credentials and restrict publication audience appropriately.

Report work coverage as required scope/artifact items addressed versus declared requirements, with justified exclusions. Report verification coverage as scenarios actually executed against that scope, with pass/fail/not-assessed and evidence. Report acceptance readiness against decision/gate prerequisites separately. Do not convert these measures into a calibrated correctness probability.

## Current verification disposition

All V01–V30 are **Not Executed**: no runner exists in this design package. Documentation link/fence/requirement-reference checks are separate authoring checks and are recorded in the PR and package checklist. They do not satisfy runtime acceptance.

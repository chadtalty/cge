# Example: Decomposition Request

**Status:** Illustrative example; not a live GitHub issue.

## Request

A client needs a new query capability with clearly defined authorization, query behavior, and operational limits.

## Context

- Functional requirements and expected users must be documented.
- Security, performance, and operational constraints must be identified.
- The architect may derive multiple features, including work across repositories.

## Architecture activity

**Objective:** Decompose the request into independently testable and deployable features.

**Inputs:** Intake request and context manifest.  
**Outputs:** Feature definitions, analysis, design, planning artifacts.  
**Boundary:** Architecture only; no implementation or deployment.  
**Gate:** Architecture readiness review with pinned artifact versions.

## Traceability sketch

`INTAKE-001 → ACT-DECOMPOSE-001 → FEATURE-001 / FEATURE-002 → design artifacts → GATE-ARCH-001 → audit event(s)`

## v0.2 lifecycle and audit walkthrough

**Status:** Proposed illustrative extension for [Issue #3](https://github.com/chadtalty/cge/issues/3). All identities, versions, people, times, and events below are fictional. They are not records of repository activity or an implemented ledger.

The [activity lifecycle](../../docs/architecture/activity-lifecycle.md) distinguishes the reusable activity definition, the work item, and its execution attempts. The [audit model](../../docs/architecture/audit-model.md) supplies event and authority requirements.

### Boundaries and bindings

| Reference | Meaning |
| --- | --- |
| `WF-QUERY-001` | Workflow instance for intake `INTAKE-001`; correlation ID `CORR-QUERY-001` |
| `WF-DECOMPOSE@v1` | Workflow-definition version with the architecture transitions in the reference workflow |
| `ACT-DECOMPOSE-001` | One bounded architecture work item; no implementation or deployment permission |
| `DEF-DECOMPOSE@v1` | Reusable decomposition activity definition |
| `CTX-QUERY@v1` | Initial requirements, issue snapshots, constraints, and dependencies |
| `CTX-QUERY@v2` | Revised requirements/snapshots addressing gate findings about authorization boundaries |
| `MANIFEST-001@v1` / `MANIFEST-002@v1` | Sealed manifests for attempts 001 and 002; same governing context/input versions, different attempt identities |
| `MANIFEST-003@v1` | Sealed manifest for attempt 003, using revised context/input versions |
| `BUNDLE-QUERY@v1` / `BUNDLE-QUERY@v2` | Sealed review bundles containing exact context, manifest, feature snapshots, analysis, design, planning, and check-result versions |
| `GATE-ARCH@v1` | Architecture gate definition: boundaries, security, dependencies, acceptance criteria, and verification strategy |
| `POLICY-ARCH@v1` | Architect may execute; distinct designated reviewer may decide the architecture gate; coordinator may advance the workflow |

These symbolic versions stand in for immutable fixture versions, not mutable branch names. A real GitHub execution would replace them with resolvable context/manifest identifiers and repository + path + full commit SHA references. Each bundle identifies the attempt it evaluates; it excludes its later gate decision record to avoid a circular commit reference.

### Failure, retry, and gate rework

| Attempt | Binding | Outcome and next action |
| --- | --- | --- |
| `ATTEMPT-001` | Definition v1, context v1, manifest 001 | Fails after an evidence export error; partial outputs remain unaccepted. Executor confirms no unresolved external effects. |
| `ATTEMPT-002` | Same definition/context/input versions; manifest 002; `retry_of: ATTEMPT-001` | Completes and submits bundle v1. Reviewer requests changes because the proposed features do not state independent authorization boundaries. |
| `ATTEMPT-003` | Definition v1, context v2, manifest 003; `rework_of: ATTEMPT-002` | Addresses the findings, completes, and submits bundle v2. A new evaluation approves that bundle; the coordinator then advances the workflow. |

Attempt 002 remains `completed` after the gate requests changes. Completion means its execution contract was satisfied; gate acceptance is a separate decision. Attempt 003 supplies new versions without overwriting attempt 002's evidence. No activity execution begins before its manifest is sealed and an authorized executor claims it.

### Illustrative event sequence

The sequence below is a narrative view across streams, not a claim of a global ledger sequence. Each event also has a sequence number within its own authoritative subject stream. All events share workflow instance `WF-QUERY-001` and correlation `CORR-QUERY-001`; causes refer to immediate recorded causes, not every related event.

| Event ID | Event type | Subject and result | Immediate cause(s) |
| --- | --- | --- | --- |
| `E01` | `context.versioned` | Context v1 captured; root origin is the illustrative intake | None; root rationale recorded |
| `E02` | `attempt.created` | Attempt 001 enters `planned` | E01 |
| `E03` | `attempt.readied` | Attempt 001: `planned → ready`; manifest 001 sealed | E02 |
| `E04` | `activity.started` | Attempt 001: `ready → running`; architect authorized | E03 |
| `E05` | `activity.failed` | Attempt 001: `running → failed`; export error and effect disposition recorded | E04 |
| `E06` | `attempt.created` | Attempt 002 enters `planned`, retry of 001 | E05 |
| `E07` | `attempt.readied` | Attempt 002: `planned → ready`; manifest 002 sealed | E06 |
| `E08` | `activity.started` | Attempt 002: `ready → running`; architect authorized | E07 |
| `E09` | `artifact.produced` | Bundle v1 pinned, produced by attempt 002 | E08 |
| `E10` | `activity.completed` | Attempt 002: `running → completed`; checks and bundle v1 recorded | E09 |
| `E11` | `workflow.transitioned` | `Architecture in progress → Architecture review` | E10 |
| `E12` | `gate.evaluated` | Evaluation 001: `changes_requested` against bundle v1, context v1, attempt 002 | E11 |
| `E13` | `workflow.transitioned` | `Architecture review → Architecture rework` | E12 |
| `E14` | `context.versioned` | Context v2 captures revised requirements and snapshot versions | E12, E13 |
| `E15` | `attempt.created` | Attempt 003 enters `planned`, rework of 002 addressing evaluation 001 | E13, E14 |
| `E16` | `attempt.readied` | Attempt 003: `planned → ready`; manifest 003 sealed | E15 |
| `E17` | `activity.started` | Attempt 003: `ready → running`; architect authorized | E16 |
| `E18` | `artifact.versioned` | Bundle v2 pinned, produced by attempt 003, revising bundle v1 | E17 |
| `E19` | `activity.completed` | Attempt 003: `running → completed`; checks and bundle v2 recorded | E18 |
| `E20` | `workflow.transitioned` | `Architecture rework → Architecture review` | E19 |
| `E21` | `gate.evaluated` | Evaluation 002: `approved` against bundle v2, context v2, attempt 003 | E20 |
| `E22` | `workflow.transitioned` | `Architecture review → Ready for implementation`; coordinator checks evaluation 002 | E21 |

The architect, reviewer, and coordinator are human roles here. GitHub commits and PR reviews supply evidence references; a coordinator can record these events manually without an AI agent. The example does not authorize implementation or deployment.

### Complete illustrative gate event

This is a conceptual encoding of E21, not a finalized machine-readable schema. Evidence references are symbolic as defined above. The cited review record exists before its decision is transcribed into the ledger.

```yaml
event_id: E21
contract_version: cge-audit-proposal@v0.2
event_type: gate.evaluated
occurred_at: 2026-10-19T15:00:00Z
recorded_at: 2026-10-19T15:01:00Z
actor:
  id: human:reviewer-b
  kind: human
recorded_by: human:coordinator-c
subject:
  kind: gate_evaluation
  id: EVAL-ARCH-002
  activity_id: ACT-DECOMPOSE-001
  attempt_id: ATTEMPT-003
workflow_instance_id: WF-QUERY-001
workflow_definition_ref: WF-DECOMPOSE@v1
correlation_id: CORR-QUERY-001
causation_event_ids: [E20]
command_id: CMD-REVIEW-002
context_ref: CTX-QUERY@v2
execution_manifest_ref: MANIFEST-003@v1
artifact_refs: [BUNDLE-QUERY@v2]
evidence_refs: [REVIEW-ARCH-002@v1, CHECKS-QUERY@v2]
authorization:
  policy_ref: POLICY-ARCH@v1
  grant_ref: GRANT-REVIEWER-B@v1
  authority_id: human:reviewer-b
  decision: allowed
  evidence_refs: [REVIEWER-ASSIGNMENT@v1]
outcome: approved
rationale: Revised features have independent authorization boundaries and all required criteria pass.
payload:
  gate_definition_ref: GATE-ARCH@v1
  evidence_bundle_ref: BUNDLE-QUERY@v2
  criteria:
    feature_boundaries: pass
    security_boundaries: pass
    dependencies: pass
    acceptance_criteria: pass
    verification_strategy: pass
  findings: []
  waivers: []
  supersedes_evaluation_id: EVAL-ARCH-001
stream_id: gate:EVAL-ARCH-002
sequence: 1
```

E22 references the stored E21 decision version and separately authorizes the workflow transition. A material change to context v2 after approval would require impact recording, updated snapshots, and a new evaluation for the changed scope. It would not change E21 to say it had evaluated later versions.

### Cancellation and correction cases

- If attempt 003 receives a cancellation request while running, record `cancellation.requested`. It remains running until the executor stops or is fenced and partial outputs/effects are reconciled. Only then may `activity.cancelled` be recorded. E19–E22 would not occur in that alternative path.
- If a retry command is delivered twice with the same command ID, return the original result instead of creating two attempts.
- If E21's recording time were transcribed incorrectly, append `audit.corrected` identifying E21 and the corrected assertion. Keep E21 unchanged. If the error concerned approval authority or evaluated scope instead, reconciliation must pause dependent progression; a correction cannot manufacture valid approval.

### Review checklist

- [ ] Definition, activity, attempt, manifest, and evaluation identities are distinct.
- [ ] Retry preserves governing versions; rework supplies new bindings and links findings.
- [ ] Completed execution and gate approval remain separate.
- [ ] Human roles and authorization evidence suffice without AI execution.
- [ ] Every gate and workflow transition references exact evaluated evidence versions.
- [ ] Failed attempts, prior bundles, findings, and corrections remain visible.


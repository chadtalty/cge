# Audit Model — v0.2 Proposal

**Status:** Proposed; requires the [Issue #3 quality gate](https://github.com/chadtalty/cge/issues/3).  
**Scope:** Conceptual event contracts and manual recording conventions; no implemented event store or validation schema.  
**Related:** [Activity lifecycle](activity-lifecycle.md), [conceptual model](conceptual-model.md), and [decomposition workflow](../workflows/decomposition.md).

## Intent and limits

An audit record preserves a meaningful occurrence or decision: what happened, who acted, under which authority, against which exact versions, and with what result. Traceability links describe relationships between requests, activities, artifacts, and decisions; an audit event may establish a link, but the link graph is not the event history.

The requirements below are proposed CGE contracts. JSON/YAML encodings and identifiers in examples are illustrative, not finalized schemas or existing infrastructure.

## Event envelope

| Field | Requirement and meaning |
| --- | --- |
| `event_id` | Unique stable record identity; reuse on redelivery of the same record |
| `contract_version` | Version of the event contract used to interpret this record |
| `event_type` | Named occurrence with defined payload semantics |
| `occurred_at`, `recorded_at` | UTC occurrence and recording times; unknown occurrence time is explicitly null with an explanation |
| `actor` | Attributable acting identity and kind (`human`, `agent`, `script`, or `hybrid`); hybrid work identifies accountable participants |
| `recorded_by` | Identity of the person or service recording the event, which may differ from the actor |
| `subject` | Primary subject kind and stable identity; include activity/attempt or gate-evaluation identifiers as applicable |
| `workflow_instance_id`, `workflow_definition_ref` | Workflow correlation and exact governing definition version |
| `correlation_id` | Stable identifier grouping events for the bounded request/workflow |
| `causation_event_ids` | Immediate recorded causes; an explicit empty list and origin rationale for a root event |
| `command_id` | Stable identity of the command/request being handled, when applicable; distinguishes delivery retries from new attempts |
| `context_ref`, `execution_manifest_ref` | Exact versions governing the action; required for attempt transitions and gate decisions |
| `artifact_refs`, `evidence_refs` | Exact artifact/evidence versions relevant to the occurrence; an explicit empty list if none |
| `authorization` | Required for controlled actions: policy version, grant/delegation reference, authority identity, decision and evidence; explicit reason when not applicable |
| `outcome`, `rationale`, `payload` | Result, explanation, and event-specific facts, including state changes, findings, or corrections |
| `stream_id`, `sequence` | Ledger-assigned order within an authoritative subject stream; sequence increases without reuse |

If context or a manifest does not yet exist (for example, intake capture), record null with an explicit reason. That exception must not be used to start work or approve a gate without pinned versions. Links to live issues may supplement immutable evidence, but cannot replace it.

Per-stream sequence establishes recording order, not a universal causal clock. Timestamps can be delayed or skewed. Across streams, use explicit causation references and version identifiers rather than inferring order from timestamps. Record later-received evidence with its actual recording time; never backdate or insert it into previously recorded history.

## Event vocabulary and required payloads

| Event | Required event-specific facts |
| --- | --- |
| `intake.created` | Request identity, origin, and capture evidence; identify live issue and snapshot separately when a snapshot exists |
| `attempt.created` | Activity and new attempt IDs, initial `planned` state, definition version, draft manifest reference; predecessor relation and rationale when applicable |
| `attempt.readied` | `planned → ready`, sealed manifest, pinned inputs, readiness-check evidence |
| `activity.started` | `ready → running`, executor claim, start authorization |
| `activity.completed` | `running → completed`, exact outputs, completion-check results, effects disposition |
| `activity.failed` | `running → failed`, failure/stopping condition, partial outputs, reconciliation and effect uncertainty |
| `cancellation.requested` | Requested stop, requester, authority, reason, affected attempt; no implied state change |
| `activity.cancelled` | Previous state → `cancelled`, effective stop/fencing evidence and effects disposition |
| `context.versioned` | Context identity and new version, predecessor version if any, pinned dependencies and change rationale |
| `artifact.produced` / `artifact.versioned` | Artifact identity, exact new version, producing attempt, and predecessor reference for a revision |
| `feature.proposed` | Feature identity, source request, pinned definition snapshot and boundaries |
| `gate.evaluated` | Evaluation ID, gate-definition version, criteria results, decision, findings, evaluated context/manifest/attempt and evidence-bundle versions, reviewer and authority |
| `workflow.transitioned` | Previous/new workflow states, expected state, transition-rule version, prerequisite evidence and authorizing gate decisions |
| `transition.denied` | Requested transition, observed/expected state, requesting actor, failed condition or denied authority; state unchanged |
| `approval.impact_recorded` | Prior decision ID, changed scope/versions, affected activities, and authorized pause/review/rework disposition |
| `audit.corrected` | Original event ID, erroneous assertions, corrected assertions, reason, evidence and correction authority |

The vocabulary preserves v0.1 event names. `artifact.produced` records initial production; `artifact.versioned` records a revision. These events alone do not approve an artifact. A missing tool event must not be filled in with a fabricated actor, time, or decision.

## Gate evidence and version pinning

A gate evidence bundle is a sealed manifest of the exact context version, execution manifest, attempt ID, issue snapshots, artifacts, and check results being evaluated. Pin the bundle, the gate definition, and the authorization policy in `gate.evaluated`.

Keep the decision separate from its evaluated bundle. For GitHub, record the evaluated commit SHA in the decision; a later commit may store that decision without circularly claiming to be its own evaluated commit. Pin the decision record's own storage version when it is consumed by a workflow transition.

`approved` requires every mandatory criterion to pass or an explicitly permitted, versioned waiver from its authorized authority. Record waived criteria and their scope. `changes_requested` and `rejected` require findings and a declared rework destination. A check result is evidence; it becomes a decision only when the workflow explicitly grants decision authority to that actor or automation.

After material input or scope changes, append `approval.impact_recorded` rather than editing the original decision. Changed versions require a new evaluation. A correction to an old gate event does not itself grant new approval or silently move the workflow.

## Append-only recording, corrections, and replay

- Accepted records must not be edited, replaced, renumbered, or silently deleted. A correction is a new event referencing the original; retain both and make the correction visible in projections.
- A new gate evaluation is a new decision, not a correction merely because its outcome differs. Retry and rework create new attempts and preserve predecessor history.
- Deduplicate by `event_id`; a repeated ID with different content is an integrity conflict requiring investigation. Use `command_id` plus its recorded outcome to prevent repeated command delivery from repeating effects.
- State-changing records include expected previous state and sequence. Conflicts or missing causal evidence must be reconciled before dependent advancement.
- A ledger and state projection must have a recoverable persistence relationship. Replay may reconstruct state from validated accepted transitions; it must never rerun external actions such as merging or deploying.

Corrections identify inaccurate assertions and their replacements; consumers preserve original occurrence/recording times and record the correction's own times. An authorized reconciliation is needed if a correction affects state or approval validity. Do not silently rewrite projected decisions and allow downstream work to proceed.

## Manual GitHub reference procedure

1. Use the request/workflow ID as a correlation key. Allocate stable attempt and evaluation IDs before recording their events.
2. Maintain an explicitly ordered ledger, such as a reviewable Markdown table plus versioned evidence files. A designated coordinator serializes controlled transitions and verifies claims.
3. Record actor-supplied facts and native GitHub evidence separately. A recorder may transcribe a human decision; it must identify the human decision-maker and the recording identity.
4. Append the next event and link exact context, artifact, review, and commit references. Record occurrence and recording times separately.
5. Before advancement, reconcile the state projection with accepted events and verify the designated reviewer and transition authority.

This procedure demonstrates execution independence. It does not make GitHub files tamper-proof: branch protection, history-rewrite controls, restricted writes, retention, backup, and optional signing/integrity mechanisms remain implementation decisions. Git commits provide evidence versions; they do not automatically create the normalized event ledger described here.

Never put credentials, tokens, unnecessary personal information, or sensitive full payloads into the ledger. Prefer minimal facts and access-controlled evidence references. Retention and required erasure need a separately reviewed policy; this proposal does not choose one or claim absolute immutability.

## Review decisions still required

- Confirm the event names, minimum envelope, and per-subject stream ordering.
- Choose identifier formats, timestamp precision, contract evolution rules, validation schemas, and a storage adapter in a later implementation.
- Define durable deduplication, recovery, access control, retention, and correction/reconciliation authority before automation.
- Decide how cross-repository causal events are collected without claiming unsupported global ordering.

The [worked example](../../examples/decomposition-request/README.md) supplies an illustrative sequence and a complete gate event using these concepts.

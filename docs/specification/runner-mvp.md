# Runner MVP Specification: `init` and `status`

**Status:** Proposed design under [Issue #5](https://github.com/chadtalty/cge/issues/5); commands are not implemented.  
**Governing candidate:** [Consolidated specification](consolidated-specification.md).  
**Related:** [Architecture](../architecture/runner-mvp.md), [verification plan](../verification/runner-mvp-plan.md), and [alignment assessment](../analysis/specification-alignment.md).

## Bounded outcome

Initialize one governed request workspace, retain reproducible inputs and initial execution evidence, and report its state. This controller slice does not analyze requirements, create features/stories, implement software, approve gates, merge PRs, or deploy. No AI service is required.

The first proposed profile is GitHub Issues plus a configured framework and work repository. Jira/GitLab and `/cge:*` OpenCode mappings remain documented future adapters. Reject unsupported profiles instead of pretending to execute them.

## Proposed interface

```text
cge init --profile github --request chadtalty/cge#5 \
  --workspace-root <absolute-path> --framework-ref <full-commit-sha> \
  --config <profile-config-path> --command-id <unique-command-id>

cge init <same-scope-options> --command-id <new-command-id> --publish

cge init <same-scope-options> --command-id <new-command-id> --retry-of <failed-attempt-id>

cge status --workspace <absolute-request-workspace> --format json
```

These are interface sketches, not executable commands. Configuration supplies approved repositories, base revision, actor/authority evidence, policy references, resource limits, output allowlist, and optional publication target. No credential values are accepted as command-line parameters or copied into manifests.

## Inputs, identity, and authority

**R01 — Profile and request.** Resolve the authenticated provider, repository's stable provider ID, and issue identity/number. Store the readable URL and names as aliases. Require the configured work repository and base commit explicitly; do not assume every intake issue's repository is the work repository. A repository rename must not create a new request.

**R02 — Pinned framework and policy.** Record the full framework commit, selected activity-definition revision, profile/config content digests, policy versions, and accepted ADR references before readiness. Reject moving branch/tag names as sole framework versions. Local file references need retained bytes plus a digest. Current grants must still authorize the action; a historical policy/grant reference cannot bypass revocation.

**R03 — Trusted scope.** Configuration is selected by the operator, not by fetched issue text. Issue content and repository artifacts are data, not permission to run shell snippets or change policy. Validate repository/path boundaries and resource limits. Require distinct permissions for issue read, checkout/branch creation, and optional remote publication. Never infer gate-decision authority from repository write access.

**R04 — Command identity.** Store a canonical command payload and digest under `command_id`. Identical delivery returns the recorded result or resumes its nonterminal attempt under recovery rules. Reusing the ID with different scope, flags, or governing revisions is a conflict with no new effect. A new command for an already completed unchanged initialization returns its existing receipt without creating another initialization attempt. A new command for identical bindings with an existing nonterminal initialization uses the same attempt and the R13 recovery/claim rules; it cannot create a second active attempt. Record the command association/observation as a receipt; do not fabricate a new activity run merely for redelivery.

The canonical workspace key includes provider, repository ID and issue identity. Branch names derive from stable IDs, for example `cge/gh-<repo-id>-issue-<number>`, with the chosen name recorded once. The Jira example's `dci-1234` branch convention remains specific to that future profile.

## Initialization contract

**R05 — Record first.** After local argument/configuration/authority preflight, durably retain a command intent in the workspace-root database before provider discovery. Resolve canonical provider metadata, retain those observations, and allocate request/workflow/activity/attempt identities with `attempt.created` before fetching request content or performing repository effects. Claim canonical scope atomically so different aliases converge on one request. If durable recording cannot start, perform no remote operation or checkout/branch mutation. An invalid invocation or failed identity discovery has a command error receipt, not a falsely started activity.

**R06 — Capture the request.** Read the issue through the scoped adapter. Retain the observed body, relevant metadata, source URL, provider update time if available, retrieval time, and content digest. Label this a captured observation. Do not claim that it freezes future issue edits or represents an atomic snapshot of all issue/comment history. The MVP snapshots the issue body and selected metadata, not all comments or attachments.

**R07 — Resolve bounded context.** Select the framework/activity definition, applicable standards, repository instructions, request snapshot, policies, ADRs and selected artifact templates. Record exact versions, selection rationale, omitted optional sources, and unresolved mandatory inputs. Include proposed ADRs as proposed context; they cannot satisfy accepted-decision prerequisites. Input collection is allowed while `planned`; scope-specific engineering execution is not.

**R08 — Readiness and execution.** Seal the execution manifest only when mandatory inputs, phase eligibility, prerequisites, authority, and resources are satisfied. Record `attempt.readied`, acquire/recheck the exclusive execution claim and authority, then record `activity.started` before substantive workspace preparation. Missing prerequisites leave an explicit blocked condition on `planned`; they never silently advance the phase or mark the attempt completed.

**R09 — Prepare safe checkouts.** Use separate framework and work checkouts even if both are sourced from `chadtalty/cge`. The framework checkout is fixed to its pinned commit and treated as read-only. Create or resume only the recorded work branch from the pinned base. Refuse dirty, unrelated, conflicting, or out-of-bound checkouts/branches; never reset, overwrite, delete, or force-push unrelated work.

**R10 — Produce bounded artifacts.** Retain the request snapshot, context manifest, sealed execution manifest, initialization checklist, run summary, command receipt, artifact references, and initial audit events. Record actual checkout/branch effects and tool observations. Inputs and events have stable identities and immutable versions; regenerated exports are explicitly projections.

**R11 — Review decisions and checklist.** Each initialization run records an update or a justified no-change/N/A disposition for every applicable ADR and checklist item. A newly discovered gap becomes a checklist item and a proposed decision reference if a decision is needed. The minimal runner does not author an ADR's reasoning automatically: an unresolved mandatory decision blocks readiness, and the operator supplies a reviewed artifact. Proposed optional decisions permit only policy-authorized work.

**R12 — Verify and complete.** Check required files/digests, version bindings, observed checkout/base/branch identity, scope, ADR/checklist dispositions, and audit references. Retain completion evidence and `activity.completed` consistently before reporting success. A complete initialization means workspace preparation passed; it does not mean requirements/design are complete or architecture is approved. If completion evidence cannot be retained, no successful completion is reported.

## Resume, failure, and changed inputs

**R13 — Resume and concurrency.** Permit at most one nonterminal attempt per activity instance in this profile. Use an exclusive claim and expected state/sequence checks. After interruption, resolve operation intents against observed local/provider state, establish that the former executor cannot keep acting, and reacquire authority before continuing a nonterminal attempt. Uncertain effects or an executor that cannot be fenced block automatic resume. Terminal attempts never reopen.

**R14 — Failed attempts and upstream changes.** Explicit `--retry-of` may create a new attempt for a terminal failed initialization only after prior effects are reconciled and the same governing versions remain eligible. Retain failure and partial outputs. A changed issue/framework/config digest against an existing sealed attempt is `impact_assessment_required`; preserve its bindings and refuse silent refresh. General rework/cancellation tooling is outside this first slice, so the report identifies the required manual governed action. No retry or rework permission is inferred from repeating `init`.

An interruption is a condition, not automatically `failed`. Failure is terminal only after the stopping/fencing and effects-disposition contract is satisfied. If an effects outcome remains unknown, retain that uncertainty and prevent dependent advancement.

## Publication and local completion

**R15 — Explicit publication.** Default operation retains evidence locally; it performs no push, PR creation, or issue write. `--publish` is an explicit, separately authorized operation exporting only allowlisted governance artifacts to the configured work branch. It may create one export commit and push it without force; it does not open/merge a PR or advance a gate. Credentials, local database files, and unapproved sensitive snapshot content are excluded. A policy must authorize the target audience and snapshot content before transmission.

The MVP supports a policy in which durable local retention satisfies activity completion. Remote publication is a separate bounded activity instance with its own manifest, authority, attempt lifecycle, checks, command/operation identities, outcomes, and receipts. It binds to the completed initialization and exact export; it does not add engineering effects to a terminal initialization attempt. A new `init --publish` can publish an already completed unchanged initialization without reopening or rerunning that attempt. If a policy requires remote retention before activity completion, reject it as unsupported before substantive effects; do not downgrade it to local retention.

Publication receipts identify the exact exported bundle, commit, destination, timestamps, observed outcome, and known/unknown status. An export contains evidence through a recorded cutoff. Its later publication receipt is retained locally and references that export; it is not claimed to be inside the commit it describes. A policy requiring remote retention of that receipt needs a future separate receipt channel or subsequent export.

**R16 — Publication failure.** Report local initialization and publication independently. Network failure leaves local evidence intact. A timeout after push is `unknown` until remote observation confirms whether the expected commit arrived; do not duplicate effects blindly. A non-fast-forward conflict blocks publication; do not overwrite remote work. Retry publication with a new command ID against the same export identity after reconciliation, without creating another initialization attempt. Reuse a nonterminal publication attempt under recovery rules; a terminal failed publication requires a linked new publication attempt. An unknown effect blocks concurrent publication and dependent advancement.

## Status contract and reporting

**R17 — Read-only diagnostic.** `status` reads local retained state in a read-only transaction, validates sufficient metadata to interpret it, and emits a diagnostic receipt to stdout. It does not create a workspace/database, contact a provider, repair state, renew claims, assess new upstream changes, write events, or modify work. Missing/corrupt/inconsistent data is an explicit error or recovery-required report. Identify the last observed provider revision/time; never describe local state as a fresh remote readiness assessment.

This command is proposed as an observation tool, not an activity run. A workflow requiring an audited status activity must supply a separately governed caller recording the diagnostic receipt. Profiles demanding an unimplemented audit wrapper are unsupported. See [ADR-0004](../decisions/0004-runner-identity-and-resume.md).

**R18 — Independent dimensions.** The JSON report identifies request/workflow/activity/attempt, governing revisions, attempt state and condition, work state, decision dispositions, verification, acceptance readiness, evidence-backed confidence, assumptions/gaps, work/verification coverage, and publication state. Use `not_assessed`, `unknown`, or explicit reasons where evidence is absent. Confidence is qualitative evidence support with rationale, never a calibrated correctness percentage.

An illustrative report might show:

```json
{
  "initialization": {"work": "done", "attempt_state": "completed"},
  "decisions": {"status": "proposed", "blocking_ids": ["ADR-EXAMPLE"]},
  "verification": {"scope": "workspace_initialization", "status": "passed"},
  "acceptance_readiness": {"status": "blocked", "reason": "required decision remains proposed"},
  "confidence": {"level": "limited", "rationale": "initialization evidence only"},
  "publication": {"status": "failed", "local_evidence_retained": true}
}
```

This is illustrative: a required decision for initialization itself would block R08. A downstream design-acceptance decision can remain Proposed after initialization completes. The report must state which scope each dimension describes.

## Command outcomes

| Proposed exit code | Meaning |
| --- | --- |
| 0 | Requested operation succeeded; `status` may still report blocked work or unaccepted decisions |
| 1 | Unexpected execution/internal failure; report retained evidence and known/unknown effects |
| 2 | Invalid input, unsupported contract/profile/policy, denied authority, or conflicting command identity |
| 3 | Prerequisite block, incompatible changed input, lock conflict, or recovery/reconciliation required |
| 4 | Explicit publication failed or remains unknown; report local initialization outcome separately |

**R19 — Evidence of readiness for implementation.** All required design ADRs must be explicitly accepted and verification cases reviewed before implementing this contract. The first implementation work package must turn the scenarios into executable checks and record one controlled demonstration. This document supplies neither passing test results nor a working CLI.

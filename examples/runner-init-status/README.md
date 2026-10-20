# Illustrative Runner `init` / `status` Walkthrough

**Status:** Proposed interface example; no commands were executed and no listed workspace, run, or publication is claimed to exist.  
**Contract:** [MVP specification](../../docs/specification/runner-mvp.md).  
**Validation:** [Planned verification scenarios](../../docs/verification/runner-mvp-plan.md).

## First initialization

```text
cge init --profile github --request chadtalty/cge#5 \
  --workspace-root <workspace-root> --framework-ref <accepted-full-commit-sha> \
  --config <trusted-profile-config> --command-id <command-001>
```

The placeholders must be replaced with real validated values after implementation and design acceptance. The reference to Issue #5 illustrates a request format; it does not make this example an audit record for that issue.

Expected behavior: retain the command intent, resolve stable request identity, allocate an initialization attempt, capture the issue observation, select/pin context, verify prerequisites, seal the manifest, acquire an authorized execution claim, prepare safe checkouts, verify outputs, and retain completion evidence. Default execution makes no remote write.

Expected artifact set (encoding remains proposed):

| Artifact | Required content |
| --- | --- |
| Request snapshot | Source identity/URL, observed body/metadata, provider update time, retrieval time, byte digest |
| Context manifest | Exact framework/standard/policy/input/ADR revisions; scope and inclusion/exclusion rationale |
| Execution manifest | Activity/attempt/workflow identities, definition version, inputs, scope, role, authority and completion contract |
| Initialization checklist | Required outputs/checks and per-ADR/checklist update/no-change/N/A dispositions |
| Run summary | Actual effects/outputs, assumptions/gaps, decisions, observations, confidence rationale, scoped coverage and publication outcome |
| Command receipt | Canonical command digest/identity, associated attempt/result and recovery information |
| Event/evidence export | Ordered accepted records and evidence manifest through an explicit export cutoff; no claim of a full universal ledger implementation |

The root database stores accepted state and event metadata; request-scoped content-addressed files retain exact bytes. Human-readable exports are projections, not a second authority that can silently contradict the database.

## Repeated initialization

- Same command ID and payload: return its recorded result, or resume its nonterminal attempt after ownership/authority/effect reconciliation.
- New command ID, unchanged scope/bindings, initialization already completed: return the existing initialization receipt; no duplicate branch or attempt.
- Same command ID with different framework/config/flags: report conflict.
- Changed input against a sealed attempt: preserve the prior binding and report impact assessment required.

## Missing prerequisites and interruption

A missing required accepted ADR leaves work blocked and the attempt `planned`. The runner reports the exact missing version/disposition. It does not interpret a Proposed recommendation as Accepted.

An interrupted running attempt is not automatically failed. Resume requires establishing exclusive ownership, checking current authority, and reconciling every in-flight effect. Unknown effects block automatic progression. An explicitly failed/fenced terminal attempt needs an authorized new retry attempt; it is never reopened.

## Publication after local completion

```text
cge init <same-scope-options> --command-id <command-002> --publish
```

The new command may publish a completed initialization's already identified export after separate content/audience/actor authorization. It does not rerun initialization. Publication has its own bounded activity and attempt lifecycle, with a manifest consuming the completed initialization and pinned export. A failed push yields a failed/unknown publication result while retaining the completed local work and its evidence. A receipt records the expected/observed remote commit, or why the result is unknown. It does not claim to be inside its own export commit.

## Status observation

```text
cge status --workspace <workspace-root>/requests/<scope-key> --format json
```

The report shows exact local snapshot/sequence, framework/context/attempt references, decision/checklist dispositions, scoped verification and coverage, confidence rationale, blockers, and publication outcome. It identifies the last provider observation and does not make a network request to refresh it.

This diagnostic does not create or repair local state. If policy needs a governed audited status activity, an explicitly configured audited caller must capture the stdout receipt; an unavailable required wrapper is reported as unsupported.

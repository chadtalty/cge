# ADR-0004: Identity, conservative resume, and diagnostic status

- **Status:** Proposed; no command behavior is implemented.
- **Date:** 2026-10-19
- **Related:** [Issue #5](https://github.com/chadtalty/cge/issues/5), [lifecycle proposal](../architecture/activity-lifecycle.md), [MVP contract](../specification/runner-mvp.md)

## Context and options

An issue URL or branch name can change. Repeated invocations, process interruption and remote timeouts must not duplicate effects or reopen terminal attempts. A status query must not repair state or accidentally perform engineering work.

| Decision area | Alternatives | Recommendation and tradeoff |
| --- | --- | --- |
| Scope identity | Display names/paths; stable provider IDs | Stable provider/repository/request identity plus readable aliases; requires metadata resolution and retained observations |
| Duplicate commands | Always start again; compare a stable command identity/payload | Persist command ID and canonical payload digest; same payload returns/resumes prior result, conflicting payload is rejected |
| Resume | Optimistically repeat effects; reconcile first | Reconcile retained intents and actual effects; reacquire authority and exclusive claim; uncertainty blocks progress |
| Failed runs | Reopen old attempt; create linked successor | Explicit retry creates a new attempt after eligibility/effects checks; previous history remains terminal |
| Changed sealed inputs | Refresh automatically; require impact assessment | Block for impact assessment and governed rework; initial CLI reports unsupported rework tooling instead of altering bindings |
| Status audit | Mutate the target ledger on each read; classify a read-only diagnostic | Diagnostic stdout receipt without state writes; an audited activity wrapper is required when policy demands one |

## Recommendation

Use canonical request identity and at most one nonterminal attempt per activity instance. Serialise state changes with expected sequence/state and exclusive single-host execution claims. A new command identity alone does not authorize a new attempt or repeated external effects. Revalidate current grants at action time and on recovery; version pinning never extends a revoked permission.

`status` is an observation tool, not a governed engineering activity in this profile. It reads a retained local snapshot and reports its age and limitations; it does not change the checklist, lifecycle, claims or audit ledger. A separately governed caller can retain its receipt when required. The runner must reject a profile whose required audited-query wrapper is unavailable. This classification requires explicit review because the consolidated specification requires audit/checklist dispositions for every activity attempt.

Branch naming derives from stable identity. Use GitHub conventions only in that profile; preserve the Jira/GitLab `dci-1234` convention as a separate mapping. Colliding names, dirty checkouts, expired/revoked authority, uncertain effects and unverifiable process ownership produce blockers, not automatic cleanup.

## Consequences and acceptance

Conservative handling can require manual reconciliation, especially where remote outcomes or old executor termination cannot be proven. This is preferable to claiming successful resumed work from incomplete evidence. A later runner can add audited rework/cancel/query wrappers through separate bounded changes.

**Acceptance record:** Pending reviewer identity, evaluated full commit SHA, decision, time, rationale and evidence. Identity, authority/resume semantics and diagnostic classification must all be accepted before implementation.

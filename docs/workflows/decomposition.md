# Reference Workflow: Intake to Architecture-Approved Features

**Status:** Proposed manual-first reference implementation  
**Tooling:** GitHub (replaceable adapter)

## Intent

An intake/decomposition request is **not yet a feature or story**. An architect evaluates it and derives one or more features. A **feature** is a unit of work that is independently testable and deployable; it may span multiple repositories and contain one or more stories.

## Phases and activities

| Step | Phase | Bounded activity | Required evidence |
| --- | --- | --- | --- |
| 1 | Intake | Capture request, objectives, constraints, origin | GitHub intake issue |
| 2 | Architecture | Establish context and analyze request | Versioned context and analysis |
| 3 | Architecture | Decompose into one or more features | Linked feature issues, boundaries, dependencies |
| 4 | Architecture | Produce design and planning artifacts | Linked Markdown specifications/ADRs/plans |
| 5 | Architecture gate | Review feature readiness and artifacts | Review decision, evaluated versions, rationale |
| 6 | Implementation | Execute approved feature work | Linked branches/PRs/tests |
| 7 | Verification / delivery | Verify and deploy under subsequent gates | Test and deployment evidence |

## Reference workflow transitions

These are **reference workflow states**, not a universal CGE activity state machine. Advancement requires an authorized actor and the stated evidence. A failed prerequisite leaves the request in its current state; rejection explicitly routes to rework.

| From state | To state | Advancement condition | Required pinned evidence | Failure / rework destination |
| --- | --- | --- | --- | --- |
| Intake draft | Intake ready | Request has objective, origin, constraints, and accountable owner | Intake snapshot and context reference | Intake draft |
| Intake ready | Architecture in progress | Architect accepts scope and confirms required inputs are available | Accepted intake snapshot, context version, actor/time | Intake ready (request clarification) |
| Architecture in progress | Architecture review | Features have testable/deployable boundaries; analysis, design, planning, and dependencies are documented | Versioned intake/feature snapshots, context manifest, artifacts at a commit SHA | Architecture in progress |
| Architecture review | Ready for implementation | Authorized reviewer explicitly approves all architecture gate criteria against pinned versions | Gate decision, reviewer, timestamp, rationale, evaluated commit SHA and context version | Architecture rework if changes requested or rejected |
| Architecture review | Architecture rework | Reviewer requests changes or rejects with findings | Decision and findings referencing evaluated versions | Architecture rework |
| Architecture rework | Architecture review | Findings addressed, affected snapshots and artifacts updated, and a new review version submitted | New commit SHA, context version, change summary, links to prior findings | Architecture rework |
| Ready for implementation | Implementation in progress | Feature owner accepts approved feature scope and downstream prerequisites | Architecture gate approval and pinned feature version | Ready for implementation |

Changes to an approved feature's scope or acceptance boundaries invalidate the prior approval **for the changed scope** and require a new architecture review. Downstream implementation and delivery gates are intentionally out of scope for this reference workflow.

## Version pinning and review integrity

GitHub issue bodies are mutable. **Issue URLs provide live coordination and traceability, not immutable gate evidence.** Before requesting architecture review:

1. Capture the intake request and each feature definition (including acceptance boundaries) as Markdown snapshots in the repository, with source issue URLs and capture timestamps.
2. Capture a versioned context manifest referencing governing specifications, constraints, and assumptions.
3. Commit the snapshots, context manifest, analysis, design, and planning artifacts together on the review branch.
4. Have the reviewer evaluate a specific commit SHA. Record that SHA, the context version, the included snapshot paths, reviewer identity, timestamp, criteria results, and rationale in the gate decision.
5. If the source issue changes materially after snapshotting, refresh the snapshots and repeat the review. Never silently treat the live issue as the approved version.

A Git commit identifies repository content, but the approval itself must be recorded separately and linked to that commit. A PR review of an earlier commit must not be presumed to approve later changes.

## GitHub mapping

- **Intake:** Issue with a decomposition template and stable issue reference.
- **Features / stories:** Linked issues or sub-issues; use labels or issue types where configured.
- **Context and artifacts:** Versioned repository files and immutable-in-Git snapshots of mutable issue bodies, pinned by commit SHA and context version.
- **Activity execution:** Assigned work with scoped branch/PR and recorded outcomes.
- **Quality gate:** Explicit PR review or designated approval with checklist and evidence.
- **Audit evidence:** Issue events, commits, reviews, Actions results, plus a future normalized CGE event ledger.

GitHub provides tooling, **not** the CGE semantics automatically. In particular, a merged PR is not sufficient evidence of architectural approval unless the workflow declares and enforces that rule.

## Example: manual execution

1. Create intake Issue `#12` describing the requested capability.
2. Architect records the governing context and analyzes scope and constraints.
3. Architect creates linked feature issues `#13` and `#14`, each with independently testable and deployable acceptance boundaries.
4. Architect snapshots the intake and feature issue bodies into Markdown, records a context manifest, and commits these with analysis, design, and planning artifacts on a review branch.
5. Reviewer evaluates the exact commit SHA, pinned issue snapshots, and context version against architecture criteria.
6. On approval, the workflow records the gate decision with reviewed versions and marks features ready for implementation; on rejection, it enters architecture rework with findings. Resubmission requires a new pinned review version.

Numbers above are illustrative; no such example issues are claimed to exist.

## Architecture gate criteria

- [ ] Each feature has an independently testable and deployable outcome.
- [ ] Feature scope and cross-repository dependencies are identified.
- [ ] Relevant context, constraints, and assumptions are explicit.
- [ ] Intake and feature issue content are snapshotted and pinned alongside versioned analysis, design, and planning artifacts.
- [ ] Acceptance criteria and verification strategy are defined.
- [ ] Review authority, decision, rationale, reviewed commit SHA, context version, and snapshot paths are recorded.
- [ ] Rework paths and unresolved risks are identified.

## Audit events to capture

`intake.created`, `activity.started`, `context.versioned`, `feature.proposed`, `artifact.versioned`, `gate.evaluated`, `workflow.transitioned`.

An implementation should avoid inventing audit events from incomplete GitHub metadata. The event ledger is a future design item.

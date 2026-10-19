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

## GitHub mapping

- **Intake:** Issue with a decomposition template and stable issue reference.
- **Features / stories:** Linked issues or sub-issues; use labels or issue types where configured.
- **Context and artifacts:** Versioned repository files, referenced by commit SHA.
- **Activity execution:** Assigned work with scoped branch/PR and recorded outcomes.
- **Quality gate:** Explicit PR review or designated approval with checklist and evidence.
- **Audit evidence:** Issue events, commits, reviews, Actions results, plus a future normalized CGE event ledger.

GitHub provides tooling, **not** the CGE semantics automatically. In particular, a merged PR is not sufficient evidence of architectural approval unless the workflow declares and enforces that rule.

## Example: manual execution

1. Create intake Issue `#12` describing the requested capability.
2. Architect records the governing context and analyzes scope and constraints.
3. Architect creates linked feature issues `#13` and `#14`, each with independently testable and deployable acceptance boundaries.
4. Architect commits analysis, design, and planning artifacts on a review branch.
5. Reviewer evaluates the exact commit and linked feature issues against architecture criteria.
6. On approval, the workflow records the gate decision and marks features ready for implementation; on rejection, the work returns for rework with findings.

Numbers above are illustrative; no such example issues are claimed to exist.

## Architecture gate criteria

- [ ] Each feature has an independently testable and deployable outcome.
- [ ] Feature scope and cross-repository dependencies are identified.
- [ ] Relevant context, constraints, and assumptions are explicit.
- [ ] Analysis, design, and planning artifacts are versioned and linked.
- [ ] Acceptance criteria and verification strategy are defined.
- [ ] Review authority, decision, rationale, and artifact versions are recorded.
- [ ] Rework paths and unresolved risks are identified.

## Audit events to capture

`intake.created`, `activity.started`, `context.versioned`, `feature.proposed`, `artifact.versioned`, `gate.evaluated`, `workflow.transitioned`.

An implementation should avoid inventing audit events from incomplete GitHub metadata. The event ledger is a future design item.

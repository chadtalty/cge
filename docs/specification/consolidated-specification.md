# Context Governed Engineering — Consolidated Specification

**Status:** Proposed governing specification; not an implemented runtime.  
**Source:** Consolidated specification supplied by Chad in the project conversation on 2026-10-19 (America/New_York).  
**Capture:** Wording and qualifications retained; tables and lists formatted as Markdown. The source is an instruction/specification, not a recorded gate approval.  
**Review:** [Issue #5](https://github.com/chadtalty/cge/issues/5); see [alignment assessment](../analysis/specification-alignment.md) for proposed reconciliation with earlier documents.

## 1. Definition and purpose

Context Governed Engineering is a policy-governed, artifact-centric framework for defining, executing, verifying, and auditing software engineering work across the full SDLC.

CGE extends specification-driven development by governing:

- What context an activity receives.
- What work it may perform.
- Which artifacts it consumes and produces.
- What evidence establishes completion.
- How decisions, dependencies, and execution history are retained.

A human, deterministic program, AI agent, or combination can execute the work. The engineering contracts apply regardless of executor.

Its goals are to reduce architectural drift, reduce unnecessary context and token consumption, and improve accuracy through iteration.

## 2. Pillars

| Pillar | Principle |
| --- | --- |
| Governed Context | Supply the minimum sufficient authoritative context, bound to identified revisions. |
| Bounded Work | Define explicit scope, inputs, outputs, constraints, and completion criteria. |
| Architectural Alignment | Maintain consistency with requirements, standards, interfaces, and architectural decisions. |
| Iterative Verification | Produce, check, review, and refine using evidence. |
| Persistent Artifacts | Preserve engineering knowledge as versioned work products. |
| Traceability and Auditability | Record what governed the work, what happened, and what supports its outcome. |

## 3. Work hierarchy

For the Jira/GitLab process defined here:

| Element | Definition |
| --- | --- |
| Decomposition | The initial Jira request created by CE and assigned to an architect. |
| Feature | A testable, deployable unit of work derived from a decomposition. It may span multiple repositories. |
| Story | A subdivision of a feature used to organize implementation work. |
| Work package | A bounded, assignable portion of planned engineering work with explicit dependencies and completion criteria. |
| Activity | A governed engineering operation performed against a defined scope. |
| Activity run | One actual execution attempt, including failed, blocked, or interrupted attempts. |

The primary organizational hierarchy is:

**Decomposition → Features → Stories**

Work packages connect that hierarchy to executable work. Their precise mapping to stories is configurable.

## 4. Phases and activities

A phase determines which activities are eligible for a particular scope. Phases do not lock every feature or repository into the same state.

The lifecycle can cover:

| Phase | Representative activities |
| --- | --- |
| Intake | Capture request, initialize workspace, identify stakeholders |
| Analysis | Clarify requirements, assess current state, identify features and impacts |
| Design | Define architecture, interfaces, data models, and decisions |
| Planning | Define stories, work packages, dependencies, and verification strategy |
| Implementation | Change code, configuration, tests, and documentation |
| Verification and validation | Execute checks, assess behavior, review evidence |
| Release and deployment | Evaluate readiness, deploy, validate outcomes |
| Operations and evolution | Monitor, investigate, maintain, reassess changes |
| Retirement | Decommission capabilities and retain required records |

These are a configurable lifecycle vocabulary. Analysis, design, planning, and verification can iterate.

Each activity definition declares:

- Required inputs and applicable context.
- Permitted output artifact types.
- Responsible roles and eligible executors.
- Tools, permissions, and resource boundaries.
- Preconditions and dependency requirements.
- Verification and completion criteria.
- Audit requirements.

Phase eligibility alone does not establish readiness. Inputs, dependencies, authority, and resources must also be satisfied.

## 5. Artifacts

Artifacts are first-class inputs and outputs. CGE maintains an artifact type catalog from which activities select appropriate work products.

| Category | Examples |
| --- | --- |
| Analysis | Requirements, acceptance criteria, current-state assessment, impact assessment |
| Design | Architecture views, API contracts, data models, ADRs |
| Planning | Stories, work-package specifications, dependency plans, verification strategies |
| Implementation | Source code, configuration, migrations, tests |
| Verification | Test results, review findings, compatibility reports |
| Governance | Completion checklists, context manifests, decision records |
| Execution | Activity audits, publication receipts, release evidence |

The catalog is a bank of options. CGE does not require every artifact for every feature.

Artifacts have stable identities; revisions identify their exact state. Downstream work binds to identified revisions. An upstream change triggers impact assessment.

## 6. Context governance

CGE resolves context according to the command, activity, phase, and target scope.

A context package may include:

- Global engineering standards and policies.
- Decomposition and feature requirements.
- Relevant repository instructions.
- Required input artifacts.
- Accepted and proposed ADRs.
- Applicable templates and tool contracts.

A context manifest records the sources and revisions used. A mutable branch name or symlink makes content accessible but does not establish a reproducible baseline.

The orchestrator’s global instructions should remain small. Detailed context is loaded when an activity needs it.

## 7. Agents, tools, and controller

| Component | Responsibility |
| --- | --- |
| Orchestrator agent | Interpret intent, coordinate specialists, analyze findings, and explain outcomes |
| Engineering specialists | Perform focused analysis, design, planning, implementation, or review |
| Integration agents | Navigate external systems and perform scoped operations |
| Tool library | Perform repeatable operations and return structured evidence |
| Execution controller | Enforce contracts, transitions, dependencies, and completion requirements |

Existing Jira and GitLab agents provide integration capabilities. Potential engineering specialists include requirements analyst, architect, planner, implementation agent, test engineer, security reviewer, and architectural reviewer.

Tools can resolve context, scaffold artifacts, validate schemas, calculate dependency readiness, run checks, and record execution evidence.

Tool contracts describe inputs, outputs, effects, permissions, failures, and retry safety. Scripted execution is not necessarily deterministic: external queries and flaky tests require their observations and environment to be recorded.

## 8. ADRs and completion checklists

ADRs and completion checklists are first-class, evolving governance artifacts.

Every activity reviews the applicable decisions and checklist, then records an update or a justified no-change/not-applicable disposition.

When an agent identifies a gap, it follows Provisional Decision Management:

1. Record the gap in the checklist.
2. Document viable options and tradeoffs in an ADR.
3. Recommend an option with status Proposed.
4. Link affected artifacts and dependent work.
5. Continue work permitted by policy.

Track these dimensions separately:

| Dimension | Example states |
| --- | --- |
| Work | Open, In Progress, Done, Blocked |
| Decision | Not Required, Proposed, Accepted, Rejected, Superseded |
| Verification | Not Assessed, Passed, Failed |

Done — Proposed means the work is done while its supporting decision remains proposed.

Selecting an option does not constitute acceptance. Required decisions remain gate conditions. Rejected or revised decisions trigger reassessment of dependent work.

## 9. Dependencies and cross-repository work

CGE maintains three distinct structures:

| Structure | Purpose |
| --- | --- |
| Work breakdown | Describe scope and deliverables |
| Dependency graph | Determine prerequisites and eligible execution order |
| Traceability graph | Connect requests, requirements, decisions, changes, and evidence |

Shared work has one canonical identity even when several features or decompositions depend on it.

Execution waves are derived from ready work. They are scheduling results, separate from lifecycle phases. Readiness can require a particular artifact revision, accepted decision, published package, or verified result.

## 10. Audit requirements

Every activity attempt must record:

- Execution identity, scope, actor, and timestamps.
- Activity definition and framework revision.
- Inputs and context references.
- Actual outputs and changes.
- Tool outcomes and verification evidence.
- Assumptions and unresolved gaps.
- Decision references.
- Confidence assessment and rationale.
- Execution outcome and audit publication status.

Observed evidence, agent declarations, and authorized decisions remain distinguishable.

Confidence measures evidence support; it is not a calibrated probability of correctness. Work coverage, verification coverage, and acceptance readiness are reported separately.

Audit recording begins before substantive execution. Required records must be retained before the activity is marked complete. Local success and remote publication are separate outcomes.

## 11. Quality gate

The decomposition quality gate reviews Jira features and their associated artifacts.

The review evaluates:

- Scope and requirements clarity.
- Testability and deployability.
- Architectural alignment.
- Implementation and dependency readiness.
- Verification strategy.
- ADR disposition and unresolved gaps.
- Checklist completeness and supporting evidence.

Acceptance applies to identified revisions. Review authority and whether individual features may pass independently remain configurable process decisions.

## 12. Commands and integration

Action commands provide a recognizable interface:

```text
/cge:init DCI-1234
/cge:analyze DCI-1234
/cge:design <feature-key>
/cge:plan <feature-key>
/cge:review <feature-key>
/cge:implement <work-package-id>
/cge:verify <scope-id>
/cge:status DCI-1234
/cge:close <scope-id>
```

These are proposed interfaces. An adapter maps them to the execution environment.

OpenCode initially provides developer interaction and agent execution. A CLI and pipeline adapter can invoke the same underlying contracts. Jira/GitLab are the integration profile described here; CGE’s core model remains portable.

## How it works in practice

The architect enters:

```text
/cge:init DCI-1234
```

CGE retrieves the Jira decomposition, prepares the SDK checkout, creates or resumes branch `dci-1234`, initializes the request snapshot and checklist, and records the activity.

The architect then requests analysis. CGE loads relevant standards and system context, coordinates analysis, and produces candidate features and supporting artifacts. Gaps become checklist items and proposed ADRs.

Design and planning refine those features into reviewable engineering packages, stories, and repository work. The quality gate reviews the Jira features together with their artifacts.

Implementation consumes the reviewed inputs. Tools generate evidence; agents interpret results and iterate. The controller evaluates completion against explicit rules. Subsequent release, deployment, and operational activities follow the same pattern:

**Resolve inputs → check eligibility → load context → execute → verify → retain evidence → report the resulting state.**

This is the proposed CGE specification. The saved reference documents define the architecture and contracts; they do not establish that a complete execution runtime has been implemented.

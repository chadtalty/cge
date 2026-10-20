# Consolidated Specification Alignment

**Status:** Proposed analysis for [Issue #5](https://github.com/chadtalty/cge/issues/5).  
**Baseline inspected:** `chadtalty/cge@c514957c632f00995bbc817037e26cf3e97f40c7`, containing merged PRs #2 and #4.  
**Candidate requirements:** [Consolidated specification](../specification/consolidated-specification.md).

## Finding and proposed precedence

The consolidated specification preserves the six core abstractions and execution independence, but adds requirements not fully expressed in the merged conceptual documents. It also describes a Jira/GitLab organizational profile, whereas this repository uses a GitHub reference workflow. These profiles can coexist without changing the core model.

Proposed interpretation order after explicit acceptance: accepted policies/decisions for the target scope; the accepted consolidated specification; compatible detailed lifecycle/audit contracts; the selected integration profile; activity-specific bindings. A narrower document cannot silently weaken a higher-level requirement. Conflicts block affected work and require a recorded decision. Until review acceptance, these additions remain candidate requirements; merge history alone does not settle precedence or accept ADRs.

This package adds an explicit candidate layer instead of rewriting the historical v0.1/v0.2 proposals as though they had always included these requirements.

## Alignment matrix

| Consolidated section | Merged baseline | Gap or difference | Proposed disposition |
| --- | --- | --- | --- |
| 1–2: purpose and pillars | Definition and nine core principles | Goals include minimal sufficient context and token economy; pillars are a presentation of principles, not replacement abstractions | Preserve six abstractions; use pillars as goals and evaluation criteria; do not claim measured savings |
| 3: hierarchy | Intake → feature → stories in reference workflow | Work packages and activity runs need explicit mapping | Work package is planning scope; activity is governed operation; run is one attempt; mappings are configurable |
| 4: lifecycle | Configurable SDLC phases, six attempt states | Extended phase vocabulary and per-scope eligibility | Keep phases, work states, and attempt states separate; phase eligibility is only one readiness input |
| 5: artifact catalog | Versioned artifacts with examples | No explicit optional type catalog | Add an MVP allowlist of output types; defer complete catalog implementation; never require every artifact type |
| 6: context | Versioned context and sealed manifests | Need bounded loading and source-selection rationale | Record selected/excluded sources and why; include repository instructions and both proposed/accepted ADRs with statuses |
| 7: controller and tools | Execution-independent core and replaceable adapters | No runner or tool-contract implementation | Design a deterministic control path around observed results; do not claim external operations are deterministic |
| 8: ADR/checklist discipline | Proposed ADR and gate conditions | No per-activity update/no-change/N/A rule or three-dimensional status | Require explicit dispositions; separate work, decision, verification and gate readiness |
| 9: dependency structures | Workflow dependencies and traceability | Breakdown, readiness graph, execution waves need separation | MVP checks declared prerequisites for `init`; no wave scheduler or general dependency engine |
| 10: audit | Proposed envelope, event types, persistence and ordering | Framework revision, assumptions/gaps, confidence and publication need explicit run-summary treatment | Add candidate run-summary fields linked to events; local-retention MVP policy is explicit; remote-required completion is unsupported initially |
| 11: decomposition gate | GitHub architecture gate | Jira feature gate, independent feature acceptance and review authority configurable | Keep provider-specific mappings; no gate acceptance or feature advancement from `init` |
| 12: integration | GitHub manual workflow; no CLI | OpenCode commands and Jira/GitLab SDK initialization are proposed | CLI is the first runner interface for the GitHub profile; OpenCode and Jira/GitLab adapters remain future work |

## Terminology and state mapping

| Supplied term | Existing term / proposed interpretation |
| --- | --- |
| Activity run | Execution attempt (`attempt_id` remains the proposed internal field); one run can fail, wait, or be interrupted |
| Blocked/interrupted run | Condition plus reason on a nonterminal attempt; not new universal attempt states; a stopped/fenced execution may transition to `failed` under its contract |
| Work status | Open / In Progress / Done / Blocked for work scope; does not replace attempt states |
| Done — Proposed | Work Done, required decision Proposed; acceptance readiness blocked until the decision is accepted where required |
| Execution controller / runner | Policy enforcement and evidence coordination; not necessarily an AI agent |
| Orchestrator | Optional agent interpreting intent and coordinating bounded activities; cannot override controller policy |
| SDK checkout | Governed framework/standards material distinct from the target work repository; GitHub MVP pins a framework checkout separately |
| Execution wave | Derived scheduling set; not an SDLC phase or an attempt state |

## Required run-summary additions

The existing audit envelope remains the event-level proposal. The runner additionally needs a versioned run summary binding: framework commit; activity definition and execution manifest; environment/tool observations; actual outputs/effects; assumptions and gaps; ADR/checklist dispositions; qualitative confidence with rationale and evidence; work and verification coverage; acceptance readiness; and publication attempts/receipts.

The summary is a rebuildable projection of retained evidence and explicit declarations. It must distinguish observed facts, executor declarations, and authorized decisions. Confidence is evidence support, not a probability or substitute for verification. Unknown coverage stays unknown; no synthetic score is inferred from a successful command.

## Proposed MVP choices requiring acceptance

- [ADR-0002](../decisions/0002-runner-cli-language.md): Python CLI and standard-library core; no model dependency.
- [ADR-0003](../decisions/0003-local-state-and-evidence.md): SQLite state/events, immutable content-addressed evidence, local-retention completion and separate publication.
- [ADR-0004](../decisions/0004-runner-identity-and-resume.md): canonical request identity, conservative resume, explicit authority, and read-only `status` classification.

Classifying `status` as a diagnostic tool is an explicit design choice: it emits a read receipt but does not perform a governed activity or mutate the request ledger. If policy requires an audited status activity, a separately audited caller must wrap it; the first runner must reject a profile requiring an unimplemented wrapper. This is a review point, not an exception silently assumed for every CGE command.

## Dependency and implementation boundary

Issues #1 and #3 remain open in the inspected baseline; their checklist/acceptance records must be resolved or explicitly incorporated into the design gate by the authorized reviewer. The design package can be written and reviewed while required decisions remain Proposed. Implementing the chosen architecture requires those decisions and precedence rules to be accepted against pinned revisions.

The current repository contains Markdown contracts and examples. It contains no installed runner, invocation agent, complete schema catalog, authorization service, or ledger implementation. The supplied references to existing Jira/GitLab agents describe the user's integration environment, not code found in this repository.

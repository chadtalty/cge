# CGE Core Principles v0.1

**Status:** Proposed

1. **Context governed.** Every execution has explicit, identifiable, versioned context and constraints. Implicit knowledge must not silently become authoritative.
2. **Execution independent.** Human, AI, scripted, and hybrid executors operate against the same engineering contracts. Automation is optional.
3. **Bounded activities.** Each activity has a clear objective, scope, permitted actions, inputs, outputs, and stopping conditions.
4. **Deterministic workflow governance.** States, transitions, prerequisites, failure paths, and required evidence are defined. Engineering judgment and generated content need not be deterministic.
5. **Artifact based.** Engineering outcomes and evidence are identifiable, versioned, reviewable artifacts. Gates evaluate pinned versions.
6. **SDLC phase bounded.** Lifecycle phases and handoffs are explicit, with governance at appropriate boundaries; phase definitions may be tailored.
7. **Quality gated.** Progression requires defined acceptance criteria, evidence, and authorized decisions. A check result is not automatically an approval.
8. **Auditable and traceable.** Significant actions, decisions, and transitions create append-only audit events, linked through stable identifiers to their causes and outcomes.
9. **Evolvable by design.** Context, contracts, workflows, and decisions may change through controlled, versioned revisions. Historical evidence remains attributable to the version in effect.

## Non-goals

- Requiring AI, a particular agent framework, or a particular development tool.
- Guaranteeing identical design or code from different executors.
- Treating Git history alone as a complete audit system.
- Prescribing one universal SDLC, issue hierarchy, or organizational structure.

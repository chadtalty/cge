# ADR-0001: Execution-independent CGE core

- **Status:** Proposed
- **Date:** 2026-10-19
- **Related:** [Issue #1](https://github.com/chadtalty/cge/issues/1)

## Context

CGE extends specification-driven practices across the SDLC. Defining CGE as an AI orchestration platform would unnecessarily couple engineering governance to a particular executor or tool stack.

## Proposed decision

Define CGE as a technology-independent engineering abstraction with six core concepts: Context, Activity, Artifact, Workflow, Quality Gate, and Audit Record. Treat traceability and governance as cross-cutting properties. Implement human, agentic, and hybrid execution through replaceable adapters.

Use GitHub as the first reference implementation, without making it part of the conceptual contract.

## Consequences

**Benefits:** Manual adoption remains possible; workflow semantics can be tested before automation; implementation technologies can change without redefining core governance.

**Costs:** Tool-native issue, PR, and commit histories do not automatically satisfy CGE's unified context, gate, or audit contracts; mapping and validation are needed.

## Alternatives considered

1. **AI-first framework:** Rejected for the conceptual core because it excludes manual execution.
2. **GitHub-specific process:** Rejected as the core model because it couples CGE semantics to one provider.
3. **Pure SDD convention set:** Insufficient for the proposed explicit execution, lifecycle, and audit contracts.

## Review criteria

Approve only if manual and automated execution can satisfy the same activity contract and if audit and gate evidence remain attributable to exact artifact/context versions.

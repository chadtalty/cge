# Context Governed Engineering (CGE)

**An execution-independent, specification-driven engineering framework.**

Context Governed Engineering (CGE) extends Specification-Driven Development (SDD) with explicit governance of engineering work across the software development lifecycle (SDLC). Work is defined by context, bounded activities, versioned artifacts, controlled workflows, quality gates, and auditable events.

CGE defines **what constitutes governed engineering work**, independently of **who or what performs it**. Humans, AI agents, scripts, and hybrid teams can execute the same activity contract.

## Status

**Conceptual architecture v0.1 — proposed, under review.** This is an evolving reference model, not a claim of a complete implementation.

## Start here

- [Conceptual model](docs/architecture/conceptual-model.md)
- [Core principles](docs/principles/core-principles.md)
- [Manual GitHub decomposition workflow](docs/workflows/decomposition.md)
- [ADR-0001: Execution-independent core](docs/decisions/0001-execution-independent-core.md)
- [Example decomposition request](examples/decomposition-request/README.md)

## Scope

CGE's core model is tool-agnostic. GitHub Issues, Projects, repositories, pull requests, and Actions are used as an initial **reference implementation**, not as mandatory CGE dependencies.

## Development approach

CGE is developed using its own principles: issues capture requests, branches bound changes, Markdown files represent reviewable artifacts, pull requests provide review boundaries, and commits and review events provide initial audit evidence. Native GitHub history alone is **not** a complete CGE audit ledger.

See [Issue #1](https://github.com/chadtalty/cge/issues/1) for the v0.1 architecture quality gate.

# ADR-0002: Initial runner language and interface

- **Status:** Proposed; no implementation choice has been accepted.
- **Date:** 2026-10-19
- **Related:** [Issue #5](https://github.com/chadtalty/cge/issues/5), [MVP specification](../specification/runner-mvp.md)

## Context and options

The first runner must be easy to inspect, execute without an AI service, and exercise through a CLI before adding OpenCode or pipeline adapters. The core needs command parsing, structured data, hashing, filesystem operations, SQLite, and scoped provider/Git integrations.

| Option | Benefits | Costs |
| --- | --- | --- |
| Python CLI | Small readable control path; standard-library building blocks; convenient local checks | Requires a Python interpreter and a later packaging/support policy |
| TypeScript/Node CLI | Familiar agent-tool ecosystem and typed interface conventions | Introduces a package/build toolchain and dependency choices before the small control path is proven |
| Shell scripts | Quick local prototype and direct Git invocation | Harder structured state, recovery, cross-platform quoting, and contract validation |

## Recommendation

Use a Python CLI with a standard-library core, targeting Python 3.12 or later as a proposed minimum. Use Git as an explicitly checked external tool. Define provider and storage interfaces separately from command parsing; later OpenCode/pipeline adapters call the same controller. GitHub authentication transport, packaging and supported operating systems must be verified in the implementation work package, not inferred from the user's laptop configuration.

The language is a proposed adapter/implementation choice, not part of CGE's portable conceptual model. Do not add an agent framework or model API dependency to this slice.

## Consequences and acceptance

This keeps the initial executable surface small, but does not remove the need to design adapter failures, permission boundaries, or executable schema validation. Before implementation, the owner must accept the language, supported environment, Git prerequisite, and packaging approach. Until then, design artifacts may be complete while this decision remains Proposed.

**Acceptance record:** Pending reviewer identity, evaluated full commit SHA, decision, time, rationale and evidence. Selection of this recommendation is not acceptance.

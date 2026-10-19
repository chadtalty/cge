# Example: Decomposition Request

**Status:** Illustrative example; not a live GitHub issue.

## Request

A client needs a new query capability with clearly defined authorization, query behavior, and operational limits.

## Context

- Functional requirements and expected users must be documented.
- Security, performance, and operational constraints must be identified.
- The architect may derive multiple features, including work across repositories.

## Architecture activity

**Objective:** Decompose the request into independently testable and deployable features.

**Inputs:** Intake request and context manifest.  
**Outputs:** Feature definitions, analysis, design, planning artifacts.  
**Boundary:** Architecture only; no implementation or deployment.  
**Gate:** Architecture readiness review with pinned artifact versions.

## Traceability sketch

`INTAKE-001 → ACT-DECOMPOSE-001 → FEATURE-001 / FEATURE-002 → design artifacts → GATE-ARCH-001 → audit event(s)`

This example will be refined as CGE's identity, context, and audit schemas evolve.

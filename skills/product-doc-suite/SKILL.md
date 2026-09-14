---
name: product-doc-suite
description: Use when generating a full product documentation pack (System Architecture Document, Use Cases & Capability Matrix, Data Contracts & I/O Schemas, and Viability Scorecard) derived from an approved PRD.
---

# Product Documentation Suite Generator

## Overview

The `product-doc-suite` skill ingests a validated, approved `PRD.md` and deterministically derives four modular, comprehensive engineering documents in `docs/`:
1. `docs/architecture.md` (System Architecture Document / SAD with STRIDE Threat Model)
2. `docs/use-cases.md` (Capability Matrix, Persona Stories, UI/UX Wireframes, and SPIDR Slicing)
3. `docs/contracts.md` (Data Schemas, RBAC Permission Tables, and Interface Specifications)
4. `docs/viability.md` (Business Case, Latency SLO Budgets, and Zero-Downtime Rollout Runbooks)

## When to Use

- `PRD.md` has passed quality audit (`product-audit`).
- User invokes `/product-doc-suite`.
- Preparing documentation handoff for engineering teams or coding subagents.

---

## Output Document Mapping

| Source PRD Section | Target Document | Derived Content |
|---|---|---|
| Section 5 & 7.2 (Architecture & Security) | `docs/architecture.md` | In-depth SAD, system touchpoints, Mermaid diagrams, failure domains, STRIDE threat mitigations. |
| Section 1, 3.2 & 4 (Problem, Scope & Use Cases) | `docs/use-cases.md` | Persona profiles, capability matrix, SPIDR vertical MVP slicing, Gherkin specs, ASCII wireframes. |
| Section 6 & 7.2 (Contracts & RBAC) | `docs/contracts.md` | JSON Schemas, OpenAPI endpoints, event payload definitions, RBAC role-permission matrices. |
| Section 2, 7.1 & 7.3 (Business, NFRs & Rollout) | `docs/viability.md` | ROI calculations, p95/p99 latency SLO budgets, zero-downtime expand/contract runbook, rollback triggers. |

---

## Execution Workflow

1. **Verify Source Integrity**: Check that `PRD.md` exists and has an approved status or 0 fatal placeholders.
2. **Generate Architecture Spec**: Extract Section 5 & 7.2 and write `docs/architecture.md`.
3. **Generate Use Cases & UI Spec**: Extract Section 1, 3.2, 4.1 & 4.2 and write `docs/use-cases.md`.
4. **Generate Contracts & RBAC Spec**: Extract Section 6 & 7.2 and write `docs/contracts.md`.
5. **Generate Viability & Rollout Spec**: Extract Section 2, 7.1, 7.3 & 7.4 and write `docs/viability.md`.
6. **Emit Standalone Diagrams**: Export `.mmd` files to `docs/diagrams/`.
7. **Generate/Update Index**: Update `docs/README.md` with links to all generated files.

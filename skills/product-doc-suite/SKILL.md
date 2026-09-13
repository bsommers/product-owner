---
name: product-doc-suite
description: Use when generating a full product documentation pack (System Architecture Document, Use Cases & Capability Matrix, Data Contracts & I/O Schemas, and Viability Scorecard) derived from an approved PRD.
---

# Product Documentation Suite Generator

## Overview

The `product-doc-suite` skill ingests a validated, approved `PRD.md` and deterministically derives four modular, comprehensive engineering documents in `docs/`:
1. `docs/architecture.md` (System Architecture Document / SAD)
2. `docs/use-cases.md` (Capability Matrix & Persona Stories)
3. `docs/contracts.md` (Data Schemas & Interface Specifications)
4. `docs/viability.md` (Business Case & Viability Scorecard)

## When to Use

- `PRD.md` has passed quality audit (`product-audit`).
- User invokes `/product-doc-suite`.
- Preparing documentation handoff for engineering teams or coding subagents.

---

## Output Document Mapping

| Source PRD Section | Target Document | Derived Content |
|---|---|---|
| Section 5 (Architecture & Topology) | `docs/architecture.md` | In-depth SAD, system touchpoints, Mermaid diagrams, failure domains. |
| Section 1 & 4 (Problem & Use Cases) | `docs/use-cases.md` | Persona profiles, capability matrix, Gherkin acceptance criteria. |
| Section 6 (Contracts & I/O) | `docs/contracts.md` | JSON Schemas, OpenAPI endpoints, event payload definitions. |
| Section 2 & 7 (Business & Viability) | `docs/viability.md` | ROI calculations, risk mitigation matrix, compliance checklist. |

---

## Execution Workflow

1. **Verify Source Integrity**: Check that `PRD.md` exists and has an approved status or 0 fatal placeholders.
2. **Generate Architecture Spec**: Extract Section 5 and write `docs/architecture.md`.
3. **Generate Use Cases Spec**: Extract Section 1 & 4 and write `docs/use-cases.md`.
4. **Generate Contracts Spec**: Extract Section 6 and write `docs/contracts.md`.
5. **Generate Viability Spec**: Extract Section 2 & 7 and write `docs/viability.md`.
6. **Emit Standalone Diagrams**: Export `.mmd` files to `docs/diagrams/`.
7. **Generate/Update Index**: Update `docs/README.md` with links to all generated files.

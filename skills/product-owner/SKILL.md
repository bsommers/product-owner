---
name: product-owner
description: Use when ingesting meeting notes, transcripts, or raw product requirements, orchestrating the end-to-end PRD lifecycle, driving Socratic product refinement, generating product documentation packs, or auditing product specifications.
---

# Product Owner (Umbrella Meta-Skill)

## Overview

The `product-owner` meta-skill acts as the central router and orchestrator for turning raw, unstructured product inputs (meeting minutes, stakeholder transcripts, brainstorm notes) into a complete, grounded, production-grade product specification and documentation suite.

## The Product Owner Pipeline

```mermaid
flowchart TD
    RawInput["Raw Input\n(Meeting Minutes, Transcripts, Notes)"] --> S1["raw-to-prd\n(1. Ingestion & Baseline PRD)"]
    S1 --> PRD["PRD.md\n(Living Spec with [UNKNOWN] Tags)"]
    PRD --> S2["prd-refine\n(2. Socratic 1-at-a-Time Refinement)"]
    S2 <--> User["Human Stakeholder / Architect"]
    S2 -->|Patch & Log Decision| PRD
    PRD --> S4["product-audit\n(3. Quality Gate & SCI Score)"]
    S4 -->|Audit Passed| S3["product-doc-suite\n(4. Full Documentation Suite)"]
    S4 -.->|Gaps Found| S2

    subgraph DocPack ["Derived Product Documentation (docs/)"]
        S3 --> SAD["docs/architecture.md"]
        S3 --> UC["docs/use-cases.md"]
        S3 --> IO["docs/contracts.md"]
        S3 --> BIZ["docs/viability.md"]
    end
```

---

## Sub-Skill Delegation Routing

When handling product owner workflows, delegate to the specialized sub-skills based on the user's intent or current document state:

| Intent / State | Command / Action | Delegate To |
|---|---|---|
| Ingest meeting minutes, notes, transcripts, or audio notes | `/raw-to-prd` | `skills/raw-to-prd` |
| Resolve open `[UNKNOWN]` placeholders via interactive interview | `/prd-refine` | `skills/prd-refine` |
| Validate completeness, calculate SCI, and audit quality | `/product-audit` | `skills/product-audit` |
| Generate complete `docs/` pack from approved `PRD.md` | `/product-doc-suite` | `skills/product-doc-suite` |

---

## Core Invariants

1. **Zero Unverified Invention**: When raw input lacks specific technical details (auth providers, DB engines, latency SLAs), NEVER guess. Always flag with `[UNKNOWN: ...]` or `[DECISION NEEDED: ...]`.
2. **Atomic Socratic Interviews**: Refine open questions one at a time with clear context, options, and recommended defaults.
3. **Immutable Audit Trail**: Every resolved decision must be recorded in Section 9 (*Decision Log & Audit Trail*) of `PRD.md`.
4. **Single Source of Truth**: All auxiliary documentation in `docs/` must strictly derive from `PRD.md` without divergence.

---

## Command Shortcuts

- `/product-owner [input]` — Runs end-to-end pipeline (ingest → check completeness → prompt next step).
- `/raw-to-prd [input]` — Synthesizes baseline `PRD.md` from input text.
- `/prd-refine` — Starts interactive Socratic question loop on open placeholders in `PRD.md`.
- `/product-audit` — Runs quality audit, computes SCI, and generates `AUDIT-PRD.md`.
- `/product-doc-suite` — Derives SAD, Use Cases, Contracts, and Viability docs into `docs/`.

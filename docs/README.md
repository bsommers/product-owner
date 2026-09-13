# Product Owner Skill Suite Documentation

Welcome to the documentation suite for the **Product Owner Skill Suite (Raw-to-PRD & Product Documentation Engine)**.

This system bridges the gap between messy, unstructured meeting minutes, interview transcripts, or raw notes and production-grade, actionable product specifications and documentation packs.

---

## Documentation Index

| Document | Description | Key Contents |
|---|---|---|
| [System Architecture](file:///home/bill/src/ai/product-owner/docs/architecture.md) | Full architectural blueprint of the skill pipeline and engine | Component topology, 7-facet engine, pipeline flow, ASCII/Mermaid models |
| [System Interactions](file:///home/bill/src/ai/product-owner/docs/interactions.md) | Interaction protocols between users, agents, and upstream systems | Socratic interview protocol, multi-system integration touchpoints, sequence flows |
| [Use Cases & Capabilities](file:///home/bill/src/ai/product-owner/docs/use-cases.md) | Actor journeys, functional requirements, and edge case specifications | Intake workflows, conflict resolution, interview progression, Gherkin specs |
| [Data Contracts & Schemas](file:///home/bill/src/ai/product-owner/docs/contracts.md) | Exact schema definitions for PRD, placeholder syntax, and derived documents | `PRD.md` format, `[UNKNOWN]` taxonomy, SAD/Contracts/Viability schema contracts |
| [Viability & Quality Gate](file:///home/bill/src/ai/product-owner/docs/viability.md) | Quality gates, Spec Completeness Index (SCI), and business viability scoring | SCI formula, 5-axis audit rubric, ROI projection, pass/fail gating |
| [Diagram Library](file:///home/bill/src/ai/product-owner/docs/diagrams/) | Standalone `.mmd` diagram files | Architecture, sequence, data flow, state lifecycle, system boundaries |

---

## System Architecture at a Glance

### ASCII Architecture Overview

```text
+-----------------------------------------------------------------------------------+
|                                  INPUT SOURCES                                    |
|   +-------------------+   +--------------------+   +--------------------------+   |
|   | Raw Meeting Notes |   | Audio Transcripts  |   | /meeting-notes Output    |   |
|   +---------+---------+   +---------+----------+   +------------+-------------+   |
+-------------|-----------------------|---------------------------|-----------------+
              |                       |                           |
              +-----------------------v---------------------------+
                                      |
+-------------------------------------v---------------------------------------------+
|                         PRODUCT OWNER PIPELINE ENGINE                             |
|                                                                                   |
|  +-----------------------------------------------------------------------------+  |
|  | [1] raw-to-prd: 7-Facet Analysis & Grounding                                |  |
|  | - Extracts explicit facts                                                   |  |
|  | - Flags gaps as [UNKNOWN: ...] / [DECISION NEEDED: ...]                     |  |
|  | - Generates Mermaid architecture & sequence diagrams                       |  |
|  +-------------------------------------+---------------------------------------+  |
|                                        | emits baseline                           |
|                                        v                                          |
|  +-----------------------------------------------------------------------------+  |
|  | [2] Living PRD: Master Contract (PRD.md)                                    |  |
|  | - Living source of truth with embedded placeholder registry & audit trail   |  |
|  +-------------------------------------+---------------------------------------+  |
|                                        | feeds into                               |
|                                        v                                          |
|  +-----------------------------------------------------------------------------+  |
|  | [3] prd-refine: Socratic Interactive Refinement Loop                        |  |
|  | - Prioritizes unknowns -> Asks 1 targeted question at a time                |  |
|  | - Patches PRD.md in-place -> Records decision in audit log                  |  |
|  +-------------------------------------+---------------------------------------+  |
|                                        | freezes when SCI = 100%                  |
|                                        v                                          |
|  +-----------------------------------------------------------------------------+  |
|  | [4] product-audit: Quality Gate & Validation                                |  |
|  | - Evaluates Spec Completeness Index (SCI) & cross-diagram consistency       |  |
|  +-------------------------------------+---------------------------------------+  |
|                                        | emits AUDIT-PRD.md (PASS)                |
|                                        v                                          |
|  +-----------------------------------------------------------------------------+  |
|  | [5] product-doc-suite: Derived Product Documentation Pack                   |  |
|  | - docs/architecture.md  - docs/use-cases.md                                 |  |
|  | - docs/contracts.md     - docs/viability.md                                 |  |
|  +-----------------------------------------------------------------------------+  |
+-----------------------------------------------------------------------------------+
```

### Inline Mermaid Architecture

```mermaid
flowchart TD
    Inputs["Raw Input\n(Meeting Minutes / Transcripts)"] --> S1["1. raw-to-prd\n(Grounding & 7-Facet Extraction)"]
    S1 --> PRD["PRD.md\n(Master Living Spec)"]
    PRD --> S2["2. prd-refine\n(Socratic 1-at-a-Time Interview)"]
    S2 <--> User["Stakeholder / Engineer"]
    S2 -->|In-Place Patch| PRD
    PRD --> S4["3. product-audit\n(Quality Gate & SCI Calculation)"]
    S4 -->|Audit PASS| S3["4. product-doc-suite\n(Derive Documentation Pack)"]
    S4 -.->|Audit FAIL| S2

    subgraph DocPack ["Generated Documentation Suite (docs/)"]
        S3 --> SAD["architecture.md\n(SAD & Topologies)"]
        S3 --> UC["use-cases.md\n(Capability Matrix)"]
        S3 --> IO["contracts.md\n(I/O & Schemas)"]
        S3 --> BIZ["viability.md\n(ROI & Risk Scorecard)"]
    end
```

---

## Core Guiding Principles

1. **Grounding Over Hallucination**: No technical stack, data model, or business SLA is invented without explicit evidence in raw notes.
2. **Actionable Placeholders**: All missing details are formatted as structured `[UNKNOWN: ...]` markers that double as interview prompts.
3. **Progressive Refinement**: A PRD is not a static one-off dump; it matures through iterative Socratic dialogue until reaching 100% Spec Completeness.
4. **Synchronized Documentation**: Downstream architectural, interface, and viability specifications are strictly derived from the approved PRD.

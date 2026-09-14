---
name: raw-to-prd
description: Use when transforming raw meeting minutes, audio transcripts, unformatted brainstorm notes, or /meeting-notes outputs into a structured, grounded baseline PRD with Mermaid diagrams and explicit unknown placeholders.
---

# Raw-to-PRD Synthesizer

## Overview

The `raw-to-prd` skill ingests unstructured product meeting minutes, notes, transcripts, or upstream summaries and extracts the 7 core facets into an implementation-ready baseline `PRD.md`. It enforces **Grounding Over Hallucination**: missing technical decisions, NFRs, and schemas are explicitly captured as `[UNKNOWN: ...]` placeholders.

## When to Use

- User supplies meeting notes, minutes, or transcripts to turn into a product spec.
- User invokes `/raw-to-prd`.
- Ingesting output from `/meeting-notes` or audio transcription tools.

---

## The 7 Core Facets & Extended Specifications

```text
1. Problem Space & Utility    (Personas, Pain points, Value prop, Workaround/Buy-vs-Build table)
2. Business Value & KPIs       (Target ROI, Financial metrics, Milestones, Success criteria)
3. Scope & SPIDR MVP Slicing   (In-scope, Out-of-scope, Spike/Paths/Interfaces/Data/Rules slicing)
4. Use Cases & UI/UX Facet     (Actor journeys, Gherkin specs, ASCII wireframes, Field validation matrix)
5. Systems Touched & Topology  (Mermaid topology, internal DBs, 3rd party APIs, Sequence flows)
6. Data Contracts & I/O        (Input JSON schemas, Output models, Error models, Event schemas)
7. Viability, NFRs & Rollout   (SLO/SLA latency budgets, STRIDE/RBAC security, Expand/Contract runbook)
```

---

## Execution Workflow

1. **Ingest & Parse**: Scan raw text for explicit facts, decisions, debates, UI screen mentions, performance expectations, and unstated technical parameters.
2. **Apply Zero-Hallucination Filter**:
   - If a technology, NFR, or schema parameter is named $\rightarrow$ extract it.
   - If omitted or ambiguous $\rightarrow$ create a formatted placeholder:
     ```markdown
     > [!NOTE] UNRESOLVED ITEM
     > **[UNKNOWN: <CATEGORY> - <TOPIC>]**
     > - **Source Context**: <Context from raw notes>
     > - **Options Identified**: <Options discussed or left blank>
     > - **Impact**: <What downstream engineering task is blocked>
     > - **Interview Prompt**: "<Actionable Socratic question>"
     ```
3. **Synthesize Specialized Sections**:
   - **UI/UX & Wireframes**: Generate ASCII layouts and field validation tables if user screens are discussed.
   - **SPIDR MVP Slicing**: Slice scope into Spike, Paths, Interfaces, Data, and Rules columns.
   - **NFRs & Security**: Compile quantitative SLA/SLO latency targets and STRIDE/RBAC tables.
   - **Zero-Downtime Rollout**: Specify feature flag keys, expand/contract migration phases, and rollback triggers.
4. **Generate Mermaid Visualizations**:
   - Architecture Topology (`flowchart LR` / `flowchart TD`)
   - Sequence Flow (`sequenceDiagram`)
5. **Assemble Master `PRD.md`**:
   - Populate Sections 1 through 7 with extracted facts and inline placeholders.
   - Compile Section 8 (*Open Questions & Placeholder Registry*).
   - Initialize Section 9 (*Decision Log & Audit Trail*).
6. **Output Summary**:
   - Report draft completion, number of extracted facets, and count of open placeholders.

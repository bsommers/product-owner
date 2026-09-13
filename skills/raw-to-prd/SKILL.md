---
name: raw-to-prd
description: Use when transforming raw meeting minutes, audio transcripts, unformatted brainstorm notes, or /meeting-notes outputs into a structured, grounded baseline PRD with Mermaid diagrams and explicit unknown placeholders.
---

# Raw-to-PRD Synthesizer

## Overview

The `raw-to-prd` skill ingests unstructured product meeting minutes, notes, transcripts, or upstream summaries and extracts the 7 core facets into a baseline `PRD.md`. It enforces **Grounding Over Hallucination**: missing technical decisions are explicitly captured as `[UNKNOWN: ...]` placeholders.

## When to Use

- User supplies meeting notes, minutes, or transcripts to turn into a product spec.
- User invokes `/raw-to-prd`.
- Ingesting output from `/meeting-notes` or audio transcription tools.

---

## The 7 Core Facets

```text
1. Problem Space & Utility    (Who, What pain, Why now, Value proposition)
2. Business Value & KPIs       (Target ROI, Metrics, Timeline, Milestone goals)
3. Scope & Capability Map      (In-scope, Out-of-scope, Non-goals)
4. Use Cases & Journeys        (Actors, Preconditions, Happy & Edge paths)
5. Systems Touched & Topology  (Mermaid topology, internal DBs, 3rd party APIs)
6. Data Contracts & I/O        (Input payloads, schemas, output events)
7. Viability & Constraints     (Risks, mitigations, performance, compliance)
```

---

## Execution Workflow

1. **Ingest & Parse**: Scan raw text for explicit facts, decisions, debates, and unstated technical parameters.
2. **Apply Zero-Hallucination Filter**:
   - If a technology (e.g. database, broker, auth) is named $\rightarrow$ extract it.
   - If a technology is NOT named $\rightarrow$ create a formatted placeholder:
     ```markdown
     > [!NOTE] UNRESOLVED ITEM
     > **[UNKNOWN: <CATEGORY> - <TOPIC>]**
     > - **Source Context**: <Context from raw notes>
     > - **Options Identified**: <Options discussed or left blank>
     > - **Impact**: <What is blocked>
     > - **Interview Prompt**: "<Actionable Socratic question>"
     ```
3. **Generate Mermaid Visualizations**:
   - Architecture Topology (`flowchart LR` / `flowchart TD`)
   - Sequence Diagram (`sequenceDiagram`)
4. **Assemble Master `PRD.md`**:
   - Populate Sections 1 through 7 with knowns and inline placeholders.
   - Compile Section 8 (*Open Questions & Placeholder Registry*).
   - Initialize Section 9 (*Decision Log & Audit Trail*).
5. **Output Summary**:
   - Report draft completion, number of extracted facets, and count of open placeholders.

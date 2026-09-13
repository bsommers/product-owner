# System Architecture Document (SAD): Product Owner Skill Suite

## 1. Architectural Overview

The **Product Owner Skill Suite** provides an automated, deterministic pipeline that transforms unstructured product discussions, meeting minutes, and audio transcripts into a formal **Product Requirements Document (PRD)** and comprehensive **Product Documentation Suite**.

The architecture adheres to three core architectural invariants:
1. **Strict Evidence Grounding**: Deductions must be strictly backed by input text. Missing details trigger standardized placeholders instead of ungrounded completions.
2. **Deterministic State Lifecycle**: The PRD is treated as an immutable state machine where `[UNKNOWN]` markers are systematically converted into resolved decisions with timestamped audit trails.
3. **Derived Coherence**: Technical specifications, interface contracts, and viability models are generated deterministically from the approved PRD to prevent specification drift.

---

## 2. Architecture Diagrams

### 2.1 ASCII System Topology

```text
+===================================================================================================+
|                                    PRODUCT OWNER ARCHITECTURE                                     |
+===================================================================================================+
|                                                                                                   |
|  [ LAYER 1: INTAKE & PARSING ]                                                                    |
|  +---------------------+      +------------------------+      +--------------------------------+  |
|  | Unstructured Notes  |  OR  | Meeting Minutes (.txt) |  OR  | Upstream /meeting-notes Skill  |  |
|  +----------+----------+      +-----------+------------+      +---------------+----------------+  |
|             \                             |                                  /                    |
|              \--------------------------->+<--------------------------------/                     |
|                                           |                                                       |
|                                           v                                                       |
|  [ LAYER 2: 7-FACET SYNTHESIS & GROUNDING (raw-to-prd) ]                                          |
|  +---------------------------------------------------------------------------------------------+  |
|  |  +-----------------------+   +------------------------+   +------------------------------+  |  |
|  |  | Fact & Evidence Match |   | Anti-Hallucination Gap |   | Mermaid Topology & Sequence  |  |  |
|  |  | (100% Text Grounding) |   | (Flags [UNKNOWN] Tags) |   | (Graph Visualizer Engine)    |  |  |
|  |  +-----------+-----------+   +-----------+------------+   +--------------+---------------+  |  |
|  +--------------|---------------------------|-------------------------------|------------------+  |
|                 \                           |                              /                      |
|                  \------------------------->+<----------------------------/                       |
|                                             |                                                     |
|                                             v                                                     |
|  [ LAYER 3: MASTER LIVING SPECIFICATION ]                                                         |
|  +---------------------------------------------------------------------------------------------+  |
|  | PRD.md                                                                                      |  |
|  |  - Sections 1-7: Core Facets (Utility, Business, Use Cases, Systems, I/O, Viability)        |  |
|  |  - Section 8:    Active Placeholder Registry                                                |  |
|  |  - Section 9:    Decision Log & Audit Trail                                                 |  |
|  +------------------------------------------+--------------------------------------------------+  |
|                                             |                                                     |
|                     +-----------------------+-----------------------+                             |
|                     |                                               |                             |
|                     v                                               v                             |
|  [ LAYER 4: SOCRATIC REFINEMENT (prd-refine) ]     [ LAYER 5: QUALITY GATE (product-audit) ]      |
|  +-------------------------------------------+     +-------------------------------------------+  |
|  | - Scans active [UNKNOWN] markers          |     | - Calculates Spec Completeness Index (SCI)|  |
|  | - Prioritizes highest architectural risk  |     | - Validates Mermaid nodes vs text systems |  |
|  | - Executes 1-at-a-time interactive interview |  | - Checks I/O schema completeness          |  |
|  | - Patches PRD.md in-place                 |     | - Emits AUDIT-PRD.md                      |  |
|  +-------------------------------------------+     +---------------------+---------------------+  |
|                     ^                                                    |                        |
|                     |================ (If Audit Fails) ==================| (If Audit Passes)      |
|                                                                          |                        |
|                                                                          v                        |
|  [ LAYER 6: MULTI-SPECIFICATION SUITE GENERATOR (product-doc-suite) ]                             |
|  +---------------------------------------------------------------------------------------------+  |
|  |  +--------------------+   +-------------------+   +--------------------+   +-------------+  |  |
|  |  | docs/architecture.md|   | docs/use-cases.md |   | docs/contracts.md  |   |docs/viability|  |  |
|  |  | (Detailed SAD Spec)|   | (Gherkin Scenarios|   | (I/O JSON Schemas) |   | (ROI & Risk)|  |  |
|  |  +--------------------+   +-------------------+   +--------------------+   +-------------+  |  |
|  +---------------------------------------------------------------------------------------------+  |
+===================================================================================================+
```

### 2.2 Inline Mermaid System Architecture

```mermaid
flowchart TD
    subgraph Ingestion ["1. Ingestion Layer"]
        Inp1["Raw Notes / Transcripts"]
        Inp2["Upstream /meeting-notes Output"]
    end

    subgraph CoreEngine ["2. Core Analysis & Extraction Engine"]
        RTP["raw-to-prd Skill"]
        FacetExt["7-Facet Entity Extractor"]
        GapFilter["Grounding & Unknowns Engine"]
        Diagrammer["Mermaid Graph Synthesizer"]
    end

    subgraph StateStorage ["3. Living Master Specification"]
        PRD[("PRD.md\n(Master Living Document)")]
    end

    subgraph RefinementGate ["4. Socratic Loop & Quality Assurance"]
        PRDRefine["prd-refine Skill\n(1-at-a-time Socratic Loop)"]
        PRDAudit["product-audit Skill\n(Completeness & Integrity Check)"]
        AuditFile[("AUDIT-PRD.md")]
    end

    subgraph DocGeneration ["5. Product Documentation Pack (docs/)"]
        DocSuite["product-doc-suite Skill"]
        SAD["docs/architecture.md"]
        UC["docs/use-cases.md"]
        IO["docs/contracts.md"]
        VIAB["docs/viability.md"]
    end

    Inp1 --> RTP
    Inp2 --> RTP
    RTP --> FacetExt
    FacetExt --> GapFilter
    FacetExt --> Diagrammer
    GapFilter --> PRD
    Diagrammer --> PRD

    PRD <--> PRDRefine
    PRD --> PRDAudit
    PRDAudit --> AuditFile
    PRDAudit -.->|Fail: Remaining Unknowns| PRDRefine
    PRDAudit -->|Pass: SCI >= 95%| DocSuite

    DocSuite --> SAD
    DocSuite --> UC
    DocSuite --> IO
    DocSuite --> VIAB
```

*Standalone Diagram Source:* [system-architecture.mmd](file:///home/bill/src/ai/product-owner/docs/diagrams/system-architecture.mmd)

---

## 3. Subsystem Detailed Specifications

### 3.1 Subsystem 1: Ingestion & 7-Facet Synthesizer (`raw-to-prd`)
- **Purpose**: Consumes raw input text and constructs an exhaustive first draft of `PRD.md`.
- **The 7-Facet Extraction Matrix**:
  1. **Viability**: Extracts operational limitations, performance bottlenecks, tech compatibility constraints.
  2. **Utility & Problem Space**: Extracts target personas, user pain points, and current inefficient workarounds.
  3. **Business Value & Success Metrics**: Extracts quantifiable targets (KPIs, time saved, revenue goals).
  4. **Use Cases & User Journeys**: Identifies primary workflows, preconditions, actor actions, and edge cases.
  5. **Technologies Involved & Systems Touched**: Compiles an explicit inventory of all databases, microservices, 3rd party APIs, and cloud infrastructure.
  6. **Data Contracts & I/O Specifications**: Identifies payload models, schema structures, triggers, and response codes.
  7. **Visual Architecture & Sequence Models**: Synthesizes Mermaid graphs representing system boundaries and data interactions.

### 3.2 Subsystem 2: Socratic Refinement Engine (`prd-refine`)
- **Purpose**: Systematically eliminates all `[UNKNOWN: ...]` and `[DECISION NEEDED: ...]` markers from `PRD.md`.
- **Execution Mechanism**:
  1. **Dependency Analysis**: Builds an internal dependency graph of open unknowns (e.g. Database Choice blocks Data Schema).
  2. **Atomic Socratic Querying**: Prompts the user with one question at a time, providing the raw meeting context, evaluated tradeoffs, and a concrete recommendation.
  3. **In-Place Mutation**: Directly edits the affected PRD sections upon user confirmation and logs the decision into Section 9 (*Decision Log & Audit Trail*).

### 3.3 Subsystem 3: Quality Gate & Auditor (`product-audit`)
- **Purpose**: Acts as an automated quality gate before downstream engineering or full doc derivation begins.
- **Verification Dimensions**:
  - **Spec Completeness Index (SCI)**: Mathematical ratio of completed sections to total required sections.
  - **Cross-Diagram Consistency**: Every system entity referenced in Section 5 must have a corresponding node in the Mermaid diagrams.
  - **Data Contract Completeness**: Every declared input must have a corresponding output contract and error schema.
  - **Zero Fabrication Verification**: Checks that no ungrounded technical decisions leaked past the placeholder engine.

### 3.4 Subsystem 4: Product Documentation Suite (`product-doc-suite`)
- **Purpose**: Generates granular, modular engineering documents from the approved `PRD.md`.
- **Generated Artifacts**:
  - `docs/architecture.md`: System Architecture Document (SAD), service topologies, component interaction matrices.
  - `docs/use-cases.md`: Capability matrix, persona profiles, Given-When-Then (Gherkin) acceptance criteria.
  - `docs/contracts.md`: JSON Schema contracts, OpenAPI/gRPC interface definitions, event payloads.
  - `docs/viability.md`: Business viability scorecard, risk assessment matrix, compliance review, ROI forecast.

---

## 4. State Management & Lifecycle

```text
[ RAW INTAKE ] ──> [ DRAFT (SCI < 60%) ] ──> [ REFINING (SCI 60-95%) ] ──> [ FROZEN (SCI 100%) ] ──> [ DERIVED DOCS ]
```

| Lifecycle Phase | Active Document State | Allowed Actions |
|---|---|---|
| **Raw Intake** | Input stream parsed | Ingestion, initial facet extraction |
| **Draft** | `PRD.md` contains multiple `[UNKNOWN]` markers | Initial read, batch placeholder logging |
| **Refining** | `PRD.md` undergoing Socratic loop | 1-at-a-time interview, in-place patching, decision logging |
| **Frozen** | `PRD.md` validated by `product-audit` (PASS) | Read-only baseline for engineering & planning |
| **Derived Docs** | Companion docs written in `docs/` | Downstream execution (`spec-driven-development`, `/plan`, etc.) |

*Standalone State Lifecycle Source:* [state-lifecycle.mmd](file:///home/bill/src/ai/product-owner/docs/diagrams/state-lifecycle.mmd)

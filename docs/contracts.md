# Data Contracts & Schema Specifications

## 1. Placeholder & Unknown Syntax Contract

The Product Owner pipeline relies on a machine-parseable, human-readable placeholder contract to mark all ambiguities and unmade decisions.

### 1.1 Formal Placeholder Grammar

```text
> [!NOTE] UNRESOLVED ITEM
> **[UNKNOWN: <CATEGORY> - <TOPIC_TITLE>]**
> - **Source Context**: <Brief quote or note excerpt triggering the question>
> - **Options Identified**: <Option 1> | <Option 2> | <Option 3> (or None)
> - **Impact**: <Downstream sections or engineering tasks blocked>
> - **Interview Prompt**: "<Actionable Socratic question for refinement>"
```

### 1.2 Placeholder Taxonomy

| Category Key | Scope | Example |
|---|---|---|
| `REQUIREMENT` | Business logic, scope boundaries, user permissions | `[UNKNOWN: REQUIREMENT - Multi-Tenant Isolation Rule]` |
| `ARCHITECTURE` | Tech stack, cloud infra, message brokers, caching | `[UNKNOWN: ARCHITECTURE - Message Broker Selection]` |
| `DATA_SCHEMA` | Field types, primary keys, nullability, formats | `[UNKNOWN: DATA_SCHEMA - Transaction ID Format]` |
| `NFR_METRIC` | Target latency, throughput, error budget, KPIs | `[UNKNOWN: NFR_METRIC - Ingestion Latency SLA Target]` |
| `SECURITY_RBAC` | User role permissions, auth flows, token scopes | `[UNKNOWN: SECURITY_RBAC - Contributor Permission Tier]` |
| `INTEGRATION` | External APIs, auth mechanisms, webhook retries | `[UNKNOWN: INTEGRATION - CRM Webhook Signature Auth]` |
| `DECISION NEEDED` | Conflicting options surfaced in meeting minutes | `[DECISION NEEDED: FORK - GraphQL vs REST API Standard]` |

---

## 2. Master `PRD.md` Document Schema Contract

Every generated `PRD.md` must strictly adhere to this 9-section structure:

```markdown
# PRD: [Product / Initiative Name]

## 1. Executive Summary & Problem Space
- **Problem Statement**: [Concrete description of pain points]
- **Target Beneficiaries**: [User personas and stakeholders]
- **Core Value Proposition**: [Delta over current solution]
- **Alternative & Workaround Comparison**:
  | Solution / Workaround | Limitations | Cost / Friction | Why New Solution Wins |
  |---|---|---|---|

## 2. Business Value & Success Metrics
- **Strategic Objectives**: [High-level business outcomes]
- **Primary KPIs**:
  | Metric Name | Baseline | Target Goal | Measurement Window |
  |---|---|---|---|
- **Target Timeline & Milestones**: [Target release phases]

## 3. Scope, Capability Map & SPIDR MVP Slicing
### 3.1 Scope Boundaries
- **In-Scope Capabilities**: [Explicit list of features]
- **Out-of-Scope / Non-Goals**: [Explicit anti-features and deferred items]

### 3.2 SPIDR Vertical MVP Slicing
| Slice Type | Focus Area | Minimal Viable Scope (Phase 1) | Incremental Scope (Phase 2+) |
|---|---|---|---|
| **S - Spike** | Architecture & Tech Feasibility | ... | ... |
| **P - Paths** | Workflow Paths | ... | ... |
| **I - Interfaces** | Ingestion & Access Channels | ... | ... |
| **D - Data** | Schema & Payload Complexity | ... | ... |
| **R - Rules** | Business Logic & Validations | ... | ... |

## 4. Detailed Use Cases, User Journeys & UI/UX Specifications
### 4.1 Detailed Use Cases (Gherkin & Edge Cases)
### 4.2 UI/UX Wireframe & Interaction Layouts
- ASCII Screen Mockup
- Form & Field Validation Matrix
- Component State Transitions

## 5. System Interactions & Architecture
### 5.1 Architecture Topology (Mermaid flowchart)
### 5.2 Systems Interacted With & Touched
| System Name | Category | Ownership | Protocol | Data Exchanged | Fallback Strategy |
|---|---|---|---|---|---|
### 5.3 Sequence & Integration Flows (Mermaid sequenceDiagram)

## 6. Data Contracts & I/O Specifications
### 6.1 Input Ingestion Contracts (JSON Schema)
### 6.2 Output & Event Contracts (Event Schema & Error Models)

## 7. Viability, NFRs, Security & Safe Rollout
### 7.1 Quantitative NFRs & Latency SLO/SLA Targets
| Dimension | Metric | Target (SLO) | Breach Threshold (SLA) | Measurement Method |
|---|---|---|---|---|
### 7.2 Security Threat Model (STRIDE) & RBAC Capability Matrix
| Role Name | Read Spec | Edit Draft | Resolve Unknowns | Freeze PRD | Deploy / Ship |
|---|:---:|:---:|:---:|:---:|:---:|
### 7.3 Zero-Downtime Rollout & Feature Flagging Runbook
- Feature flag keys & rollout cohorts
- Expand/contract DB schema transition
- Rollback alert triggers
### 7.4 Risk Assessment & Mitigation Matrix

## 8. Open Questions & Placeholder Registry
*(Auto-populated with all unresolved `[UNKNOWN]` markers)*

## 9. Decision Log & Audit Trail
| Date (ISO) | Section Affected | Prior State / Question | Decision Made | Author / Stakeholder |
|---|---|---|---|---|
```

---

## 3. Specialized Data Contracts

### 3.1 Role-Based Access Control (RBAC) Contract Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "RBACPolicy",
  "type": "object",
  "required": ["roles", "resources"],
  "properties": {
    "roles": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["roleName", "permissions"],
        "properties": {
          "roleName": { "type": "string" },
          "permissions": {
            "type": "array",
            "items": { "type": "string" }
          }
        }
      }
    }
  }
}
```

### 3.2 Feature Flag Configuration Contract Schema
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "FeatureFlagConfig",
  "type": "object",
  "required": ["flagKey", "defaultValue", "rolloutStrategy", "rollbackAlerts"],
  "properties": {
    "flagKey": { "type": "string", "pattern": "^feat_[a-z0-9_]+$" },
    "defaultValue": { "type": "boolean" },
    "rolloutStrategy": {
      "type": "object",
      "properties": {
        "canaryPercentage": { "type": "number", "minimum": 0, "maximum": 100 },
        "targetCohorts": { "type": "array", "items": { "type": "string" } }
      }
    },
    "rollbackAlerts": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["metric", "threshold", "windowMinutes"],
        "properties": {
          "metric": { "type": "string" },
          "threshold": { "type": "string" },
          "windowMinutes": { "type": "integer" }
        }
      }
    }
  }
}
```

---

## 4. Decision Log Table Contract (Section 9)

```text
| Date (YYYY-MM-DD) | Section Affected | Prior State / Question | Decision Made | Author / Stakeholder |
```

---

## 5. Derived Documentation Suite Contract (`docs/`)

When `product-doc-suite` runs, it derives four standardized documents:

```text
docs/
├── architecture.md    # SAD, topologies, STRIDE threat model, expand/contract DB pattern
├── use-cases.md       # Personas, capability matrix, SPIDR vertical slicing, ASCII wireframes
├── contracts.md       # API schemas, RBAC tables, feature flag contracts, error models
└── viability.md       # ROI analysis, latency SLO budgets, zero-downtime rollout runbook
```

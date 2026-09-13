# PRD: [Product / Initiative Name]

## 1. Executive Summary & Problem Space
- **Problem Statement**: [Description of user/business pain point]
- **Target Beneficiaries**: [Personas and user profiles]
- **Core Value Proposition**: [Delta over current alternatives]

## 2. Business Value & Success Metrics
- **Strategic Objectives**: [Business outcomes]
- **Primary KPIs**:
  | Metric Name | Baseline | Target Goal | Measurement Window |
  |---|---|---|---|
- **Target Timeline & Milestones**: [Phased milestones]

## 3. Scope & Capability Map
- **In-Scope Capabilities**: [Features and components]
- **Out-of-Scope / Non-Goals**: [Explicit anti-features]
- **Phased Rollout Strategy**: [Phase 1 MVP -> Phase 2 -> Phase 3]

## 4. Detailed Use Cases & User Journeys
### UC-01: [Use Case Title]
- **Primary Actor**: [Actor Persona]
- **Preconditions**: [State before trigger]
- **Trigger**: [Initiating event]
- **Happy Path**: [Step-by-step sequence]
- **Edge Cases & Error Handling**: [Failure modes and fallbacks]
- **Postconditions**: [State after completion]

## 5. System Interactions & Architecture
### 5.1 Architecture Topology
```mermaid
flowchart LR
    %% System architecture nodes
```
### 5.2 Systems Interacted With & Touched
| System Name | Category | Ownership | Protocol | Data Exchanged | Fallback Strategy |
|---|---|---|---|---|---|

### 5.3 Sequence & Integration Flows
```mermaid
sequenceDiagram
    %% End-to-end integration flow
```

## 6. Data Contracts & I/O Specifications
### 6.1 Input Ingestion Contracts
- **Channel**: [REST / gRPC / Webhook / CLI]
- **Payload Schema**:
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object"
}
```

### 6.2 Output & Event Contracts
- **Output Schema**: [Response JSON / Event Schema]
- **Error Response Models**: [4xx / 5xx error formats]

## 7. Viability, Risks & Technical Constraints
- **Technical Feasibility & Bottlenecks**: [Known limitations]
- **Dependencies**: [Internal services, vendor SLAs]
- **Security, Privacy & Compliance**: [GDPR, SOC2, Data retention]
- **Risk Assessment & Mitigation Matrix**:
  | Risk Description | Severity (H/M/L) | Likelihood (H/M/L) | Mitigation Strategy |
  |---|---|---|---|

## 8. Open Questions & Placeholder Registry
*(Auto-populated with active `[UNKNOWN: ...]` markers)*

## 9. Decision Log & Audit Trail
| Date (YYYY-MM-DD) | Section Affected | Prior State / Question | Decision Made | Author / Stakeholder |
|---|---|---|---|---|

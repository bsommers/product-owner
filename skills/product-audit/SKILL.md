---
name: product-audit
description: Use when auditing a PRD or product documentation suite for completeness, calculating the Spec Completeness Index (SCI), verifying diagram-to-text consistency, checking schema pairing, or enforcing zero-hallucination gates.
---

# Product Quality & Viability Auditor

## Overview

The `product-audit` skill serves as the automated quality gate for product specifications. It calculates the **Spec Completeness Index (SCI)**, verifies cross-diagram consistency, validates schema pairing, checks NFR/Security completeness, and emits `AUDIT-PRD.md`.

## When to Use

- Validating `PRD.md` before freezing requirements.
- User invokes `/product-audit`.
- Before generating auxiliary docs with `product-doc-suite` or planning implementation tasks.

---

## Spec Completeness Index (SCI)

$$\text{SCI} = \left( 1 - \frac{W_{\text{req}} \cdot N_{\text{unknown}} + W_{\text{fork}} \cdot N_{\text{fork}}}{N_{\text{total\_dimensions}}} \right) \times 100\%$$

- $W_{\text{req}} = 1.0$ (weight for functional unknown)
- $W_{\text{fork}} = 1.5$ (weight for scope/tech fork)
- $N_{\text{total\_dimensions}} = 25$ (extended evaluation units across all 7 facets)

### Audit Thresholds
- **PASS**: $\text{SCI} \ge 95\%$ and 0 fatal structural errors $\rightarrow$ Proceed to `product-doc-suite` / `/plan`.
- **WARNING**: $80\% \le \text{SCI} < 95\%$ $\rightarrow$ Non-critical edge cases open.
- **FAIL**: $\text{SCI} < 80\%$ or ungrounded hallucinations $\rightarrow$ Block downstream work; route to `/prd-refine`.

---

## The 7 Verification Gates

1. **Section Completeness**: All 9 required PRD sections present.
2. **Diagram Consistency**: Every system entity in Section 5.2 must appear in Section 5.1 Mermaid diagram.
3. **Contract Pairing**: Every input schema in Section 6.1 must have a corresponding output/error schema in Section 6.2.
4. **NFR & Latency Budgeting**: Quantitative p95/p99 latency, throughput, and availability targets defined (Section 7.1).
5. **Threat Model & RBAC Security**: STRIDE threat analysis and role-permission matrices defined (Section 7.2).
6. **SPIDR MVP Slicing & Rollout Runbook**: Vertical slicing and feature flagging/rollback triggers specified (Sections 3.2 & 7.3).
7. **Zero Fabrication & Registry Parity**: All active inline `[UNKNOWN]` markers registered in Section 8; all technical choices grounded in notes or Section 9 Decision Log.

---

## Execution Workflow

1. Read `PRD.md`.
2. Execute the 7 verification gates.
3. Calculate SCI score.
4. Determine Verdict: `PASS`, `WARNING`, or `FAIL`.
5. Write `AUDIT-PRD.md` to workspace root.
6. Present concise status verdict and gap list to user.

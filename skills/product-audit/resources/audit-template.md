# Quality Gate Audit Report: {{PRODUCT_NAME}}

- **Audit Date**: {{AUDIT_DATE_ISO}}
- **Overall Verdict**: **{{VERDICT}}** (PASS / WARNING / FAIL)
- **Spec Completeness Index (SCI)**: **{{SCI_SCORE}}%**

---

## 1. Completeness Evaluation
- **Total Required Sections**: 9
- **Fully Specified Sections**: {{COMPLETED_SECTIONS_COUNT}} / 9
- **Active Unknown Placeholders**: {{UNKNOWN_COUNT}}
- **Active Decision Forks**: {{FORK_COUNT}}

---

## 2. Gate Verification Results

| Gate | Verification Check | Status | Notes / Gaps |
|---|---|---|---|
| **Gate 1** | Section Completeness | {{STATUS_G1}} | {{NOTES_G1}} |
| **Gate 2** | Diagram & Entity Consistency | {{STATUS_G2}} | {{NOTES_G2}} |
| **Gate 3** | I/O Schema Pairing | {{STATUS_G3}} | {{NOTES_G3}} |
| **Gate 4** | Placeholder Registry Parity | {{STATUS_G4}} | {{NOTES_G4}} |
| **Gate 5** | Zero Unverified Hallucination | {{STATUS_G5}} | {{NOTES_G5}} |

---

## 3. Active Placeholder Registry
{{PLACEHOLDER_TABLE_OR_LIST}}

---

## 4. Required Remediation Actions
{{REMEDIATION_ACTION_ITEMS}}

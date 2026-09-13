---
name: prd-refine
description: Use when resolving open [UNKNOWN] or [DECISION NEEDED] placeholders in a PRD through an interactive one-question-at-a-time Socratic interview and updating the decision log.
---

# PRD Socratic Refiner

## Overview

The `prd-refine` skill scans `PRD.md` for open `[UNKNOWN: ...]` and `[DECISION NEEDED: ...]` placeholders and runs a low-friction, atomic Socratic interview. It resolves each ambiguity, updates affected sections in-place, and maintains the decision audit log.

## When to Use

- `PRD.md` contains open `[UNKNOWN]` markers.
- User invokes `/prd-refine`.
- Transitioning a draft PRD into a frozen, implementation-ready specification.

---

## Core Refinement Rules

1. **One Question at a Time**: Never ask multiple questions in a single turn. Always prioritize the highest-impact architectural unknown first.
2. **Contextual Grounding**: State *why* the question is needed and *what* in the raw notes triggered it.
3. **Structured Options**: Provide 2-4 concrete choices with a recommended default based on best practices.
4. **Immediate In-Place Patching**: Mutate `PRD.md` as soon as the user confirms a decision.
5. **Log Every Decision**: Append a row to Section 9 (*Decision Log & Audit Trail*).

---

## Refinement Execution Flow

```text
1. Scan PRD.md -> Count [UNKNOWN] & [DECISION] markers.
2. Select highest priority item based on dependency order:
   - Priority 1: Core Problem & Target Users (Section 1)
   - Priority 2: System Architecture & Data Store Choice (Section 5)
   - Priority 3: Data Contracts & I/O Schemas (Section 6)
   - Priority 4: Viability & Compliance Constraints (Section 7)
3. Present Socratic Question:
   - State Context + Impact
   - Present Options + (Recommended) Default
4. Await User Response.
5. Patch PRD.md:
   - Replace placeholder with resolved text in relevant section.
   - Update Section 8 (Placeholder Registry).
   - Append to Section 9 (Decision Log).
6. Repeat until 0 unknowns remain -> Report PRD Frozen.
```

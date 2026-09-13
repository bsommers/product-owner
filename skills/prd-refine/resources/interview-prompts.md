# Standard Socratic Interview Prompt Templates

Use these prompt patterns when formulating questions for open placeholders:

---

## 1. Architectural & Technology Choice Prompt

```markdown
> [!IMPORTANT]
> **Decision Needed ({{CURRENT_INDEX}} of {{TOTAL_COUNT}}): {{TOPIC_TITLE}}**
>
> **Source Context**: {{RAW_CONTEXT_EXCERPT}}
> **Architectural Impact**: Blocks Section 5 (Topology) and Section 6 (Contracts).
>
> **Evaluated Options**:
> 1. *(Recommended)* **{{OPTION_1_NAME}}**: {{OPTION_1_RATIONALE}}
> 2. **{{OPTION_2_NAME}}**: {{OPTION_2_RATIONALE}}
> 3. **{{OPTION_3_NAME}}**: {{OPTION_3_RATIONALE}}
>
> *Reply with your selection (1, 2, 3) or enter custom configuration.*
```

---

## 2. Data Contract & Schema Prompt

```markdown
> [!IMPORTANT]
> **Decision Needed ({{CURRENT_INDEX}} of {{TOTAL_COUNT}}): {{FIELD_NAME}} Payload Specification**
>
> **Source Context**: {{RAW_CONTEXT_EXCERPT}}
> **Impact**: Affects API Contract validation and persistence layer.
>
> **Proposed Structure**:
> ```json
> {
>   "{{FIELD_NAME}}": "{{RECOMMENDED_FORMAT}}"
> }
> ```
> *Confirm this payload structure (Yes) or specify changes.*
```

---

## 3. Scope & Conflict Fork Prompt

```markdown
> [!NOTE]
> **Decision Needed ({{CURRENT_INDEX}} of {{TOTAL_COUNT}}): Scope Fork on {{FEATURE_NAME}}**
>
> **Conflict Found**: In meeting minutes, Person A suggested {{PROPOSAL_A}}, while Person B favored {{PROPOSAL_B}}.
>
> **Resolution Options**:
> 1. *(Recommended)* **{{FORK_OPTION_A}}**: Include in Phase 1 MVP.
> 2. **{{FORK_OPTION_B}}**: Defer to Phase 2 Backlog.
> 3. **{{FORK_OPTION_C}}**: Explicitly mark as Out-of-Scope (Non-Goal).
```

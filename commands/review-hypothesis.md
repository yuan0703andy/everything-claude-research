---
name: review-hypothesis
description: 啟動假設審查循環。Experimentalist 評估可行性，Methodologist 審查方法。
argument-hint: <hypothesis-id>
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

<review_hypothesis_command>

# /review-hypothesis Command

Start multi-agent review cycle for a hypothesis.

## Workflow

```
User: /review-hypothesis H-001
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Load Hypothesis                │
    │  - Read hypothesis proposal             │
    │  - Load domain context                  │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Parallel Review (spawn both)   │
    │  - Experimentalist: Feasibility         │
    │  - Methodologist: Methods review        │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Aggregate Feedback             │
    │  - Combine reviews                      │
    │  - Identify critical issues             │
    │  - Prepare revision guidance            │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 4: Present to User                │
    │  - Show consolidated feedback           │
    │  - Recommend action                     │
    │  - Update Elo based on review           │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 5: Route Next Steps               │
    │  - Approved → Ready for testing         │
    │  - Revise → Back to Theorist            │
    │  - Reject → Archive with reason         │
    └─────────────────────────────────────────┘
```

## Execution Steps

### 1. Load Context

```python
hypothesis_id = args[0]  # e.g., "H-001"
hypothesis_path = f"hypotheses/proposals/{hypothesis_id}-*.md"
hypothesis = read(glob(hypothesis_path)[0])

domain_md = read(get_domain_path() + "/DOMAIN.md")
project_md = read("PROJECT.md")
```

### 2. Spawn Parallel Reviews

**To Experimentalist:**
```markdown
<task>
## Hypothesis to Assess
[Full hypothesis proposal]

## Domain Context
[From DOMAIN.md]

## Request
Provide a Feasibility Report following the standard format.

Focus on:
1. Decomposition into testable sub-assumptions
2. Data requirements and availability
3. Resource estimate
4. Technical risks
5. Overall feasibility verdict
</task>
```

**To Methodologist:**
```markdown
<task>
## Hypothesis to Review
[Full hypothesis proposal]

## Domain Context
[From DOMAIN.md]

## Request
Provide a Methods Review Report following the standard format.

Apply six-dimension verification:
1. Research question coverage
2. Measurement validity
3. Causal inference validity
4. Statistical appropriateness
5. Generalizability
6. Reproducibility

Provide clear verdict: APPROVE / REVISE / MAJOR CONCERNS
</task>
```

### 3. Aggregate Results

```markdown
# Review Aggregation

## Feasibility Assessment (Experimentalist)
**Verdict**: [GREEN/YELLOW/RED]
**Key Issues**: [List]
**Showstoppers**: [Any?]

## Methods Review (Methodologist)
**Verdict**: [APPROVE/REVISE/MAJOR CONCERNS]
**Critical Issues**: [List]
**Dimension Scores**: [Summary]

## Consolidated Verdict

| Criterion | Status | Notes |
|-----------|--------|-------|
| Feasibility | [Status] | [Brief] |
| Methods | [Status] | [Brief] |
| Overall | [READY/REVISE/REJECT] | [Rationale] |
```

### 4. Output Presentation

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 REVIEW ► H-001: [Hypothesis Title]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Feasibility Assessment
**Verdict**: 🟢 GREEN / 🟡 YELLOW / 🔴 RED

[Summary of key points]

## Methods Review  
**Verdict**: ✅ APPROVE / ⚠️ REVISE / ❌ MAJOR CONCERNS

[Summary of key points]

─────────────────────────────────────────────────────

## Critical Issues to Address

1. **[Issue]**: [Description]
   - Impact: [What this affects]
   - Fix: [Suggested resolution]

2. **[Issue]**: [Description]
   - Impact: [What this affects]
   - Fix: [Suggested resolution]

─────────────────────────────────────────────────────

## Recommendation: [APPROVE / REVISE / REJECT]

[Rationale for recommendation]

## Elo Update
Previous: [Old Elo] → New: [New Elo] ([+/-] [points])
Reason: [Review outcome]

─────────────────────────────────────────────────────

## Next Steps

[If APPROVE]:
✅ Hypothesis approved for testing
→ `/execute-analysis H-001` to proceed

[If REVISE]:
⚠️ Revisions needed before proceeding
→ "Revise H-001" to send back to Theorist with feedback
→ Or address specific issues: "Fix issue 1 and 2"

[If REJECT]:
❌ Hypothesis not viable in current form
→ "Archive H-001" to document and move on
→ Or "Salvage H-001" to extract useful elements
```

### 5. State Update

```markdown
# Update STATE.md

## Hypothesis Pipeline

### Under Review → [New Status]
| ID | Title | Review Outcome | Next Step |
|----|-------|----------------|-----------|
| H-001 | [Title] | [APPROVE/REVISE/REJECT] | [Action] |

## Elo Rankings
[Update based on review outcome]
```

## Review Outcome Actions

| Outcome | Elo Change | Action |
|---------|------------|--------|
| APPROVE (both) | +30 | Move to Ready for Testing |
| APPROVE (one), REVISE (one) | +10 | Minor revisions, keep in review |
| REVISE (both) | -10 | Send back to Theorist |
| REJECT (any) | -30 | Archive with reason |

## Revision Loop

If REVISE:
```
1. Consolidate feedback for Theorist
2. Theorist revises hypothesis
3. Re-run /review-hypothesis
4. Repeat until APPROVE or REJECT
```

Maximum iterations: 3 (then escalate to PI)

</review_hypothesis_command>

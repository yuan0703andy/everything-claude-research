---
name: verify-results
description: 執行 goal-backward verification。檢查研究是否真正達成目標。
argument-hint: <hypothesis-id or "phase">
allowed-tools: Read, Bash, Glob, Grep, Write, Task
---

<verify_results_command>

# /verify-results Command

Run goal-backward verification to ensure research achieves its intended goals.

## Workflow

```
User: /verify-results H-001
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Load Context                   │
    │  - Original hypothesis and goals        │
    │  - Research design                      │
    │  - Execution artifacts                  │
    │  - Results/outputs                      │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Spawn Verifier                 │
    │  - Goal extraction                      │
    │  - Must-have derivation                 │
    │  - Verification checks                  │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Present Results                │
    │  - Verification report                  │
    │  - Gaps identified                      │
    │  - Recommendation                       │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 4: Route Next Steps               │
    │  - VERIFIED → Complete hypothesis       │
    │  - GAPS → Create fix plans              │
    │  - BLOCKED → Escalate to PI             │
    └─────────────────────────────────────────┘
```

## Execution

### Spawn Verifier

```markdown
<task>
## Verification Target
[Hypothesis ID and title]

## Original Goals
[From hypothesis proposal]

## Research Design
[From approved design]

## Execution Artifacts
[What was done - data collected, analyses run]

## Results
[Findings/outputs]

## Request
Perform goal-backward verification:
1. Extract goals and success criteria
2. Derive must-haves from goals
3. Check each must-have against artifacts
4. Identify gaps
5. Provide verdict

Output: Full Verification Report
</task>
```

### Output Presentation

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 VERIFY ► H-001: [Title]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Goal Verification

**Primary Goal**: [What we were trying to learn]

### Must-Haves Check
| Must-Have | Status | Notes |
|-----------|--------|-------|
| [MH-1] | ✅ | [Verified how] |
| [MH-2] | ✅ | [Verified how] |
| [MH-3] | ⚠️ | [Partial - issue] |
| [MH-4] | ❌ | [Missing - why] |

### Logic Chain
```
Goal: [State goal]
  ├── MH-1: ✅ 
  ├── MH-2: ✅
  ├── MH-3: ⚠️ (partial)
  └── MH-4: ❌ (missing)

Chain Status: INCOMPLETE
```

─────────────────────────────────────────────────────

## Verdict: [VERIFIED / GAPS IDENTIFIED / BLOCKED]

[If VERIFIED]:
✅ All must-haves satisfied. Goal achieved.
Confidence: [High/Medium/Low]

[If GAPS IDENTIFIED]:
⚠️ Gaps found that need addressing:

### Gap 1: [Description]
- **Impact**: [What this affects]
- **Criticality**: Critical / Important / Minor
- **Fix**: [How to address]
- **Effort**: [Estimate]

### Gap 2: [Description]
[Same structure]

[If BLOCKED]:
❌ Critical issues prevent goal achievement:
- [Issue description]
- Requires: [What's needed - likely PI decision]

─────────────────────────────────────────────────────

## Next Steps

[If VERIFIED]:
→ `/complete-hypothesis H-001` to record success
→ Update Elo: +50 for verified hypothesis

[If GAPS]:
→ "Fix gaps" to create remediation tasks
→ After fixes: re-run `/verify-results H-001`

[If BLOCKED]:
→ Requires PI decision: [Specific question]
```

## Verification Types

| Command | Scope |
|---------|-------|
| `/verify-results H-001` | Single hypothesis |
| `/verify-results phase-1` | Entire research phase |
| `/verify-results design H-001` | Verify design only (before execution) |
| `/verify-results all` | Full project verification |

## Gap Resolution Flow

```
/verify-results H-001
    │
    │ [GAPS IDENTIFIED]
    ▼
"Fix gaps"
    │
    ▼
System creates fix tasks:
- Fix-001: [Task for gap 1]
- Fix-002: [Task for gap 2]
    │
    ▼
Execute fixes
    │
    ▼
/verify-results H-001 (re-verify)
    │
    │ [VERIFIED]
    ▼
/complete-hypothesis H-001
```

</verify_results_command>

---
name: update-state
description: Update STATE.md to record current research status
---

# /update-state

Update the project's STATE.md file to record research progress and status.

## Purpose

Maintain state continuity across sessions:
- Record work completed in this session
- Update hypothesis rankings
- Record key decisions
- List tasks for next time
- Remind important context

## When to Use

**Recommended timing**:
- ✅ Before ending a session (most common)
- ✅ After completing important milestones
- ✅ After making key decisions
- ✅ After hypothesis ranking changes
- ✅ During phase transitions

**Not recommended timing**:
- ❌ In the middle of work
- ❌ While still thinking through problems
- ❌ When there's no substantial progress

## Process

### 1. Read Current State

Lab Manager reads:
```
- STATE.md (current state)
- hypotheses/HYPOTHESES.md (hypothesis rankings)
- .planning/phases/ (recent phase records)
```

### 2. Collect Session Information

Lab Manager asks or automatically detects:

**Completed work**:
```
Q: What was completed in this session?
A: [User describes, or inferred from tool call history]

Examples:
- Completed Experimentalist review for H-003
- Executed main regression analysis
- Updated literature review
- Fixed data cleaning script
```

**Key decisions**:
```
Q: Were any important decisions made?
A: [User describes]

Examples:
- Decided to use diff-in-diff instead of RDD
- Decided to exclude data before 2008
- Decided to postpone H-002 (data unavailable)
```

**Hypothesis ranking changes**:
```
Check for:
- New hypotheses added
- Hypothesis status changes (draft → under review → verified)
- Elo score updates
```

**Issues to resolve**:
```
Q: What issues were encountered that need resolution?
A: [User describes]

Examples:
- Is H-003 sample size sufficient? Need power analysis
- Should we use cluster SE or robust SE?
```

**Tasks for next time**:
```
Q: What should be done in the next session?
A: [User describes or inferred from plans]

Examples:
1. Complete Methodologist review for H-003
2. Update Elo rankings
3. Start deep review of H-007
```

### 3. Check Hypothesis Status

Automatically read `hypotheses/HYPOTHESES.md`:

```markdown
## Current Hypothesis Rankings

| Rank | Δ | ID | Elo | Title | Status | Last Updated |
|------|---|-----|-----|-------|--------|-------------|
| 1 | - | H-003 | 1580 | [Title] | 🔄 Under Review | 2024-01-27 |
| 2 | ↑1 | H-001 | 1520 | [Title] | ✅ Verified | 2024-01-25 |
| 3 | ↓1 | H-007 | 1450 | [Title] | 📝 Draft | 2024-01-26 |
| 4 | - | H-005 | 1380 | [Title] | 🔄 Under Review | 2024-01-24 |
| 5 | - | H-002 | 1200 | [Title] | ⏸️ On Hold | 2024-01-20 |
```

Calculate Δ (ranking change): Compare with last STATE.md

### 4. Check Context Reminders

Confirm information that needs reminding:
```
- Domain settings
- PI preferences
- Resource constraints (data, time, compute)
- Upcoming deadlines
- Risk warnings
```

### 5. Generate Updated STATE.md

```markdown
# Research State

> Last updated: 2024-01-27 15:30
> Session: #12

## Current Position

- **Phase**: Phase 2 - Hypothesis Verification
- **In Progress**: H-003 completed Experimentalist review, awaiting Methodologist
- **Next Steps**: Methodologist review H-003 → Update Elo → Review H-007

## Hypothesis Rankings (Top 5)

| Rank | Δ | ID | Elo | Title | Status |
|------|---|-----|-----|-------|--------|
| 1 | - | H-003 | 1580 | Minimax rate for high-dimensional sparse regression | 🔄 Under Review |
| 2 | ↑1 | H-001 | 1520 | Convergence rate of adaptive estimation | ✅ Verified |
| 3 | ↓1 | H-007 | 1450 | Optimal bandwidth for nonparametric regression | 📝 Draft |
| 4 | - | H-005 | 1380 | Asymptotic properties of change point detection | 🔄 Under Review |
| 5 | - | H-002 | 1200 | Identification of causal effects | ⏸️ On Hold |

## Key Decisions

- **2024-01-25** Chose diff-in-diff as main method - Reason: Data structure fits, literature supports
- **2024-01-26** Excluded data before 2008 - Reason: Structural break (financial crisis)
- **2024-01-27** H-002 on hold due to data unavailability - Reason: Cannot obtain individual-level data

## Issues to Resolve

- [ ] Is H-003 sample size sufficient? Need power analysis
- [ ] Should we use cluster SE or robust SE?
- [ ] H-007 theoretical foundation needs strengthening

## Next Session Should

1. Complete Methodologist review for H-003
2. Update Elo rankings (if review passes)
3. Start deep review of H-007 (`/review-hypothesis H-007`)

## Context Reminders

- **Domain**: domains/stats-theory/
- **PI Preferences**: Prioritize theoretical clarity, then computational efficiency
- **Resource Constraints**:
  - Data: Only public data available
  - Time: Complete within 3 months
  - Compute: Single machine sufficient, no cluster needed
- **Deadline**: 2024-04-30 submission target

## Recently Completed

- **Session #12** (2024-01-27): H-003 Experimentalist review, feasibility confirmed
- **Session #11** (2024-01-26): H-007 draft completed, awaiting review
- **Session #10** (2024-01-25): H-001 verification passed, pass@3 = 95%

## Risk Warnings

- ⚠️ H-003 and H-007 theories may overlap, need clarification
- ⚠️ April deadline approaching, need to accelerate progress

---

## Session Summary

**Session #12 Achievements**:
- Completed H-003 Experimentalist review
- Confirmed hypothesis feasibility and data adequacy
- Identified potential sample size issues

**Duration**: 1.5 hours
**Tool calls**: 45
```

### 6. Save and Confirm

Lab Manager:
```
✅ STATE.md updated

Summary:
- Hypothesis rankings: H-003 remains #1, H-001 rose to #2
- New decisions: 1 (H-002 on hold)
- Issues to resolve: 3
- Next tasks: 3

Next session recommend starting with:
📖 Read STATE.md to understand current status
```

## Output

1. **Updated STATE.md** - Complete current state
2. **Session Summary** - Quick summary of this session

## Automation Features

Lab Manager can automatically infer information from:
- Tool call history (which files were read, which commands executed)
- Timestamps (session duration)
- File changes (which files were added or modified)
- Updates to hypothesis files

## Best Practices

### For Each Update
- ✅ Concisely record completed work
- ✅ Clearly record decisions and reasoning
- ✅ List specific next tasks
- ✅ Remind important context

### Avoid
- ❌ Excessive detail (details belong in phase files)
- ❌ Vague tasks ("continue analysis" is too vague)
- ❌ Forgetting to update hypothesis rankings
- ❌ Not recording reasoning for decisions

## Relationship with Other Commands

```
After any command completes
        ↓
/update-state    ← Record progress
        ↓
[Session ends]
        ↓
[Next session starts]
        ↓
/progress        ← View current state
        ↓
[Continue work]
```

## Example Scenarios

### Scenario 1: After Lab Meeting

```
User: /update-state

Lab Manager:
Reading STATE.md... Current Session #11
Reading meeting_notes/2024-01-27.md...

This lab meeting:
- Reviewed 3 hypotheses
- H-003 rose to #1 (Elo 1450 → 1580)
- Decided to postpone H-002
- Assigned next week's tasks

Updating STATE.md:
✅ Hypothesis rankings updated
✅ Key decisions recorded
✅ Next week's tasks listed

Next session start with:
1. `/review-hypothesis H-007`
2. Prepare analysis scripts for H-003
```

### Scenario 2: After Analysis Completion

```
User: Just completed main analysis for H-001
User: /update-state

Lab Manager:
Recording completed work... ✅
Checking results/ directory... Found new files
Checking hypotheses/H-001.md... Status unchanged

Update content:
- Recently completed: H-001 main analysis completed
- In progress: H-001 awaiting verification
- Next step: Run `/verify-results H-001`

STATE.md updated ✅
```

## Important Notes

- STATE.md is a **summary**, not a detailed record
- Detailed content is in:
  - `meeting_notes/` - Meeting details
  - `.planning/phases/` - Phase execution details
  - `hypotheses/` - Hypothesis details
- STATE.md is for **quick context recovery**, not a complete archive

## Tips

### If You Forget to Update
No problem, Lab Manager can infer from file history:
```
Lab Manager:
Last update was 3 days ago, let me check recent changes...

Found:
- hypotheses/H-005.md was updated
- meeting_notes/2024-01-26.md was created
- results/ has new files

Let me help you update STATE.md...
```

### Frequency Recommendations
- Ideal: End of each session
- Minimum: Once per week
- Critical moments: Phase transitions, after key decisions

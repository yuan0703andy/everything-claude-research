---
name: progress
description: Quickly view research progress and status
---

# /progress

Quickly display current research status, used at session start or when you need to quickly understand progress.

## Purpose

Provide an **at-a-glance** progress overview:
- Current phase/stage
- Hypothesis ranking status
- Recent key decisions
- Open issues
- Suggested next steps

## When to Use

**Recommended scenarios**:
- ✅ At the start of a new session (most common)
- ✅ After being away for a while
- ✅ When you need a quick review
- ✅ Before reporting to PI/collaborator
- ✅ When unsure what to do next

## Workflow

### 1. Read Core Files

Lab Manager reads:
```
Must read:
- STATE.md
- hypotheses/HYPOTHESES.md
- PROJECT.md

Optional:
- RESEARCH_PLAN.md (if exists)
- .planning/phases/latest (most recent phase)
- meeting_notes/latest (most recent meeting)
```

### 2. Generate Progress Report

```markdown
# Research Progress Report

**Project**: [Project Name]
**Generated**: 2024-01-27 15:30
**Session**: #12

---

## 📍 Current Position

**Phase**: Phase 2 - Hypothesis Validation (40% complete)

**In Progress**:
- H-003 completed Experimentalist review, awaiting Methodologist
- H-007 draft completed, awaiting review

**Next Steps**:
1. 🎯 Complete H-003 Methodologist review (priority)
2. 🔄 Update Elo rankings
3. 📝 Start H-007 deep review

---

## 🏆 Hypothesis Rankings (Top 5)

| # | Δ | ID | Elo | Title | Status | Progress |
|---|---|-----|-----|------|------|------|
| 1 | - | H-003 | 1580 | Minimax rates for high-dimensional sparse regression | 🔄 Under Review | ████░░ 67% |
| 2 | ↑1 | H-001 | 1520 | Convergence rates for adaptive estimation | ✅ Validated | ██████ 100% |
| 3 | ↓1 | H-007 | 1450 | Optimal bandwidth for nonparametric regression | 📝 Draft | ██░░░░ 33% |
| 4 | - | H-005 | 1380 | Asymptotic properties of change point detection | 🔄 Under Review | ███░░░ 50% |
| 5 | - | H-002 | 1200 | Identification of causal effects | ⏸️ On Hold | ░░░░░░ 0% |

**Total**: 7 hypotheses (2 validated, 3 under review, 1 draft, 1 on hold)

---

## 🎯 Project Progress

```
Phase 1: Context        ████████████ 100% ✅ (Completed 2024-01-15)
Phase 2: Hypothesis     ████████░░░░  67% 🔄 (In Progress)
Phase 3: Analysis       ░░░░░░░░░░░░   0% 📝 (Not Started)
Phase 4: Writeup        ░░░░░░░░░░░░   0% 📝 (Not Started)

Overall Progress: ████░░░░░░░░ 33%
```

**Projected Timeline**:
- Phase 2 completion: 2024-02-15 (2 weeks remaining)
- Phase 3 completion: 2024-03-31 (6 weeks remaining)
- Phase 4 completion: 2024-04-30 (9 weeks remaining) 🎯 Submission target

---

## ✅ Recently Completed (Last 3 sessions)

- **Session #12** (2024-01-27): H-003 Experimentalist review completed
- **Session #11** (2024-01-26): H-007 draft completed
- **Session #10** (2024-01-25): H-001 validation passed (pass@3 = 95%)

---

## 🔑 Key Decisions (Recent)

- **2024-01-27** H-002 put on hold due to data unavailability
- **2024-01-26** Excluded pre-2008 data (structural break)
- **2024-01-25** Selected diff-in-diff as primary method

---

## ❓ Open Issues (3)

**Priority HIGH**:
- [ ] Is sample size sufficient for H-003? Need power analysis

**Priority MEDIUM**:
- [ ] Should we use cluster SE or robust SE?
- [ ] H-007 theoretical foundation needs strengthening

---

## 💡 Suggested Next Steps

### Immediate Actions (this session)
1. **Complete H-003 review** - `/review-hypothesis H-003` complete Methodologist portion
2. **Update rankings** - If review passes, update Elo scores
3. **Power analysis** - Address sample size concerns

### This Week's Plan
- Complete deep review of H-007
- Begin preparing analysis scripts for Top 2 hypotheses
- Resolve cluster vs robust SE issue

### Before Next Week's Lab Meeting
- Prepare progress report for H-003 and H-007
- Update all hypothesis statuses
- Prepare next phase plan

---

## ⚠️ Risks & Warnings

- **🔴 HIGH**: April deadline approaching, need to accelerate Phase 2
- **🟡 MEDIUM**: H-003 and H-007 theory may overlap, needs clarification
- **🟢 LOW**: Computational resources sufficient

---

## 📊 Statistics

- **Session count**: 12
- **Work hours**: ~18 hours
- **Hypotheses generated**: 7
- **Validated hypotheses**: 2
- **Pass rate**: 28.6% (2/7)
- **Average Elo**: 1425

---

## 📚 Context Reminder

- **Domain**: domains/stats-theory/ (high-dimensional statistical theory)
- **PI preference**: Theoretical clarity > computational efficiency
- **Resource constraints**: Public data only, single-machine computation sufficient
- **Methodology**: Frequentist primary, Bayesian supplementary
```

### 3. Interactive Q&A (Optional)

Lab Manager can ask:
```
📊 Progress report generated

Want to view details?
- [1] View specific hypothesis details (e.g., H-003)
- [2] View recent phase records
- [3] View complete hypothesis list
- [4] View timeline and deadlines
- [5] Start working directly

Your choice: _
```

## Output Format

### Concise Version (Default)
Suitable for quick scanning:
```
# Progress Snapshot

📍 Phase 2 - Hypothesis Validation (67%)
🎯 In Progress: H-003 review
🏆 Top Hypothesis: H-003 (1580 Elo)
✅ Recently Completed: H-001 validation passed

⏭️ Next Steps:
1. Complete H-003 Methodologist review
2. Update Elo rankings
3. Start H-007 review

⚠️ Risk: April deadline approaching
```

### Detailed Version
Includes all information (as in the complete report above)

## Usage Examples

### Scenario 1: Session Start

```
User: /progress

Lab Manager:
[Generate progress report]

Welcome back! Last time was 2 days ago (Session #11)

Current status:
- Phase 2 in progress (67%)
- H-003 awaiting your Methodologist review
- 2 open issues

Suggested starting point:
/review-hypothesis H-003

Need to review anything first?
```

### Scenario 2: Unsure What to Do

```
User: /progress

Lab Manager:
[Generate progress report]

Current top priority tasks:
1. 🔥 Complete H-003 review (priority HIGH)
2. Resolve sample size issue
3. Prepare for next week's Lab Meeting

Which one would you like to start with?
```

### Scenario 3: Reporting to PI

```
User: /progress

Lab Manager:
[Generate detailed report]

This report includes:
- 7 hypotheses, 2 validated
- Phase 2 progress 67%
- 3 open issues
- Risk warnings

You can:
- Export as PDF report
- Copy to meeting prep document
- Generate slides outline
```

## Advanced Features

### Filter Options

```bash
# Only show high priority hypotheses
/progress --priority high

# Only show specific status
/progress --status "Under Review"

# Only show risks and issues
/progress --warnings

# Only show next step suggestions
/progress --next-steps
```

### Comparison Feature

```bash
# Compare with last week
/progress --compare last-week

# Compare with target
/progress --compare target

Output:
Progress changes:
- Hypotheses: 5 → 7 (+2)
- Validated: 1 → 2 (+1)
- Average Elo: 1350 → 1425 (+75)
```

### Export Feature

```bash
# Export as Markdown
/progress --export md

# Export as slides outline
/progress --export slides

# Generate weekly report
/progress --export weekly-report
```

## Relationship with Other Commands

```
[Session Start]
        ↓
/progress           ← View current status
        ↓
[Choose work to do]
        ↓
[Execute commands: brainstorm, review, execute, verify]
        ↓
[Before Session End]
        ↓
/update-state      ← Update status
        ↓
[Next Session]
        ↓
/progress          ← Loop
```

## Data Source Priority

If files conflict, priority is:
1. **STATE.md** - Highest priority (most recent single source of truth)
2. **hypotheses/HYPOTHESES.md** - Hypothesis rankings
3. **.planning/phases/** - Phase details
4. **RESEARCH_PLAN.md** - Plans and timeline
5. **meeting_notes/** - Meeting decisions

## Automatic Reminders

Lab Manager will check and remind during `/progress`:

- ⏰ Deadlines approaching (<2 weeks)
- 📉 Progress behind schedule
- ⚠️ HIGH priority issues unresolved
- 📅 Lab Meeting time
- 🎯 Milestones achieved

## Best Practices

### Every Session Start
```
1. /progress          # Quick status check
2. [Read suggested next steps]
3. [Choose task and begin]
```

### Lost Mid-Session
```
1. /progress --next-steps   # See what should be done
2. [Get back on track]
```

### When Reporting Needed
```
1. /progress --export weekly-report
2. [Generate formatted progress report]
3. [Share with team/PI]
```

## Notes

- `/progress` is **read-only**, does not modify any files
- Information comes from STATE.md and related files
- If STATE.md is outdated, run `/update-state` first
- Recommended to run once at the start of each session

## Output Example (Concise Version)

```
╔══════════════════════════════════════════╗
║     📊 Research Progress Snapshot      ║
╚══════════════════════════════════════════╝

📍 Phase 2 - Hypothesis Review (67%)
⏱️  Session #12 | 2024-01-27 15:30

🎯 Current Focus:
   H-003 | Minimax rates | 1580 Elo | 🔄 Review

🏆 Top 3 Hypotheses:
   1️⃣  H-003 (1580) ████████████░░░ 67%
   2️⃣  H-001 (1520) ██████████████ 100% ✅
   3️⃣  H-007 (1450) ████░░░░░░░░░░ 33%

✅ Last 3 Wins:
   • H-003 Experimentalist review
   • H-007 draft completed
   • H-001 verification passed (95%)

❗ Blockers (1):
   • H-003 sample size unclear

⏭️  Next Steps:
   1. /review-hypothesis H-003 (Methodologist)
   2. Update Elo rankings
   3. Power analysis for H-003

⚠️  Deadline: 9 weeks until submission

Type /progress --detailed for full report
```

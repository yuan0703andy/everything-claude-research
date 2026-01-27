---
name: lab-meeting
description: Weekly lab meeting - Progress review, Elo tournament, ranking updates, next week planning
argument-hint: [optional: specific agenda or --emergency/--project-kickoff/--project-closure]
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

<lab_meeting_command>

# /lab-meeting Command

Execute virtual Lab Meeting to review progress, discuss hypotheses, update rankings.

## Purpose

This is the research team's regular synchronization mechanism, combining:
- **Standard Agenda**: Structured meeting with 8 fixed topics
- **Elo Tournament**: Pairwise comparison-based hypothesis ranking system
- **GSD Goal-backward Review**: Check whether research goals are truly being achieved
- **Decision Recording**: Clear action items and owners

Ensures all team members have consensus on project status, hypothesis priorities, and resource allocation.

## When to Use
- ✅ Recommended: Weekly at fixed time (e.g., Monday morning)
- ✅ Major project milestones
- ✅ When team collective decisions are needed
- ✅ When new hypotheses need review
- ✅ When Elo rankings need updating

---

## Workflow

```
User: /lab-meeting
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Generate Status Report         │
    │  - Pull from STATE.md                   │
    │  - Aggregate hypothesis status          │
    │  - Calculate metrics                    │
    │  - Check domain-specific signals        │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Hypothesis Tournament          │
    │  - Compare top hypotheses pairwise      │
    │  - Apply domain-aware criteria          │
    │  - Update Elo rankings                  │
    │  - Track match history (W-L-D)          │
    │  - Identify priorities                  │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Blockers & Decisions           │
    │  - Surface blockers                     │
    │  - Present decisions to PI              │
    │  - Goal-backward check                  │
    │  - Domain standards compliance check    │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 4: Plan Next Week                 │
    │  - Prioritize tasks                     │
    │  - Assign to agents                     │
    │  - Set targets                          │
    │  - Update HYPOTHESES.md + STATE.md      │
    └─────────────────────────────────────────┘
```

---

## Standard Agenda

### 1. Opening & Action Item Review (5 minutes)
**Chair**: Coordinator

**Content**:
- Review last week's meeting decisions
- Check action item completion status
- Identify obstacles for incomplete items

**Format**:
```markdown
### Completed
- [x] [Item] - Owner - ✅ Done

### Incomplete
- [ ] [Item] - Owner
  - Reason: [...]
  - New deadline: [...]
  - Support needed: [...]
```

---

### 2. Project Status Reports (5-10 minutes)
**Chair**: Coordinator
**Participants**: All members

**Content**:
- Status update for each active project
- Progress percentage
- Milestones completed this week
- Issues and risks encountered
- Resource utilization
- **Domain-specific signals** (e.g., "lower bound derived", "identification strategy validated")

**Format**:
```
Project A:
- Status: 🟢 On track / 🟡 At risk / 🔴 Blocked
- Progress: XX%
- This Week: [What was accomplished]
- Next Week: [What is planned]
- Risks: [Any blockers]
- Goal check: [Are we advancing toward the goal?]
- Domain signals: [Critical domain-specific status]
  - For stats-theory: Lower bound status, proof completeness, simulation progress
  - For policy-making: Identification strategy status, data access, mechanism testing
```

---

### 3. Hypothesis Review (20-30 minutes)
**Chair**: Appropriate Senior Postdoc based on hypothesis type

Review new hypotheses or those with major updates:

#### 3.1 Context Loading (Automatic Execution)

**CRITICAL**: Before reviewing any hypothesis, the system must load and inject domain knowledge.

**Step 3.1A: Identify hypothesis and domain**
```markdown
1. Read hypothesis file: hypotheses/proposals/H-XXX-*.md
2. Extract domain field from hypothesis frontmatter
3. Identify which domain knowledge to load
```

**Step 3.1B: Load domain knowledge (MUST READ FULL FILE)**
```markdown
Read /Users/andyhou/research/domains/{domain}/DOMAIN.md

This file contains (600+ lines):
- Core theoretical frameworks
- Proof techniques / Research methods
- Publication standards
- Domain-specific evaluation criteria
- Red flags and acceptance signals
```

**Step 3.1C: Package domain knowledge for agents**
```markdown
Domain knowledge will be injected into agent context when spawning team members for hypothesis discussion.
```

#### 3.2 Hypothesis Report
**Theorist** reports (with domain knowledge):
- Theoretical motivation of hypothesis
- Core claim and mechanism
- Relationship to existing literature
- Why this hypothesis is important
- **Domain-specific**: What theoretical framework applies? (e.g., minimax for stats, mechanism theory for policy)

#### 3.3 Feasibility Assessment
**Experimentalist** assesses (with domain knowledge):
- Verification plan
- Resource requirements (data, time, computation)
- Technical risks
- Expected timeline
- **Domain-specific**:
  - For stats-theory: Can the estimator be implemented? Simulation design? Numerical stability?
  - For policy-making: Data access feasible? Case selection? Measurement validity?

#### 3.4 Methodological Opinion
**Methodologist** provides (with domain knowledge):
- Methodological appropriateness
- Potential biases and pitfalls
- Domain standard compliance
- Expected reviewer concerns
- **Domain-specific evaluation**:
  - For stats-theory: Proof completeness? Lower bound? Assumptions stated?
  - For policy-making: Identification strategy? Confounders addressed? Mechanism observable?

#### 3.5 Discussion & Decision
**All members** discuss:
- Strengths and weaknesses
- Comparison with other hypotheses (prepare for Elo tournament)
- Priority assessment
- Resource allocation
- **Domain standard compliance**: Does this meet field-specific publication standards?

**Decision**:
- ✅ Proceed: Allocate resources, begin verification
- 🔄 Revise: Needs adjustment before proceeding
- ⏸️ Defer: Not pursuing now, but keep on record
- ❌ Abandon: No longer consider

**Domain-Specific Decision Criteria**:
```markdown
For stats-theory:
- ✅ Proceed if: Clear theorem, rate derived, lower bound strategy identified, assumptions stated
- ❌ Reject if: No lower bound plan, proof has gaps, assumptions unstated (red flags from DOMAIN.md)

For policy-making:
- ✅ Proceed if: Clear mechanism, identification strategy specified, threats to validity discussed
- ❌ Reject if: No identification strategy, confounders ignored, mechanism not observable (red flags from DOMAIN.md)
```

---

### 4. Hypothesis Ranking Update - **Elo Tournament System** (10-15 minutes)
**Chair**: Coordinator
**Method**: Pairwise comparison + Elo calculation

#### 4.1 Tournament Flow

If multiple hypotheses need ranking:

**Step 1**: Select comparison pairs
- Prioritize hypotheses with similar Elo
- Or new hypothesis vs established hypothesis
- Run 2-4 matches per meeting

**Step 2**: Multi-dimensional evaluation with domain awareness
Team evaluates based on the following dimensions:

| Dimension | Weight | Evaluation Question | Domain Considerations |
|-----------|--------|---------------------|----------------------|
| **Novelty** | 25% | How novel is this hypothesis? Does it open new directions? | Does it challenge existing theoretical frameworks? |
| **Importance** | 25% | If true, what is the impact on the field? | **DOMAIN-AWARE**: For stats, does it achieve optimal rates? For policy, does it explain major policy outcomes? |
| **Testability** | 20% | How feasible is verification? Are resource requirements reasonable? | Can domain standards be met with available resources? |
| **Progress** | 15% | What is current progress? Is it stuck? | **DOMAIN-AWARE**: For stats, is lower bound derived? For policy, is identification clear? |
| **Confidence** | 15% | How confident are we in this hypothesis? | Based on theoretical rigor and preliminary evidence |

**Domain-Specific Scoring Guidance**:

```markdown
## For Statistical Theory Projects:

**Importance scoring**:
- 5/5: Achieves minimax optimal rate + tight constant, matching lower bound
- 4/5: Achieves optimal rate but sub-optimal constant
- 3/5: Rate is sub-optimal by log factor
- 2/5: Rate is sub-optimal by polynomial factor
- 1/5: No clear rate optimality

**Progress scoring**:
- 5/5: Lower bound proven + upper bound achieved + simulation complete
- 4/5: Lower bound proven + upper bound achieved
- 3/5: Lower bound proven OR upper bound achieved
- 2/5: Rate derived but no proof
- 1/5: Still working on problem formulation

## For Policy Research Projects:

**Importance scoring**:
- 5/5: Challenges dominant theory + explains major policy outcomes
- 4/5: Extends existing theory + important policy implications
- 3/5: Incremental theoretical contribution
- 2/5: Confirms existing theory
- 1/5: Limited theoretical contribution

**Progress scoring**:
- 5/5: Identification validated + mechanism tested + robustness checks passed
- 4/5: Identification strategy specified + preliminary evidence
- 3/5: Identification strategy proposed
- 2/5: Mechanism articulated but identification unclear
- 1/5: Still developing theoretical framework
```

**Step 3**: Calculate results
```python
# Weighted scoring
h1_score = sum(score[dim] * weight[dim] for dim in dimensions)
h2_score = sum(score[dim] * weight[dim] for dim in dimensions)

winner = h1 if h1_score > h2_score else h2
margin = abs(h1_score - h2_score)

# Elo update
K = 32  # K-factor
expected_h1 = 1 / (1 + 10^((elo_h2 - elo_h1) / 400))

if h1 wins:
    elo_h1_new = elo_h1 + K * (1 - expected_h1)
    elo_h2_new = elo_h2 + K * (0 - (1 - expected_h1))
else:
    elo_h1_new = elo_h1 + K * (0 - expected_h1)
    elo_h2_new = elo_h2 + K * (1 - (1 - expected_h1))
```

#### 4.2 Tournament Output Format

```markdown
## 🏆 Hypothesis Tournament

### This Week's Matches

**Match 1: H-003 vs H-001**
**Domain**: stats-theory

| Criterion | H-003 | H-001 | Winner | Rationale |
|-----------|-------|-------|--------|-----------|
| Novelty (25%) | 4/5 | 3/5 | H-003 | New sparsity regime |
| Importance (25%) | 5/5 | 4/5 | H-003 | **Minimax optimal + matching lower bound** |
| Testability (20%) | 3/5 | 4/5 | H-001 | H-001 easier to simulate |
| Progress (15%) | 4/5 | 3/5 | H-003 | **Lower bound already proven** |
| Confidence (15%) | 4/5 | 3/5 | H-003 | Strong theoretical foundation |
| **Weighted Total** | **4.10** | **3.55** | **H-003** | |

**Domain Analysis**:
- H-003 has proven lower bound (critical for stats-theory)
- H-003 achieves minimax optimal rate
- H-001 lacks lower bound (red flag per DOMAIN.md)

**Elo Update**:
- H-003: 1780 → 1810 (+30)
- H-001: 1850 → 1820 (-30)

**Rationale**: H-003 has higher theoretical importance and novelty. While H-001 is slightly more testable, H-003 meets critical domain standards (lower bound proven) which is essential for publication in top theory journals.

---

**Match 2: H-007 vs H-002**
**Domain**: policy-making

| Criterion | H-007 | H-002 | Winner | Rationale |
|-----------|-------|-------|--------|-----------|
| Novelty (25%) | 5/5 | 3/5 | H-007 | Challenges dominant theory |
| Importance (25%) | 5/5 | 4/5 | H-007 | **Explains major policy outcome** |
| Testability (20%) | 4/5 | 3/5 | H-007 | **Clear identification via RDD** |
| Progress (15%) | 4/5 | 3/5 | H-007 | **Identification strategy validated** |
| Confidence (15%) | 3/5 | 4/5 | H-002 | H-002 more established |
| **Weighted Total** | **4.40** | **3.40** | **H-007** | |

**Domain Analysis**:
- H-007 has clear identification strategy (RDD)
- H-007 addresses confounders explicitly
- H-002 identification strategy weaker

**Elo Update**:
- H-007: 1650 → 1685 (+35)
- H-002: 1680 → 1645 (-35)

### Updated Rankings (Top 10)

| Rank | Δ | ID | Elo | Title | Status | W-L-D | Domain |
|------|---|-----|-----|-------|--------|-------|--------|
| 1 | ↑1 | H-003 | 1810 | [Title] | Ready | 6-1-0 | stats-theory |
| 2 | ↓1 | H-001 | 1820 | [Title] | Review | 5-2-1 | stats-theory |
| 3 | → | H-007 | 1685 | [Title] | Draft | 5-2-0 | policy-making |
| 4 | ↑2 | H-002 | 1645 | [Title] | Testing | 3-2-0 | policy-making |
| 5 | ↓1 | H-009 | 1650 | [Title] | Draft | 2-2-1 | stats-theory |
| ... | | | | | | | |

### Tournament Summary
- **Matches this week**: 2
- **Biggest mover**: H-007 (+35 Elo, ↑2 ranks)
- **Most wins**: H-003 (6-1-0)
- **New entrant**: H-012 (Initial Elo: 1200)
- **Domain distribution**: 6 stats-theory, 4 policy-making in top 10

### Domain-Specific Rankings

**Stats Theory Top 3**:
1. H-003 (1810) - Lower bound proven ✓
2. H-001 (1820) - Lower bound missing ✗
3. H-009 (1650) - Lower bound in progress

**Policy Making Top 3**:
1. H-007 (1685) - Identification clear (RDD) ✓
2. H-002 (1645) - Identification unclear ✗
3. H-011 (1590) - Mechanism specified ✓
```

---

### 5. Problem Discussion (10-15 minutes)
**Chair**: PI (or most relevant Senior Postdoc)

Discuss issues requiring collective wisdom:
- Technical problems that are stuck
- Directional questions
- Resource conflicts
- Cross-project coordination
- **Domain-specific concerns** (e.g., "Can we relax this assumption?", "Is this identification strategy credible?")

**Goal-Backward Check**:
- Are our actions truly advancing research goals?
- Or are we just completing tasks without achieving goals?

**Domain-Aware Problem Identification**:
```markdown
## Critical Issues (Red Flags)

**Stats Theory**:
- Hypothesis lacks lower bound (critical blocker)
- Proof has logical gaps
- Assumptions unrealistic or unstated
- Computational complexity ignored

**Policy Making**:
- No identification strategy specified
- Selection bias not addressed
- Confounders not discussed
- Mechanism not observable or testable
```

---

### 6. Resources & Timeline (5 minutes)
**Chair**: Coordinator

- Resource allocation review
- Upcoming deadlines
- Next week's priority tasks
- External assistance needed
- **Domain-specific resource needs** (e.g., computational resources for simulations, data access for policy research)

---

### 7. Next Week Plan (5 minutes)
**Chair**: Coordinator

Each member briefly states next week's goals:
- **Theorist**: [Next week tasks]
- **Experimentalist**: [Next week tasks]
- **Methodologist**: [Next week tasks]
- **Coordinator**: [Next week tasks]

**Domain-aware task prioritization**:
- For stats-theory: Prioritize lower bound derivation over simulation if not yet proven
- For policy-making: Prioritize identification strategy over data collection if not yet clear

---

### 8. Closing & Documentation (5 minutes)
**Chair**: Coordinator

- Summarize decisions
- Confirm action items and owners
- Set next meeting time
- Suggest strategic compaction (`/compact`) if appropriate

---

## Meeting Output

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 LAB MEETING ► [Date]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## 📊 Progress Report

### This Week
- Hypotheses generated: [N]
- Reviews completed: [N]
- Moved to testing: [N]
- Results obtained: [N]

### Pipeline Status
```
Draft (3) → Review (2) → Ready (5) → Testing (1) → Complete (4)
```

### Key Accomplishments
1. [Accomplishment]
2. [Accomplishment]

### Domain-Specific Highlights
**Stats Theory**:
- H-003: Lower bound proven using Fano's method ✓
- H-005: Simulation 75% complete

**Policy Making**:
- H-007: Data access granted for RDD analysis ✓
- H-011: Mechanism testing in progress

─────────────────────────────────────────────────────

## 🏆 Hypothesis Tournament

### Current Top 5 (by Elo)
| Rank | ID | Title | Elo | Change | Domain |
|------|----|-------|-----|--------|--------|
| 1 | H-003 | [Title] | 1810 | ↑1 (+30) | stats-theory |
| 2 | H-001 | [Title] | 1820 | ↓1 (-30) | stats-theory |
| 3 | H-007 | [Title] | 1685 | ↑2 (+35) | policy-making |
| 4 | H-002 | [Title] | 1645 | ↓1 (-35) | policy-making |
| 5 | H-009 | [Title] | 1650 | ↓1 | stats-theory |

### This Week's Matches

**Match 1: H-003 vs H-001** (stats-theory)
Comparison: [5 criteria, weighted, domain-aware]
Winner: H-003
Elo Change: H-003 +30, H-001 -30
Reason: H-003 has proven lower bound (critical for stats theory), achieves minimax optimal rate
Domain Impact: H-003 meets publication standards, H-001 lacks lower bound (red flag)

**Match 2: H-007 vs H-002** (policy-making)
Comparison: [5 criteria, weighted, domain-aware]
Winner: H-007
Elo Change: H-007 +35, H-002 -35
Reason: H-007 has clear identification strategy (RDD), H-002 identification unclear
Domain Impact: H-007 meets causal inference standards

### Tournament Statistics
- Total hypotheses ranked: 15
- Matches played this week: 2
- Cumulative matches: 47
- Biggest upset: [If any]
- Domain-aware upsets: H-001 dropped despite higher Elo due to missing lower bound

─────────────────────────────────────────────────────

## 🚧 Blockers & Decisions

### Current Blockers
| Blocker | Impact | Needed From | Deadline | Domain Issue |
|---------|--------|-------------|----------|--------------|
| [Description] | [What's blocked] | [Who can resolve] | [Date] | [Domain flag if applicable] |

### Domain-Specific Blockers
**Stats Theory**:
- H-001: Missing lower bound (CRITICAL - publication blocker)
- H-009: Proof gap in convergence argument

**Policy Making**:
- H-002: Identification strategy unclear (CRITICAL)
- H-011: Data access delayed

### Decisions for PI

**Decision 1: [Question]**
- Option A: [Description] - Pros: [...] - Cons: [...]
- Option B: [Description] - Pros: [...] - Cons: [...]
- **Domain considerations**: [How each option aligns with domain standards]
- **Recommendation**: [Which option and why]

**Decision 2: [Question]**
[Same structure]

─────────────────────────────────────────────────────

## 🎯 Goal-Backward Check

### Research Goals (from PROJECT.md)
1. [Goal 1]
   - Progress: ✅ On track / ⚠️ Needs attention / ❌ Off track
   - Evidence: [What shows progress?]
   - Domain standard: [Are we meeting field-specific requirements?]

2. [Goal 2]
   - Progress: [Status]
   - Evidence: [...]

### Domain Standard Compliance Check

**Stats Theory Goals**:
- [ ] Lower bounds proven for main results
- [ ] Proofs complete and rigorous
- [ ] Assumptions explicitly stated
- [ ] Computational complexity analyzed

**Policy Making Goals**:
- [ ] Identification strategies specified
- [ ] Causal mechanisms clearly articulated
- [ ] Threats to validity discussed
- [ ] Measurement approaches justified

### Misalignments Identified
- [Activity that's not advancing goals]
- [Activity that violates domain standards]
- [Suggested correction]

─────────────────────────────────────────────────────

## 📅 Plan for Next Week

### Priority Tasks
| # | Task | Owner | Target | Depends On | Goal Link | Domain Priority |
|---|------|-------|--------|------------|-----------|-----------------|
| 1 | [Task] | [Agent] | [Date] | - | [Which goal] | [If domain-critical] |
| 2 | [Task] | [Agent] | [Date] | Task 1 | [Which goal] | [If domain-critical] |

### Domain-Prioritized Tasks

**High Priority (Domain Critical)**:
- [ ] [Stats]: Derive lower bound for H-001 (publication blocker)
- [ ] [Policy]: Specify identification strategy for H-002 (publication blocker)

**Medium Priority**:
- [ ] [Task]
- [ ] [Task]

### Goals
- [ ] [Goal 1]
- [ ] [Goal 2]
- [ ] [Goal 3]

─────────────────────────────────────────────────────

## 💡 Open Discussion

[Space for PI to raise topics, ask questions, provide direction]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Decision Summary
1. [Decision 1]
2. [Decision 2]
3. [Decision 3]

## Action Items
- [ ] [Task] - [Owner] - [Deadline] - Priority: [High/Med/Low]
- [ ] [Task] - [Owner] - [Deadline] - Priority: [High/Med/Low]

## Domain-Specific Action Items
**Stats Theory**:
- [ ] [Critical task - e.g., "Prove lower bound for H-001"]

**Policy Making**:
- [ ] [Critical task - e.g., "Specify identification for H-002"]

**Next Meeting**: [Date and Time]
**Compaction Suggestion**: Yes / No - [Rationale]
```

---

## State Updates

### 1. Update HYPOTHESES.md

```markdown
## Elo Rankings

### Active Hypotheses (Top 10)
[Updated rankings from tournament]

### Elo History (Last 10 Changes)
| Date | Match | Winner | Loser | Δ | Domain | Reason |
|------|-------|--------|-------|---|--------|--------|
| [Date] | H-003 vs H-001 | H-003 | H-001 | +30/-30 | stats | Lower bound proven |
| [Date] | H-007 vs H-002 | H-007 | H-002 | +35/-35 | policy | Clear identification (RDD) |

### Domain-Specific Rankings

**Stats Theory Top 5**:
[List with domain compliance status]

**Policy Making Top 5**:
[List with domain compliance status]
```

### 2. Update STATE.md

```markdown
## Last Lab Meeting
Date: [Today]
Key Decisions:
1. [Decision]
2. [Decision]

## Elo Rankings (Top 3)
1. H-003: 1810 (stats-theory) - Lower bound ✓
2. H-001: 1820 (stats-theory) - Lower bound ✗
3. H-007: 1685 (policy-making) - Identification ✓

## Domain Compliance Alerts
**Critical Issues**:
- H-001: Missing lower bound (stats-theory red flag)
- H-002: Unclear identification (policy-making red flag)

## Next Week's Plan
[From meeting output]

## Goal Progress
[From goal-backward check]
```

---

## Interactive Elements

During lab meeting, PI can:

| Command | Action |
|---------|--------|
| "Compare H-001 and H-005" | Run head-to-head comparison with domain-aware criteria |
| "Why is H-003 ranked first?" | Explain Elo position, match history, and domain compliance |
| "Prioritize H-007" | Manually boost priority (+50 Elo bonus) |
| "Drop H-002" | Archive hypothesis, remove from rankings |
| "What's blocking H-001?" | Show blockers including domain-specific issues |
| "Plan for [topic]" | Adjust next week's plan |
| "Replay H-003 vs H-001" | Re-evaluate past match with updated information |
| "Check domain standards for H-001" | Review domain compliance checklist |

---

## Special Situations

### Emergency Meeting
For urgent issues requiring immediate discussion:
```bash
/lab-meeting --emergency "Issue description"
```
- Use simplified agenda
- Focus only on urgent issue
- Quick decision-making, document decisions
- May skip tournament if time-critical

### Project Kickoff Meeting
When starting new project:
```bash
/lab-meeting --project-kickoff "[Project name]"
```
- Focus on project goals, resource allocation, timeline setting
- Initial hypothesis ranking (if any)
- Set milestones
- **Load domain knowledge** for project's domain
- Establish domain-specific success criteria

### Project Closure Meeting
When completing project:
```bash
/lab-meeting --project-closure "[Project name]"
```
- Review outcomes
- Summarize lessons learned
- Update knowledge base (DOMAIN.md if needed)
- Final Elo rankings
- Publish successful hypotheses
- **Document domain-specific insights** for future projects

---

## Meeting Cadence

- **Weekly**: Full lab meeting (all 8 agenda items)
- **Daily (optional)**: Quick standup via `/progress` (5-minute sync)
- **Ad-hoc**: `/lab-meeting --emergency` for critical decisions

---

## Best Practices

### Meeting Efficiency
- ⏱️ Control time, strictly follow agenda
- 🔍 Technical details can be discussed offline
- ⏭️ If discussion goes over time, mark as pending task
- 📊 Use visualization aids (charts, tables)

### Elo Tournament Best Practices
- 🎯 Run 2-4 matches per meeting (don't overdo it)
- ⚖️ Prioritize comparing hypotheses with similar Elo (more meaningful)
- 📝 Record comparison rationale (for future review)
- 🔄 Periodically re-evaluate high-ranking hypotheses (avoid Elo inflation)
- 🆕 New hypotheses start at Elo 1200, find position through matches
- **🎓 Apply domain-aware criteria** (lower bound for stats, identification for policy)
- **🚩 Recognize domain red flags** that should override Elo

### Decision Quality
- ✅ All decisions must have clear action items and owners
- ❓ When uncertain, admit uncertainty, avoid hasty decisions
- 📄 Document dissenting opinions, don't force consensus
- 🎯 Use Goal-backward thinking: Does this decision truly help achieve goals?
- **🎓 Domain standard compliance**: Does this meet field-specific publication standards?

### Team Collaboration
- 🔄 Rotate facilitation of different topics among team members
- 💬 Encourage constructive criticism
- 🎉 Celebrate progress and milestones
- 🤝 Cross-agent collaboration opportunities
- **🎓 Respect domain expertise**: Defer to Methodologist on domain standards

### Continuous Improvement
- 📅 Monthly review of meeting process effectiveness
- 🔧 Adjust agenda based on project needs
- 🎨 Maintain flexibility
- 📊 Track meeting metrics (duration, decision count, action item completion rate)
- **🎓 Update domain criteria** as field standards evolve

---

## Elo Tournament Deep Dive

### Why Elo for Research?

Traditional research priority ranking problems:
- ❌ Subjective scoring susceptible to recency bias
- ❌ Absolute scores hard to calibrate
- ❌ New vs old hypotheses hard to compare

Elo system advantages:
- ✅ **Relative comparison**: Pairwise comparison easier to judge accurately
- ✅ **Dynamic**: Rankings update dynamically with progress
- ✅ **Robust**: Few matches needed to find reasonable ranking
- ✅ **Transparent**: Every ranking change has rationale
- ✅ **Domain-aware**: Can incorporate field-specific criteria

### Elo Calculation Details

```python
def update_elo(elo_a, elo_b, score_a):
    """
    elo_a, elo_b: Current Elo ratings
    score_a: 1 if A wins, 0 if A loses, 0.5 if draw
    Returns: (new_elo_a, new_elo_b)
    """
    K = 32  # K-factor (sensitivity to new results)

    # Expected scores
    expected_a = 1 / (1 + 10**((elo_b - elo_a) / 400))
    expected_b = 1 - expected_a

    # Update
    new_elo_a = elo_a + K * (score_a - expected_a)
    new_elo_b = elo_b + K * ((1 - score_a) - expected_b)

    return new_elo_a, new_elo_b
```

### When to Use Draws (0.5-0.5)

Use draws when:
- Two hypotheses equally important but test different aspects
- Team cannot reach consensus
- Two hypotheses are complementary rather than competitive
- Both hypotheses fail to meet domain standards (both get draw vs penalty)

### Elo Interpretation

| Elo Range | Interpretation |
|-----------|----------------|
| 1800+ | Top hypotheses, should prioritize resources |
| 1600-1800 | Excellent hypotheses, worth serious consideration |
| 1400-1600 | Medium hypotheses, can queue |
| 1200-1400 | New hypotheses or unproven value |
| < 1200 | Weaker hypotheses, may need rethinking |

**Domain-Aware Interpretation**:
- **Stats theory**: Elo 1800+ should have lower bound proven or clear path to proof
- **Policy making**: Elo 1800+ should have clear identification strategy

---

## Important Notes

⚠️ **Lab Meeting is not a seminar**
- Don't dive deep into technical discussions
- Focus on information sync, decision-making, task allocation

⚠️ **Elo is a tool, not the goal**
- Elo helps decision-making but doesn't replace judgment
- Sometimes low-Elo hypotheses worth pursuing for strategic reasons
- Elo reflects current consensus, but consensus can be wrong

⚠️ **Domain standards override Elo**
- A high-Elo stats hypothesis without lower bound is still problematic
- A high-Elo policy hypothesis without identification is still weak
- Use domain red flags to identify critical issues regardless of ranking

⚠️ **Avoid Elo gaming**
- Don't select easy opponents to boost Elo
- Periodically re-evaluate high-ranking hypotheses
- Record comparison rationale for audit trail

⚠️ **Identify systemic issues**
- If no substantial progress for several weeks, need reflection
- If same hypothesis stuck at same stage, need intervention
- If resource allocation consistently misaligned with Elo, need review
- **If domain standards consistently violated, need process fix**

⚠️ **Meeting notes timeliness**
- Meeting notes should be organized promptly, ideally within 24 hours
- Action items should be distributed to owners immediately
- HYPOTHESES.md and STATE.md must be updated by meeting end

---

## Integration with Other Commands

**Before lab-meeting**:
- 📊 `/progress` - Get quick status overview
- 📝 Review STATE.md for context
- **📚 Check DOMAIN.md** for current domain standards

**During lab-meeting**:
- 🔍 `/review-hypothesis H-XXX` - If detailed review needed
- ✅ `/verify-results H-XXX` - If goal-backward check needed
- **🎓 Reference DOMAIN.md** for standards compliance

**After lab-meeting**:
- 📈 Check updated HYPOTHESES.md Elo rankings
- ✅ Assign action items to team
- 🗜️ Consider `/compact` if suggested by Coordinator
- **📝 Update domain alerts** if critical issues identified

---

## Example Meeting Flow

```bash
# Start meeting
/lab-meeting

# System automatically:
# 1. Loads STATE.md, HYPOTHESES.md
# 2. Identifies domains of active hypotheses
# 3. Loads relevant DOMAIN.md files (if multiple domains)
# 4. Generates status report with domain signals

# PI can interrupt for specific actions:
"Compare H-003 and H-001"  # Triggers domain-aware tournament match
"Why is H-007 stuck?"      # Investigate blocker with domain context
"Prioritize H-005"         # Manual Elo boost
"Check domain standards for H-001"  # Review compliance

# For hypothesis review (Section 3):
# System automatically injects domain knowledge into Theorist,
# Experimentalist, and Methodologist context

# Meeting ends with:
# - Updated HYPOTHESES.md (new Elo rankings + domain compliance status)
# - Updated STATE.md (decisions, next week plan, domain alerts)
# - meeting_notes/lab_meeting_[date].md
# - Action items distributed with domain priorities flagged
```

---

## Domain-Specific Meeting Variants

### Stats Theory Focus Meeting

When most active hypotheses are stats-theory:

**Agenda modifications**:
- Section 3: Emphasize proof technique discussion, lower bound strategies
- Section 4: Tournament criteria weight Importance (minimax optimality) higher
- Section 5: Focus on proof gaps, assumption justification, computational feasibility
- Goal-backward: Check against publication standards (Annals, JRSSB, JASA)

**Key questions**:
- What is the minimax rate?
- Is the lower bound proven?
- Are assumptions realistic?
- Is the proof complete?

### Policy Making Focus Meeting

When most active hypotheses are policy-making:

**Agenda modifications**:
- Section 3: Emphasize identification strategies, causal mechanisms
- Section 4: Tournament criteria weight Importance (policy impact) and Testability (data access) higher
- Section 5: Focus on identification challenges, confounders, measurement validity
- Goal-backward: Check against publication standards (APSR, AJPS, Policy Studies Journal)

**Key questions**:
- What is the causal mechanism?
- How is the effect identified?
- What are the threats to validity?
- Are policy implications thoughtful?

### Multi-Domain Meeting

When hypotheses span multiple domains:

**Agenda modifications**:
- Section 4: Compare within-domain first, then cross-domain if needed
- Different scoring criteria by domain
- Maintain separate domain-specific rankings alongside overall Elo
- Acknowledge that cross-domain comparisons are inherently harder

**Best practice**:
"H-003 (stats) vs H-007 (policy)" comparisons should focus on strategic importance to project rather than intrinsic quality, since domain standards differ.

</lab_meeting_command>

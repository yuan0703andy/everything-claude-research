---
name: brainstorm
description: Launch hypothesis generation workflow. Structured brainstorming + Theorist agent invocation.
argument-hint: <research question or topic>
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

<brainstorm_command>

# /brainstorm Command

Conduct structured brainstorming on a specific question to generate high-quality research hypotheses.

## Purpose

This is a structured hypothesis generation workflow that produces high-quality research hypotheses through multiple rounds of divergence and convergence. It combines:
- **Desktop seminar-style process**: 5-round structured discussion
- **Agent-driven execution**: Automated agent invocation
- **Goal-backward thinking**: Derive hypothesis requirements from research goals

## When to Use
- Starting a new project
- Encountering research bottleneck and needing new ideas
- Forming hypotheses after literature review
- Theorizing from preliminary data observations

## Prerequisites
Ensure you have:
1. ✅ Read relevant literature
2. ✅ Understood research question background
3. ✅ Prepared project DOMAIN.md (domain knowledge)
4. ✅ Clarified research constraints

---

## Workflow

```
User: /brainstorm [topic]
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Load Context                   │
    │  - Read domain DOMAIN.md                │
    │  - Read project PROJECT.md              │
    │  - Read existing hypotheses (if any)    │
    │  - Check STATE.md for current status    │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Structured Rounds              │
    │  Round 1: Divergent thinking (Theorist) │
    │  Round 2: Feasibility filter (Exp)      │
    │  Round 3: Refinement (Theorist+Exp)     │
    │  Round 4: Methods review (Method)       │
    │  Round 5: Documentation (Coordinator)   │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Output & State Update          │
    │  - Present hypotheses to user           │
    │  - Update HYPOTHESES.md                 │
    │  - Update STATE.md                      │
    │  - Suggest next command                 │
    └─────────────────────────────────────────┘
```

---

## Execution Steps

### Step 1: Context Loading (Automatic Execution)

**CRITICAL**: This step must be executed, not just commented. The following files MUST be read:

1. **Identify Domain**: Read `PROJECT.md` or `CLAUDE.md` to find the `domain:` field
2. **Load Domain Knowledge**: Read `/Users/andyhou/research/domains/{domain}/DOMAIN.md`
3. **Load Project Context**: Read current project's `PROJECT.md`
4. **Load Current State**: Read `STATE.md` if exists
5. **Load Existing Hypotheses**: Read `hypotheses/HYPOTHESES.md` if exists

**Implementation**:
```markdown
# STEP 1A: Identify domain from project
Read PROJECT.md or CLAUDE.md, extract domain field
Example: domain: stats-theory

# STEP 1B: Load domain knowledge (MUST READ FULL FILE)
Read /Users/andyhou/research/domains/stats-theory/DOMAIN.md
Store entire content for injection into agent context

# STEP 1C: Load project context
Read PROJECT.md for project-specific information

# STEP 1D: Load research state
Read STATE.md for current phase and status

# STEP 1E: Load existing hypotheses (if any)
Read hypotheses/HYPOTHESES.md if exists
```

**Output**: Context package containing:
- Full DOMAIN.md content (600+ lines for stats-theory)
- Project description and constraints
- Current research phase
- Existing hypotheses (if any)

---

### Step 2: Structured Brainstorming Rounds

#### Round 1: Divergent Phase (10 minutes)
**Lead**: Theorist

**Spawn Theorist with**:
```markdown
<task>
## Domain Knowledge (AUTO-INJECTED - FULL CONTENT)

[INJECT COMPLETE DOMAIN.md FILE HERE - ALL 600+ LINES]

This includes:
- Core theoretical frameworks (Decision Theory, Minimax Theory, High-Dimensional Statistics, etc.)
- Proof techniques toolbox (Fano, Assouad, Le Cam, Concentration Inequalities)
- Publication standards (Annals of Statistics, JRSSB, etc.)
- Review criteria and evaluation checklists
- Red flags and best practices
- Standard notation and terminology

## Research Topic
[User's input topic/question]

## Project Context
[From PROJECT.md - project description, constraints, timeline]

## Current Research State
[From STATE.md - current phase, completed work]

## Existing Hypotheses (if any)
[From HYPOTHESES.md - list of existing hypothesis IDs and titles]

## Request
**Mode**: Exploration (Divergent Thinking)

Generate 5-10 initial hypothesis ideas following these principles:

1. **No criticism at this stage** - Encourage bold attempts
2. **Each idea must include**:
   - One-sentence core claim
   - Why this idea is worth exploring
   - Preliminary theoretical basis (citing frameworks from Domain Knowledge above)
   - Connection to domain evaluation standards

3. **Use goal-backward thinking**:
   - From research goals, work backward to determine what findings would be most valuable
   - Consider: "If we prove this hypothesis, then what?"
   - Prioritize hypotheses that open new research directions

4. **Apply domain-specific thinking** (for stats-theory):
   - What is the statistical problem?
   - What is the parameter space and loss function?
   - What are the expected minimax rates?
   - Can we prove optimality with lower bounds?
   - What proof techniques from the toolbox apply?

**Output format**: For each idea, provide:
- **ID**: Idea-01, Idea-02, etc.
- **Title**: Brief descriptive title
- **Core Claim**: One sentence
- **Theoretical Basis**: Which frameworks/theories support this
- **Why Worth Exploring**: Research value and potential impact
- **Initial Thoughts**: Preliminary mechanism or approach
</task>
```

**Output**: `brainstorm_ideas_initial.md` with 5-10 raw ideas, each grounded in domain knowledge

---

#### Round 2: Feasibility Screening (10 minutes)
**Lead**: Experimentalist

**Tasks**:
- Rapidly assess testability of each idea
- Flag obviously infeasible ones (but document reasons)
- Identify resource-intensive ideas
- Raise feasibility questions

**Spawn Experimentalist with**:
- Domain knowledge (same injection as Theorist)
- Initial ideas from Round 1
- Task: Evaluate feasibility of each idea

**Output**: Update `brainstorm_ideas_initial.md` with feasibility annotations
- 🟢 Feasible
- 🟡 Requires additional resources
- 🔴 Currently infeasible

---

#### Round 3: Refinement and Deepening (15 minutes)
**Lead**: Theorist + Experimentalist collaboration

**Tasks**:
- Select 3-5 most promising ideas
- Structure them into formal hypothesis format
- Clearly define:
  - Core Claim
  - Theoretical Mechanism
  - Observable Predictions
  - Testability / Verification Method
  - Alternative Explanations

**Spawn Both Agents** with domain knowledge and task to formalize selected ideas

**Output**: 3-5 formal hypothesis proposal files in `hypotheses/proposals/`

---

#### Round 4: Methodological Review (10 minutes)
**Lead**: Methodologist

**Tasks**:
- Conduct rapid methodological review of each hypothesis
- Identify potential biases and pitfalls
- Ensure compliance with domain standards (from DOMAIN.md evaluation checklists)
- Provide improvement suggestions
- Self-assess: Novelty, Importance, Testability (each 1-5 scale)

**Spawn Methodologist** with:
- Domain knowledge (including evaluation checklists)
- Hypothesis proposals from Round 3
- Task: Apply domain-specific review criteria

**Output**: Add methodological annotations and self-assessment scores to each hypothesis

---

#### Round 5: Documentation and Archiving (5 minutes)
**Lead**: Coordinator

**Tasks**:
- Archive all ideas (including rejected ones with reasons)
- Assign IDs to accepted hypotheses (H-XXX format)
- Set initial Elo scores (1200)
- Record brainstorming session metadata
- Update HYPOTHESES.md index
- Update STATE.md

**Output**:
- New hypotheses added to `hypotheses/proposals/` directory
- Updated `hypotheses/HYPOTHESES.md`
- Updated `STATE.md`
- Created `meeting_notes/brainstorm_session_[date].md` record

---

### Step 3: Output Presentation

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 BRAINSTORM ► [Topic]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Generated [N] hypothesis proposals:

## H-[NNN]: [Title]
**Core Claim**: [One sentence]
**Novelty**: [Brief statement]
**Self-Assessment**:
  - Novelty: [X/5]
  - Importance: [X/5]
  - Testability: [X/5]
**Initial Elo**: 1200

## H-[NNN]: [Title]
[Same structure]

─────────────────────────────────────────────────────

## Next Steps

Select hypotheses to pursue:
- "Review H-001 and H-003" → Starts review cycle
- "Expand on H-002" → Deeper exploration of one hypothesis
- "More ideas" → Generate additional hypotheses

Or: `/review-hypothesis H-001` to start formal review

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Output Structure

### 1. brainstorm_session_[date].md
```markdown
# Brainstorming Session - [Topic]

**Date**: [date]
**Topic**: [topic]
**Participants**: Theorist, Experimentalist, Methodologist, Coordinator

## Background
[Brief description of why this brainstorming session was conducted]

## Round 1: Initial Ideas (Divergent Thinking)
1. [Idea 1] - Theorist
2. [Idea 2] - Theorist
...

## Round 2: Feasibility Screening
| Idea | Feasibility | Experimentalist Comments |
|------|-------------|--------------------------|
| 1 | 🟢 | Data available, methods mature |
| 2 | 🟡 | Requires additional computational resources |
| 3 | 🔴 | Data unavailable |

## Round 3: Hypothesis Refinement
[List selected 3-5 hypotheses in structured format]

### H-001: [Title]
**Core Claim**: [...]
**Mechanism**: [...]
**Predictions**: [...]
**Testability**: [...]

## Round 4: Methodological Review
### H-001
- **Methodologist Assessment**:
  - Novelty: 4/5
  - Importance: 5/5
  - Testability: 3/5
- **Potential Issues**: [...]
- **Recommendations**: [...]

## Round 5: Documentation
- **Accepted Hypotheses**: H-001, H-002, H-003
- **Shelved Ideas**: [4, 5] - Reasons: [...]
- **Initial Elo**: All set to 1200

## Resolution
- ✅ H-001, H-002, H-003 entered into hypothesis pipeline
- 🔄 Next action: `/review-hypothesis H-001` for formal review
- 📊 STATE.md updated with new hypotheses
```

---

### 2. hypotheses/proposals/H-XXX-[slug].md
```markdown
---
id: H-XXX
title: "[Title]"
status: draft
created: [date]
elo: 1200
origin: "Brainstorming session on [topic]"
self_assessment:
  novelty: X/5
  importance: X/5
  testability: X/5
---

# Hypothesis H-XXX: [Title]

## Core Claim
[One-sentence core claim]

## Theoretical Basis
[Theoretical justification - why this hypothesis is theoretically sound]
[Must reference specific frameworks from DOMAIN.md]

## Mechanism
[Causal mechanism explanation - how X leads to Y]

## Predictions
If the hypothesis is true, we should observe:
1. [Specific observable prediction 1]
2. [Specific observable prediction 2]
3. [Specific observable prediction 3]

## Testability
**Verification Method**:
- Data needed: [...]
- Analysis approach: [...]
- Expected timeline: [...]

**Feasibility**: [Experimentalist preliminary assessment]

## Alternative Explanations
Besides this hypothesis, what else could explain the observations:
1. [Alternative explanation 1] - How to rule out
2. [Alternative explanation 2] - How to rule out

## Boundary Conditions
Under what conditions does the hypothesis hold:
- [Condition 1]
- [Condition 2]

## Key References
1. [Reference 1] - Provides theoretical foundation
2. [Reference 2] - Related empirical evidence

## Initial Assessment (from Round 4)
- **Novelty**: [X/5] - [Justification]
- **Importance**: [X/5] - [Justification]
- **Testability**: [X/5] - [Justification]
- **Initial Elo**: 1200

## Notes
[Any additional notes]
```

---

### 3. Update HYPOTHESES.md

Add new hypotheses to the index:

```markdown
### In Development
| ID | Title | Created | Last Update | Stage | Owner | Elo |
|----|-------|---------|-------------|-------|-------|-----|
| H-XXX | [Title] | [Date] | [Date] | Draft | Theorist | 1200 |
| H-YYY | [Title] | [Date] | [Date] | Draft | Theorist | 1200 |
```

---

### 4. Update STATE.md

```markdown
## Hypothesis Pipeline

### Recently Added (Brainstorm [Date])
| ID | Title | Stage | Initial Elo |
|----|-------|-------|-------------|
| H-XXX | [Title] | Draft | 1200 |
| H-YYY | [Title] | Draft | 1200 |

### Next Actions
- [ ] Review H-XXX via `/review-hypothesis H-XXX`
- [ ] Review H-YYY via `/review-hypothesis H-YYY`
```

---

## 🎯 Next Steps & Approval Gate

After hypotheses are generated, the system presents them to the user and waits for direction:

**⏸️ WAITING FOR USER DIRECTION**

The system will NOT automatically proceed to review. User must choose action:

**To proceed with review**:
- "Review H-001" → Launch formal review process via `/review-hypothesis H-001`
- "Review all" → Review all generated hypotheses sequentially
- "Prioritize H-003" → Mark as priority (+50 Elo boost)

**To generate more**:
- "More ideas" → Generate additional hypotheses with different angles
- "More variety" → Request more diverse approaches
- "Different angle" → Reframe the research question

**To refine existing**:
- "Expand on H-002" → Deep dive into specific hypothesis
- "Combine H-001 and H-002" → Synthesize multiple hypotheses
- "Revise H-003" → Send back to Theorist for refinement

**To reject and restart**:
- "None of these work" → Provide feedback, adjust approach
- "Different domain" → Pivot to different theoretical framework
- "Start over" → Complete reset with new research question

**CRITICAL**: Hypotheses remain in "Draft" status until user decides next action. No automatic review launch.

---

## User Interactions

| User Says | Action |
|-----------|--------|
| "Review H-001" | Route to `/review-hypothesis H-001` |
| "More ideas" | Re-invoke Theorist with "more variety" instruction |
| "Combine H-001 and H-002" | Ask Theorist to synthesize |
| "I like H-003 best" | Mark H-003 as priority, boost Elo by +50 |
| "None of these" | Ask for feedback, adjust approach, try different angle |
| "Expand on H-002" | Theorist deep dive on H-002 only |

---

## Usage Example

```bash
# In project directory
cd ~/research/projects/my-project

# Launch brainstorming
/brainstorm "Fundamental limits of statistical inference in high dimensions"

# System will automatically:
# 1. ✅ Load domains/stats-theory/DOMAIN.md (full content)
# 2. ✅ Convene research team for 5-round discussion
# 3. ✅ Generate meeting_notes/brainstorm_session_[date].md
# 4. ✅ Create new hypothesis files in hypotheses/proposals/
# 5. ✅ Update HYPOTHESES.md index
# 6. ✅ Update STATE.md
```

---

## Output Files Structure

After brainstorm:
```
project/
├── hypotheses/
│   ├── HYPOTHESES.md              # ✅ Updated with new hypotheses
│   └── proposals/
│       ├── H-001-[slug].md        # ✅ New proposal
│       ├── H-002-[slug].md        # ✅ New proposal
│       └── H-003-[slug].md        # ✅ New proposal
│
├── meeting_notes/
│   └── brainstorm_session_[date].md  # ✅ Session record
│
└── STATE.md                       # ✅ Updated with new hypotheses
```

---

## Best Practices

### Time Management
- One brainstorming session should not exceed one hour
- If stuck, do literature reading first and return
- "Need more background knowledge" is a valid conclusion

### Quality Standards
- Quality over quantity: 3 good hypotheses beat 10 mediocre ones
- Each hypothesis must have clear observable predictions
- Don't fear crazy ideas, but must explain why worth exploring

### Documentation Habits
- Record rejected ideas with reasons to avoid repetition
- Regularly review shelved ideas - technological progress may make them feasible
- Document thinking process, not just conclusions

### Goal-Backward Thinking
- Work backward from research goals: What findings would be most valuable?
- Consider: "If we prove this hypothesis, then what?"
- Prioritize hypotheses that open new research directions

---

## Important Warnings

⚠️ **This is NOT a substitute for deep literature reading**
- Should have solid literature foundation before brainstorming
- If unfamiliar with domain, use Literature-RA for systematic lit review first

⚠️ **Output is preliminary**
- Brainstorming output requires subsequent refinement
- Conduct formal review via `/review-hypothesis`
- Expect multiple iterations

⚠️ **No output is also a valid result**
- If a brainstorming session produces no viable hypotheses, this is normal
- May indicate need for more background reading or different angle

⚠️ **Maintain open mindset**
- Don't reject ideas prematurely
- Encourage cross-disciplinary connections
- Record all ideas, including "infeasible" ones

---

## Integration with Other Commands

**After brainstorm**:
- ➡️ `/review-hypothesis H-001` - Start formal review
- ➡️ `/progress` - Check hypothesis pipeline status
- ➡️ `/lab-meeting` - Present hypotheses in weekly meeting

**Before brainstorm**:
- Use Literature-RA for systematic lit review
- Ensure DOMAIN.md is up to date
- Check STATE.md for research phase

</brainstorm_command>

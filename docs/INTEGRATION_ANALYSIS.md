# Integration Analysis: Desktop → Current Framework

**Date**: 2026-01-27
**Purpose**: Compare desktop research-claude-framework with current ~/research framework and propose integration strategy

---

## Framework Comparison

### Agent Architecture

#### Current Framework (~/research) - **Flat Structure**
```
4 agents, all at same level:
├── agents/
│   ├── theorist.md (Opus)
│   ├── experimentalist.md (Opus)
│   ├── methodologist.md (Opus)
│   └── coordinator.md (Sonnet)
```

#### Desktop Framework - **Hierarchical Structure**
```
9 agents in 3-tier hierarchy:
├── agents/
│   ├── senior-postdocs/          # Strategic layer
│   │   ├── theorist.md (Opus)
│   │   ├── experimentalist.md (Opus)
│   │   └── methodologist.md (Opus)
│   │
│   ├── research-assistants/      # Execution layer ⭐ NEW
│   │   ├── literature-ra.md (Sonnet)
│   │   └── data-ra.md (Sonnet)
│   │
│   ├── lab-manager/
│   │   └── coordinator.md (Sonnet)
│   │
│   └── utilities/                # Integration layer ⭐ NEW
│       ├── verifier.md (Opus)
│       └── synthesizer.md (Sonnet)
```

**Key Addition**: Research Assistants layer for tactical execution

---

### Commands Comparison

#### Current: 11 Commands
```
Research-specific (3):
├── brainstorm.md
├── lab-meeting.md
└── review-hypothesis.md

GSD-inspired (4):
├── execute-analysis.md
├── verify-results.md
├── progress.md
└── update-state.md

From plugin (4):
├── checkpoint.md
├── eval.md
├── verify.md
└── analysis-review.md
```

#### Desktop: 5 Commands
```
Core workflow only:
├── brainstorm.md      (more detailed workflow)
├── lab-meeting.md     (Elo tournament system)
├── review-hypothesis.md (parallel review spawn)
├── verify-results.md  (goal-backward verification)
└── progress.md        (quick status)
```

**Design Philosophy Difference**:
- Current: More commands, GSD phase workflow
- Desktop: Fewer commands, agent-centric workflow

---

## Key New Features in Desktop Version

### 1. ⭐ Verifier Agent (Goal-Backward Verification)
**File**: `agents/utilities/verifier.md`

**What it does**:
- GSD-inspired goal-backward verification
- Starts from goals → derives must-haves → checks completion
- 4 verification types: Design, Pre-Execution, Post-Execution, Results
- Outputs verification reports with gap identification

**Example workflow**:
```
Goal: "Determine if X causes Y"
    │
    ├── Must-have: Measure of X (valid, reliable)
    ├── Must-have: Measure of Y (valid, reliable)
    ├── Must-have: Temporal precedence (X before Y)
    ├── Must-have: Control for confounds
    └── Must-have: Analysis that tests causal claim

For each must-have → Check if satisfied → Identify gaps
```

**Value**: Ensures task completion ≠ goal achievement

---

### 2. ⭐ Literature-RA (Systematic Literature Work)
**File**: `agents/research-assistants/literature-ra.md`

**What it does**:
- 4 research modes (borrowing from GSD researcher pattern):
  1. **Ecosystem Exploration**: Map the field (who, what, key debates)
  2. **Feasibility Research**: Prior attempts, lessons learned
  3. **Implementation Research**: Methods, measures, how-tos
  4. **Comparison Research**: Systematic alternative comparison

- Systematic search protocol (PRISMA-style)
- Source quality hierarchy (Tier 1-5)
- Literature review reports with synthesis

**Value**: Offloads systematic literature work from Theorist

---

### 3. ⭐ Data-RA (Reproducible Data Work)
**File**: `agents/research-assistants/data-ra.md`

**What it does**:
- 5-phase data workflow:
  1. Data Audit (availability assessment)
  2. Data Acquisition (raw data + integrity checks)
  3. Data Cleaning (scripted, documented)
  4. Feature Engineering (analysis-ready datasets)
  5. EDA (exploratory data analysis)

- Reproducibility-first: No manual edits, all scripted
- Quality standards and coding best practices
- Outputs: Data availability reports, cleaning logs, EDA reports

**Value**: Ensures data work is reproducible and well-documented

---

### 4. ⭐ Synthesizer (Integration Specialist)
**File**: `agents/utilities/synthesizer.md`

**What it does**:
- 4 synthesis modes:
  1. Literature Synthesis (integrate multiple reviews)
  2. Hypothesis Synthesis (cluster and map landscape)
  3. Results Synthesis (integrate findings)
  4. Meta-Synthesis (project learnings)

- Techniques: Thematic coding, matrix approach, narrative threading
- Outputs: Integrated narratives, hypothesis landscape maps

**Value**: Transforms fragments into coherent stories

---

### 5. 🔄 Updated Command Workflows

#### brainstorm.md (Desktop version)
- More structured: 4-step workflow explicitly defined
- Context loading phase (domain + project + existing hypotheses)
- State update after brainstorm
- Clear output file structure

#### lab-meeting.md (Desktop version)
- **Elo tournament system** with pairwise comparisons
- Weighted criteria (Novelty 25%, Importance 25%, Testability 20%, Progress 15%, Confidence 15%)
- Match history tracking
- Interactive elements during meeting

#### review-hypothesis.md (Desktop version)
- **Parallel agent spawn** (Experimentalist + Methodologist simultaneously)
- Aggregation layer for combined feedback
- Elo updates based on review outcomes (+30 approve, +10 partial, -10 revise, -30 reject)
- Maximum 3 revision iterations

#### verify-results.md (Desktop version)
- Integrated with Verifier agent
- Goal-backward verification framework
- Gap identification and remediation flow
- Multiple verification types (single hypothesis, phase, design-only, full project)

---

### 6. 📋 New Template: HYPOTHESES.md
**File**: `templates/HYPOTHESES.md`

**What it provides**:
- Centralized hypothesis index
- Pipeline summary visualization
- Elo rankings with match history (W-L-D records)
- Hypotheses grouped by status (Development, Review, Ready, Testing, Complete, Archived)
- Hypothesis clusters identification
- Quick reference: next actions, blockers
- Metrics: average time in stages, approval rates

**Value**: Single source of truth for hypothesis tracking

---

## Integration Recommendation

### Strategy: **Selective Integration with Modular Approach**

I recommend a **phased integration** rather than full replacement:

#### Phase 1: Core Additions (High Value, Low Disruption) ✅ RECOMMEND
```
1. Add Verifier agent
   - agents/lab-manager/verifier.md
   - Why: Goal-backward verification is critical for quality

2. Add HYPOTHESES.md template
   - templates/HYPOTHESES.md
   - Why: Centralized hypothesis tracking missing

3. Update verify-results command
   - commands/verify-results.md (integrate with Verifier)
   - Why: Currently lacks goal-backward approach
```

#### Phase 2: Research Assistant Layer (Medium Value, Medium Complexity) ⚠️ EVALUATE
```
4. Add Literature-RA (optional)
   - agents/research-assistants/literature-ra.md
   - Why: Systematic lit review is valuable BUT overlaps with Theorist
   - Decision: Do you want to separate strategic (Theorist) from tactical (RA)?

5. Add Data-RA (conditional)
   - agents/research-assistants/data-ra.md
   - Why: Reproducible data work BUT only if doing computational research
   - Decision: Is your research data-heavy?
```

#### Phase 3: Integration Utilities (Low Priority) 📝 DEFER
```
6. Add Synthesizer (optional)
   - agents/utilities/synthesizer.md
   - Why: Nice to have but not critical initially
   - Can be added later if integration needs emerge
```

#### Phase 4: Command Refinement (Case-by-case) 🔄 REVIEW
```
7. Update brainstorm command
   - Use desktop version's explicit workflow
   - Keep GSD integration from current version
   - Merge: Desktop structure + Current GSD concepts

8. Update lab-meeting command
   - Add Elo tournament system from desktop
   - Keep weekly cadence from current version

9. Update review-hypothesis command
   - Add parallel spawn from desktop
   - Keep integration with current agent structure
```

---

## Detailed Decision Matrix

| Component | Current | Desktop | Recommendation | Reason |
|-----------|---------|---------|----------------|---------|
| **Verifier** | ❌ None | ✅ Opus agent | **ADD** | Goal-backward verification critical |
| **Literature-RA** | ❌ None | ✅ Sonnet agent | **EVALUATE** | Depends on research style |
| **Data-RA** | ❌ None | ✅ Sonnet agent | **CONDITIONAL** | Only if computational |
| **Synthesizer** | ❌ None | ✅ Sonnet agent | **DEFER** | Nice-to-have, not urgent |
| **HYPOTHESES.md** | ❌ None | ✅ Template | **ADD** | Critical tracking tool |
| **brainstorm** | ✅ Basic | ✅ Detailed | **MERGE** | Desktop has better structure |
| **lab-meeting** | ✅ Basic | ✅ Elo tournament | **MERGE** | Elo system valuable |
| **review-hypothesis** | ✅ Basic | ✅ Parallel spawn | **MERGE** | Parallel spawn efficient |
| **verify-results** | ✅ GSD-inspired | ✅ Verifier integration | **UPDATE** | Desktop version superior |
| **progress** | ✅ GSD-inspired | ✅ Streamlined | **KEEP BOTH** | Both useful |
| **execute-analysis** | ✅ GSD | ❌ None | **KEEP** | Part of GSD workflow |
| **update-state** | ✅ GSD | ❌ None | **KEEP** | Part of GSD workflow |
| **checkpoint/eval** | ✅ From plugin | ❌ None | **KEEP** | Quality assurance |

---

## Proposed File Changes

### Files to ADD (from desktop):
```
✅ agents/lab-manager/verifier.md
✅ templates/HYPOTHESES.md
📊 agents/research-assistants/literature-ra.md (evaluate first)
📊 agents/research-assistants/data-ra.md (evaluate first)
📝 agents/utilities/synthesizer.md (defer)
```

### Files to UPDATE (merge desktop + current):
```
🔄 commands/brainstorm.md
   - Desktop's 4-step workflow
   - + Current's GSD integration

🔄 commands/lab-meeting.md
   - Desktop's Elo tournament system
   - + Current's weekly structure

🔄 commands/review-hypothesis.md
   - Desktop's parallel spawn
   - + Current's agent structure

🔄 commands/verify-results.md
   - Desktop's Verifier integration
   - + Current's GSD concepts
```

### Files to KEEP (current only):
```
✅ commands/execute-analysis.md
✅ commands/update-state.md
✅ commands/checkpoint.md
✅ commands/eval.md
✅ commands/verify.md
✅ commands/analysis-review.md
✅ templates/STATE_TEMPLATE.md
✅ templates/RESEARCH_PLAN_TEMPLATE.md
```

---

## Architecture Impact

### Current Philosophy: **GSD-Inspired Phase Workflow**
```
Phases: Context → Hypothesis → Analysis → Writeup
State Management: STATE.md as single source of truth
Commands: Map to phase transitions
```

### Desktop Philosophy: **Agent-Centric Hypothesis Pipeline**
```
Pipeline: Draft → Review → Ready → Testing → Complete
State Management: HYPOTHESES.md + STATE.md
Commands: Map to agent invocations
```

### Proposed: **Hybrid Architecture**
```
✅ Keep GSD phase workflow (current)
✅ Add hypothesis pipeline tracking (desktop)
✅ Add agent specialization hierarchy (desktop - conditional)
✅ Keep comprehensive command set (current)
✅ Add goal-backward verification (desktop)

Result: Best of both worlds
- Strategic: GSD phase thinking
- Tactical: Hypothesis pipeline management
- Quality: Goal-backward verification
```

---

## Recommendation Summary

### Immediate Actions (Do Now):
1. ✅ Add `agents/lab-manager/verifier.md`
2. ✅ Add `templates/HYPOTHESES.md`
3. ✅ Update `commands/verify-results.md` to use Verifier
4. ✅ Update `commands/lab-meeting.md` to add Elo tournament

### Evaluate Before Adding:
- Literature-RA: If you do heavy lit reviews, add it
- Data-RA: If you do computational research, add it
- Otherwise: Skip for now, can add later

### Defer:
- Synthesizer: Nice to have, but not critical initially

### Merge Strategy:
- Commands: Take desktop's structure, add current's GSD concepts
- Don't lose: STATE.md, phase workflow, GSD principles
- Gain: Goal-backward verification, Elo ranking, hypothesis tracking

---

## Next Steps

**Option A: Minimal Integration** (Safest)
```bash
# Add only Verifier + HYPOTHESES.md
# Lowest risk, highest value additions
```

**Option B: Moderate Integration** (Recommended)
```bash
# Add Verifier + HYPOTHESES.md + Elo tournament
# Update 4 core commands (brainstorm, lab-meeting, review, verify)
# Keep all current GSD infrastructure
```

**Option C: Full Integration** (Most Comprehensive)
```bash
# Add all new agents (Verifier, RAs, Synthesizer)
# Restructure to 3-tier hierarchy
# Update all commands
# Risk: More complexity, longer integration time
```

**My Recommendation**: **Option B** - Moderate Integration
- High value: Verifier, HYPOTHESES.md, Elo system
- Low disruption: Keep GSD workflow intact
- Extensible: Can add RAs later if needed

---

## Questions for You

Before proceeding with integration:

1. **Research Style**: Do you do heavy computational/data work?
   - Yes → Add Data-RA
   - No → Skip for now

2. **Literature Work**: Do you do systematic literature reviews?
   - Yes → Add Literature-RA
   - No → Skip for now

3. **Integration Scope**: Which option do you prefer?
   - Option A: Minimal (just Verifier + HYPOTHESES.md)
   - Option B: Moderate (+ Elo + command updates) ← **Recommended**
   - Option C: Full (+ all agents + restructure)

4. **Preserve Current Work**: Should we keep all 11 current commands?
   - Yes → Keep everything, add from desktop
   - No → Consolidate to streamlined set

Let me know your preferences and I'll execute the integration!

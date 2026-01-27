# Research Framework Improvements Summary

**Date**: 2025-01-27
**Source**: Patterns borrowed from everything-claude-code-main

---

## ✅ Implemented Improvements

### 1. **PROACTIVELY Triggers Added to Agent Descriptions**

Updated all core agents with explicit "Use PROACTIVELY" guidance:

#### Methodologist (`agents/methodologist.md`)
```yaml
Use PROACTIVELY:
- After hypothesis generation (check domain standards before review)
- Before execution planning (verify design meets domain requirements)
- After execution (verify results meet publication standards)
- When domain standard questions arise (clarify interpretation)
```

#### Theorist (`agents/theorist.md`)
```yaml
Use PROACTIVELY:
- When exploring new research directions (literature gaps, theoretical puzzles)
- After preliminary results suggest theoretical patterns
- When revising hypotheses based on Methodologist feedback
- When connecting disparate empirical findings (theory building)
```

#### Experimentalist (`agents/experimentalist.md`)
```yaml
Use PROACTIVELY:
- After hypothesis approval (design verification plan)
- Before execution (check resources and data availability)
- During execution (monitor progress, handle issues)
- When theoretical predictions need empirical validation
```

#### Verifier (`agents/verifier.md`)
```yaml
Use PROACTIVELY:
- Before execution (design verification - will this answer the question?)
- After execution (results verification - did we achieve the goal?)
- Before major transitions (phase gates - ready to proceed?)
- When gaps suspected (goal-backward check - what's missing?)
```

#### Coordinator (`agents/coordinator.md`)
```yaml
Use PROACTIVELY:
- Weekly for lab meetings (track progress, update rankings)
- After major milestones (document decisions, update state)
- When blockers identified (flag to PI, coordinate resolution)
- Before context overflow (suggest strategic compaction)
```

**Impact**: Agents now actively look for opportunities to help rather than waiting to be invoked.

---

### 2. **Explicit Approval Gates in Commands**

#### Added to `commands/brainstorm.md`

After hypothesis generation:
```markdown
⏸️ WAITING FOR USER DIRECTION

The system will NOT automatically proceed to review. User must choose action:

To proceed with review:
- "Review H-001" → Launch formal review process
- "Review all" → Review all generated hypotheses

To generate more:
- "More ideas" → Generate additional hypotheses
- "Different angle" → Reframe the research question

To refine existing:
- "Expand on H-002" → Deep dive into specific hypothesis
- "Combine H-001 and H-002" → Synthesize multiple hypotheses

CRITICAL: Hypotheses remain in "Draft" status until user decides next action.
```

#### Added to `commands/review-hypothesis.md`

After review completion:
```markdown
⏸️ WAITING FOR EXPLICIT APPROVAL

The system will NOT automatically proceed. User must explicitly respond:

To approve and proceed:
- "approve" or "yes" → Proceed to execution phase
- "approve H-XXX" → Approve and move to ready status

To request revisions:
- "revise" or "needs changes" → Send back to Theorist
- "fix issue 1 and 2" → Specify which issues to address

To reject:
- "reject" or "archive" → Archive hypothesis with reason
- "salvage H-XXX" → Extract useful elements

CRITICAL: No code execution, no state changes until explicit approval received.
```

**Impact**: Clear separation between system recommendations and user decisions. No automatic execution without approval.

---

### 3. **Archive & Decision Tracking System**

#### Created `HYPOTHESIS_ARCHIVE.md`

Purpose: Learn from failures, avoid repetition, extract value from rejected work

**Template includes**:
- Rejection reason and category
- Evidence against hypothesis
- Domain-specific issues
- Learnings extracted
- Salvageable elements
- Archive statistics

**Example entry structure**:
```markdown
### H-XXX: [Hypothesis Title]
**Archived**: YYYY-MM-DD
**Domain**: stats-theory
**Rejection Reason**: No viable lower bound strategy - computationally intractable

#### Evidence Against
- Theoretical: Hardness reduction from known NP-hard problem
- Proof impossibility shown by counterexample

#### Learnings Extracted
1. Problem class requires polynomial-time algorithm for lower bound
2. Avoid this parameter regime in future work
3. Consider relaxed version with additional structure

#### Salvageable Elements
- Method applicable to restricted problem class
- Simulation code reusable for other hypotheses
```

#### Created `decisions/` Directory with MDR System

**Methodological Decision Records (MDRs)** for audit trail:

Files created:
- `decisions/README.md` - Explains MDR system and when to use
- `decisions/MDR-000-TEMPLATE.md` - Standard template for all MDRs
- `decisions/MDR-001-EXAMPLE-fano-lower-bound.md` - Concrete example

**MDR structure**:
```markdown
# MDR-NNN: [Decision Title]

## Context
[Why this decision needed]

## Decision
[What was decided]

## Rationale
[Why this choice]

## Alternatives Considered
[Other options with pros/cons]

## Consequences
[Positive/negative outcomes accepted]

## Domain-Specific Considerations
[How this aligns with domain standards]

## Implementation Notes
[Practical execution details]

## Success Criteria
[How to verify decision was correct]

## References
[Related literature and MDRs]
```

**Example decisions to track with MDRs**:
- Proof technique selection (Fano vs Assouad vs Le Cam)
- Identification strategy (RDD vs IV vs DID)
- Assumption choices (sub-Gaussian vs bounded moments)
- Estimator selection (ℓ₁ vs ℓ₀ penalization)

**Impact**: Preserves institutional memory, enables learning from experience, provides audit trail for research decisions.

---

## 📊 File Structure After Improvements

```
/Users/andyhou/research/
├── agents/
│   ├── theorist.md              ✅ Added PROACTIVELY triggers
│   ├── experimentalist.md       ✅ Added PROACTIVELY triggers
│   ├── methodologist.md         ✅ Added PROACTIVELY triggers
│   ├── verifier.md              ✅ Added PROACTIVELY triggers
│   └── coordinator.md           ✅ Added PROACTIVELY triggers
│
├── commands/
│   ├── brainstorm.md            ✅ Added approval gate
│   ├── review-hypothesis.md     ✅ Added approval gate
│   └── ... (other commands)
│
├── decisions/                   ✨ NEW DIRECTORY
│   ├── README.md                ✨ MDR system documentation
│   ├── MDR-000-TEMPLATE.md      ✨ Template for all MDRs
│   └── MDR-001-EXAMPLE-fano-lower-bound.md  ✨ Concrete example
│
├── HYPOTHESIS_ARCHIVE.md        ✨ NEW FILE - Track rejected hypotheses
└── IMPROVEMENTS_SUMMARY.md      ✨ NEW FILE - This document
```

---

## 🎯 Key Benefits

### From PROACTIVELY Triggers:
- ✅ Agents actively help instead of waiting to be asked
- ✅ Clear guidance on when each agent should be involved
- ✅ Reduces user cognitive load ("Do I need X agent now?")
- ✅ Mirrors successful pattern from everything-claude-code

### From Approval Gates:
- ✅ User maintains control over research direction
- ✅ No surprise automatic execution
- ✅ Clear decision points in workflow
- ✅ Explicit consent before major transitions

### From Archive & MDR System:
- ✅ Learn from failures (hypothesis archive)
- ✅ Avoid repeating mistakes
- ✅ Audit trail for methodological decisions
- ✅ Institutional memory preservation
- ✅ Justify decisions to reviewers

---

## 🔄 Workflow Changes

### Before:
```
Brainstorm → [System proceeds automatically] → Review → Execute
```
**Problem**: User loses control, system makes assumptions

### After:
```
Brainstorm → ⏸️ Wait for approval → Review → ⏸️ Wait for approval → Execute
```
**Benefit**: User explicitly approves each transition

### With PROACTIVELY:
Agents actively suggest when they should be involved:
- "Based on this hypothesis draft, I (Methodologist) proactively checked domain standards..."
- "As Experimentalist, I proactively assessed feasibility before formal review..."

---

## 📝 Usage Examples

### Example 1: Hypothesis Generation with Approval Gate

```bash
User: /brainstorm "Minimax rates for sparse estimation"

System:
[Generates 3 hypotheses]

⏸️ WAITING FOR USER DIRECTION
- "Review H-001" → Launch review
- "More ideas" → Generate more
- "Expand on H-002" → Deep dive

User: "Review H-001"

[Review process starts]
```

### Example 2: Proactive Methodologist Check

```bash
User: [Working on hypothesis H-003]

Methodologist (PROACTIVELY):
"I proactively checked H-003 against stats-theory domain standards:
- ⚠️ No lower bound strategy specified (critical for Annals)
- ✅ Assumptions clearly stated
- ⚠️ Proof technique not identified

Suggest addressing lower bound before formal review."
```

### Example 3: Creating MDR for Decision

```bash
User: "We need to decide: use Fano or Assouad for lower bound"

Methodologist: "Let me help create an MDR for this decision..."

[Creates decisions/MDR-002-lower-bound-technique.md documenting:
- Context: Why choosing now
- Alternatives: Fano vs Assouad vs Le Cam
- Decision: Fano selected
- Rationale: Global problem, literature precedent
- Consequences: More complex but tighter bound]

User: "Approved"

[MDR status: Accepted, can reference in hypothesis: "Per MDR-002..."]
```

### Example 4: Archiving Rejected Hypothesis

```bash
User: "H-005 doesn't work - the lower bound proof is impossible"

System: "Archiving H-005 to HYPOTHESIS_ARCHIVE.md..."

[Records:
- Rejection reason: Proof impossibility
- Evidence: Computational hardness reduction
- Learnings: Avoid this problem class
- Salvageable: Method works for restricted version]

User can later reference: "We previously tried this in H-005 (archived),
learned that [X], so now trying [Y] instead"
```

---

## 🔮 Future Enhancements (Not Yet Implemented)

Based on everything-claude-code patterns that could be added later:

1. **TDD-style Research Pattern**: RED (hypothesis) → GREEN (evidence) → REFACTOR (theory)
2. **Common Pitfalls Checklists**: Separate files like `COMMON_PITFALLS_STATS.md`
3. **Pattern Extraction**: `/learn-from-hypothesis` command to extract reusable patterns
4. **Context Modes**: `research.md`, `writing.md`, `review.md` context switching

---

## 📚 Documentation Added

1. **HYPOTHESIS_ARCHIVE.md** - Template and guidelines for archiving
2. **decisions/README.md** - MDR system documentation
3. **decisions/MDR-000-TEMPLATE.md** - Standard format
4. **decisions/MDR-001-EXAMPLE-fano-lower-bound.md** - Concrete example
5. **IMPROVEMENTS_SUMMARY.md** - This document

---

## ✨ Summary

**What changed**:
- 5 agents updated with PROACTIVELY triggers
- 2 commands updated with explicit approval gates
- 1 archive system created (HYPOTHESIS_ARCHIVE.md)
- 1 decision tracking system created (decisions/ directory with MDRs)
- 5 new documentation files

**Impact**:
- More proactive agents
- Better user control
- Learning from failures
- Audit trail for decisions
- Institutional memory

**Inspired by**: everything-claude-code-main framework patterns

**Status**: ✅ Complete and ready to use

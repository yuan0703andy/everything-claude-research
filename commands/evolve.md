---
name: evolve
description: Automatically refine hypothesis based on critique/feedback - generates improved version addressing all identified issues
argument-hint: <hypothesis-id>
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

# /evolve Command

Automatic hypothesis evolution based on critique and feedback. Generates improved version (H-XXX-v2) that addresses all identified issues while preserving core insights.

## Purpose

This command implements the **Evolution** cycle from Google's AI Co-Scientist paper:
1. **Analyze** all feedback (from `/stress-test`, `/review-hypothesis`, or Methodologist)
2. **Generate** improved hypothesis version that addresses each critique point
3. **Preserve** core novel contribution (don't dilute the insight)
4. **Document** what changed and why (audit trail)

Returns an evolved hypothesis ready for re-verification.

## Usage

```bash
# Basic: Evolve based on most recent stress-test
/evolve H-001

# Evolve based on specific review
/evolve H-001 --review meeting_notes/lab_meeting_2024-01-27.md

# Conservative evolution (minimal changes)
/evolve H-001 --conservative

# Aggressive evolution (major restructuring allowed)
/evolve H-001 --aggressive
```

## Workflow

```
User: /evolve H-001
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Gather Critique                │
    │  - Read original hypothesis H-001       │
    │  - Load stress-test results             │
    │  - Load review comments (if any)        │
    │  - Load domain standards                │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Automatic Evolution            │
    │  (Single theorist-enhanced agent call)  │
    │                                         │
    │  Internal process:                      │
    │  2a. Analyze each critique point        │
    │  2b. Determine evolution strategy       │
    │  2c. Generate improved version          │
    │  2d. Preserve core novelty              │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Document Changes               │
    │  - Create change log                    │
    │  - Identify what was preserved          │
    │  - Map critiques to fixes               │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 4: Save & Suggest Next            │
    │  - Save H-001-v2 (new file)             │
    │  - Keep H-001 (original preserved)      │
    │  - Update HYPOTHESES.md                 │
    │  - Suggest `/stress-test H-001-v2`      │
    └─────────────────────────────────────────┘
```

## What Happens Automatically

### Single Coordinated Evolution

The command spawns `theorist-enhanced` agent **once** with complete context:
- Original hypothesis H-001
- All critique points from stress-test/reviews
- Domain standards
- Project constraints

The agent internally:
1. Assesses which critiques are critical vs minor
2. Determines evolution strategy (feasibility vs rigor vs novelty improvement)
3. Generates coherent improved version
4. Maintains thought continuity (doesn't forget why changes were made)

### Evolution Strategies (Auto-Selected)

Based on critique type, agent chooses approach:

**Feasibility Improvement** (if Experimentalist flagged infeasibility):
- Simplify experimental design
- Identify alternative methodologies
- Break into phased approach

**Rigor Improvement** (if Methodologist flagged gaps):
- Add missing proof steps
- Specify identification strategy (policy research)
- Strengthen causal reasoning

**Novelty Improvement** (if Verifier flagged lack of originality):
- Differentiate from existing work more clearly
- Add mechanistic detail
- Identify unique angle

## Output

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 EVOLVE ► H-001 → H-001-v2
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Evolution Summary

**Original**: H-001 (Minimax rates for sparse regression)
**Evolved**: H-001-v2
**Trigger**: Stress-test identified 3 gaps
**Strategy**: Rigor improvement (address assumption issues)

─────────────────────────────────────────────────────

## Critiques Addressed

### 🔴 Critique 1: Incoherence assumption too strong (CRITICAL)
**Original Problem**:
- Assumed design matrix X satisfies μ(X) < 0.1 (strong incoherence)
- Unrealistic in many applications
- Domain standard: Annals reviewers will reject

**Evolution**:
- **Relaxed to**: μ(X) < C for some constant C
- **Added**: Explicit characterization of when this holds
- **Justification**: Shown to hold for Gaussian designs, sub-Gaussian with conditions

**Status**: ✅ Resolved

### 🟡 Critique 2: Implicit signal strength assumption (IMPORTANT)
**Original Problem**:
- Proof assumed β_min > c log(p)/n but never stated
- This is testable condition, should be explicit

**Evolution**:
- **Added**: Explicit assumption A4: β_min > (2+ε)σ√(log p / n)
- **Justification**: Standard in variable selection literature
- **Testability**: Can verify in any given application

**Status**: ✅ Resolved

### 🟡 Critique 3: Heavy-tail robustness untested (IMPORTANT)
**Original Problem**:
- Claimed robustness to heavy tails but no evidence

**Evolution**:
- **Revised claim**: Removed robustness claim
- **Added**: Explicit assumption A1: Errors are sub-Gaussian
- **Future work**: Note that heavy-tail extension is open problem

**Status**: ✅ Resolved (by scope reduction)

─────────────────────────────────────────────────────

## What Changed (Detailed)

### 1. Theorem Statement
**Before**:
> Theorem 1: Under sparsity s=O(n^(1/5)), the adaptive threshold estimator achieves rate n^(-2/5).

**After**:
> Theorem 1: Suppose:
> - (A1) Errors are sub-Gaussian with parameter σ
> - (A2) Sparsity s = O(n^(1/5))
> - (A3) Design satisfies μ(X) ≤ C_μ (incoherence)
> - (A4) Min signal strength β_min ≥ (2+ε)σ√(log p / n)
>
> Then the adaptive threshold estimator achieves E||β̂ - β||² ≤ Cs²(log p / n) with probability 1-O(1/p).

**Why Better**: All assumptions explicit, testable, and justified

### 2. Proof Technique
**Added**: Section 3.1 on when incoherence (A3) holds
- Gaussian design: μ(X) = O(√(log p / n)) w.h.p.
- Sub-Gaussian: Similar but with constants depending on ψ₂ norm

**Why Better**: Addresses domain standard requirement for assumption justification

### 3. Scope
**Removed**: Claim about heavy-tail robustness
**Added**: Subsection 5.3 "Extensions and Future Work" noting this as open

**Why Better**: Honest about limitations, avoids unfounded claims

─────────────────────────────────────────────────────

## What Was Preserved (Core Insights)

✓ **Novel contribution remains intact**:
  - Adaptive thresholding technique (unchanged)
  - Connection to wavelet theory (unchanged)
  - Rate optimality claim (unchanged, now with rigorous assumptions)

✓ **Proof structure preserved**:
  - Still uses Fano's method for lower bound
  - Oracle inequality approach unchanged

✓ **Main result strength**:
  - Still minimax optimal under stated assumptions
  - Actually stronger now (assumptions are realistic)

**Philosophy**: Evolution refined the hypothesis, didn't dilute it.

─────────────────────────────────────────────────────

## Evolution Metrics

| Aspect | H-001 (Original) | H-001-v2 (Evolved) | Change |
|--------|------------------|-------------------|--------|
| Novelty | 8.5/10 | 8.5/10 | No change ✓ |
| Rigor | 6.0/10 | 9.0/10 | +50% ↑ |
| Testability | 7.0/10 | 8.5/10 | +21% ↑ |
| Domain Compliance | 6.5/10 | 9.5/10 | +46% ↑ |
| **Overall** | **7.0/10** | **8.9/10** | **+27%** ↑ |

Predicted `/stress-test` result: **PASS** (≥9.0 likely)

─────────────────────────────────────────────────────

## Files Created/Updated

- **New**: hypotheses/proposals/H-001-v2-[slug].md
- **Preserved**: hypotheses/proposals/H-001-[slug].md (original unchanged)
- **New**: hypotheses/evolution/H-001-evolution-log.md (change audit trail)
- **Updated**: hypotheses/HYPOTHESES.md (added H-001-v2 entry)
- **Updated**: STATE.md (current focus: H-001-v2)

─────────────────────────────────────────────────────

## Recommended Next Steps

1. **Re-verify** (Recommended):
   ```bash
   /stress-test H-001-v2
   ```
   Expected: PASS verdict (all gaps addressed)

2. **If PASS, proceed to execution**:
   ```bash
   /execute-analysis H-001-v2
   ```

3. **If still gaps, iterate**:
   ```bash
   /evolve H-001-v2
   ```
   (Typically 1-2 iterations sufficient)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Next: `/stress-test H-001-v2` to verify improvements
```

## Evolution Modes (Auto-Selected)

| Critique Type | Evolution Mode | What Changes | What Preserves |
|---------------|---------------|--------------|----------------|
| **Feasibility** | Simplification | Methods, design | Core claim, novelty |
| **Rigor** | Strengthening | Assumptions, proof | Mechanism, results |
| **Novelty** | Differentiation | Positioning, framing | Technical content |
| **Scope** | Refinement | Boundary conditions | Central contribution |

Agent automatically selects based on critique content.

## Options

| Flag | Effect | When to Use |
|------|--------|-------------|
| `--review=<file>` | Use specific review as basis | Have detailed review notes |
| `--conservative` | Minimal changes only | Preserve more of original |
| `--aggressive` | Allow major restructuring | Original has fundamental issues |
| `--focus=<aspect>` | Target specific improvement | Know exactly what to fix |

## Integration with Other Commands

```
/hypothesize [goal]           ← Generate candidates
        ↓
/stress-test H-001            ← Identify gaps
        ↓
/evolve H-001                 ← Fix issues (YOU ARE HERE)
        ↓
/stress-test H-001-v2         ← Re-verify
        ↓
[If PASS]
/execute-analysis H-001-v2    ← Run experiments
```

## Examples

### Example 1: Statistical Theory - Add Lower Bound

```bash
# Stress-test found: "No lower bound - cannot claim minimax optimality"
/evolve H-001

# Agent automatically:
# - Identifies that lower bound is missing (critical for stats theory)
# - Chooses Fano's method (appropriate for this problem)
# - Generates Section 3.2 with lower bound derivation
# - Updates theorem to claim "minimax optimal"
#
# H-001-v2 now has complete proof (upper + lower bound)
```

### Example 2: Policy Research - Specify Identification

```bash
# Stress-test found: "Identification strategy unclear"
/evolve H-005

# Agent automatically:
# - Recognizes this is policy research (from DOMAIN.md)
# - Proposes RDD identification at policy threshold
# - Adds identifying assumptions (continuity, no manipulation)
# - Specifies robustness checks
#
# H-005-v2 now has clear causal identification strategy
```

### Example 3: Multiple Iterations

```bash
# First evolution
/evolve H-003
# → H-003-v2 (fixed assumptions)

/stress-test H-003-v2
# → Still has gap: "Proof of Lemma 2 incomplete"

# Second evolution
/evolve H-003-v2
# → H-003-v3 (completed proof)

/stress-test H-003-v3
# → PASS ✅
```

## Why This Design?

### ❌ Manual Fix Approach:
```
1. Read stress-test report
2. Think about how to fix each issue
3. Edit hypothesis file manually
4. Hope you didn't introduce new issues
5. Hope you preserved core contribution
```

**Problems**:
- Easy to over-correct or under-correct
- Risk of introducing new gaps
- May accidentally dilute novelty
- No systematic change tracking

### ✅ Automated Evolution:
```bash
/evolve H-001
```

**Benefits**:
- Agent has context of **why** each critique matters
- Systematic addressing of all issues
- Automatic preservation of core insights
- Built-in change log for audit
- Can re-evolve if needed (iterable)

## Behind the Scenes

**What You Don't See**:

1. Command spawns `theorist-enhanced` agent via Task tool
2. Agent receives:
   - Original hypothesis (full text)
   - Critique summary (from stress-test or review)
   - Domain standards (knows what matters)
   - Project context (knows constraints)
3. Agent internally (evolution mode):
   - Analyzes each critique's severity and type
   - Chooses evolution strategy
   - Generates improved version maintaining thought continuity
   - Creates audit trail
4. System parses output, creates v2 file, updates indices

**Why Hidden?**
- You want **result** ("improved hypothesis")
- Not **process** ("which evolution strategy?")
- Agent can make subtle judgment calls (e.g., when to scope-reduce vs expand)

## Important Notes

⚠️ **Original is preserved**
- H-001 file remains unchanged
- H-001-v2 is new file
- Can always revert or compare
- Full evolution history maintained

⚠️ **Not a magic fix**
- If original hypothesis is fundamentally flawed, evolution can't save it
- Sometimes verdict is "hypothesis not salvageable"
- In that case: run `/hypothesize` again with refined goals

⚠️ **Expect iteration**
- First evolution usually addresses most issues
- May need 2-3 iterations to reach PASS
- This is normal! Research is iterative

⚠️ **Core preservation is heuristic**
- Agent tries to preserve novelty
- But if novelty itself was the problem, it may change
- Review evolved version critically

## When Evolution Fails

If `/evolve` reports **"Hypothesis not salvageable"**:

**Possible Reasons**:
1. Core claim contradicts evidence
2. Novelty claim is false (prior work exists)
3. Assumptions required are unrealistic
4. Mechanism is logically inconsistent

**What to Do**:
1. Read evolution report for explanation
2. Decide: Major revision vs fresh start
3. If fresh start: `/hypothesize` with lessons learned
4. If revision: Manual edit, then `/stress-test` again

## Comparison: /evolve vs Manual Editing

| Feature | /evolve | Manual Edit |
|---------|---------|-------------|
| Speed | Fast (automated) | Slow (requires thought) |
| Completeness | Addresses all critiques | May miss some |
| Consistency | Maintains coherent narrative | Risk of patchwork |
| Audit trail | Automatic change log | Must document manually |
| Context | Agent remembers full critique | You must remember |
| Risk | May over/under-correct | You control exactly |

**Recommendation**: Use `/evolve` for first pass, manual edit for fine-tuning if needed.

## Troubleshooting

**"Evolution didn't address my main concern"**
- Use --focus flag to target specific issue
- Or provide --review with your own critique
- Or manually edit, then /stress-test again

**"Evolved version is worse"**
- Check evolution log to understand reasoning
- Original H-001 is preserved, can discard v2
- May indicate original hypothesis has fundamental issues

**"Too many iterations needed"**
- After 3 iterations, consider whether hypothesis is viable
- May be sign to start fresh with `/hypothesize`
- Consult with Methodologist for domain-specific guidance

**"Evolution changed my core claim"**
- Review evolution report's "What Was Preserved" section
- If truly changed, may indicate claim was problematic
- Can manually revert specific changes and re-test

## Domain-Specific Evolution

Evolution automatically adapts to domain:

**Stats Theory**:
- Emphasis on adding missing proof steps, stating assumptions
- Knows common requirements (lower bounds, rate optimality)

**Policy Research**:
- Emphasis on identification strategies, confounders
- Knows causal inference requirements

**Biomedical**:
- Emphasis on mechanism plausibility, clinical relevance
- Knows pathway validation standards

This happens automatically based on DOMAIN.md and critique type.

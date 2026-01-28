---
name: stress-test
description: Comprehensive hypothesis verification - automatically checks novelty, observations, and assumptions in one unified workflow
argument-hint: <hypothesis-id>
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

# /stress-test Command

Comprehensive hypothesis verification using AI Co-Scientist methodology. Automatically performs multi-dimensional validation in a single coordinated workflow.

## Purpose

This command implements the **Verifier** role from Google's AI Co-Scientist paper, performing three critical checks automatically:
1. **Novelty Verification**: Is this hypothesis truly novel vs existing literature?
2. **Observation Matching**: Does it explain/predict observed phenomena?
3. **Assumption Decomposition**: Are all assumptions explicit and valid?

Returns a **unified scientific validation score** with identified gaps.

## Usage

```bash
# Basic: Full verification
/stress-test H-001

# With specific observation data
/stress-test H-001 --observations data/experimental_results.csv

# Quick check (novelty + assumptions only, skip observations)
/stress-test H-001 --quick
```

## Workflow

```
User: /stress-test H-001
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Context Loading                │
    │  - Read hypothesis H-001                │
    │  - Load DOMAIN.md (standards)           │
    │  - Load observations (if available)     │
    │  - Load recent literature               │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Automatic Multi-Check          │
    │  (Single verifier-enhanced agent call)  │
    │                                         │
    │  Internal sequence:                     │
    │  2a. Novelty check (literature scan)    │
    │  2b. Observation matching (if data)     │
    │  2c. Assumption decomposition           │
    │  2d. Cross-check consistency            │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Synthesize Results             │
    │  - Compute overall confidence score     │
    │  - Identify critical gaps               │
    │  - Generate actionable recommendations  │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 4: Verdict & Next Steps           │
    │  PASS / GAPS / BLOCKED                  │
    │  - Update hypothesis status             │
    │  - Create remediation tasks (if gaps)   │
    └─────────────────────────────────────────┘
```

## What Happens Automatically

### Single Coordinated Verification

Unlike fragmented mode-based calls, `/stress-test` runs **one unified verification** where the verifier agent:

1. **Maintains context** across all three checks (no broken thought chain)
2. **Identifies interactions** between novelty, observations, and assumptions
3. **Provides holistic assessment** rather than three disconnected reports

### Behind the Scenes

The command spawns `verifier-enhanced` agent **once** with complete context:
- Full hypothesis text
- Domain standards from DOMAIN.md
- Available observations/data
- Recent literature (via web search)

The agent internally performs all three verification types and synthesizes results into a unified report.

## Output

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 STRESS TEST ► H-001: [Hypothesis Title]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Overall Verdict: GAPS IDENTIFIED ⚠️

**Confidence Score**: 7.2/10
- Novelty: 8.5/10 ✓ (Strong novel contribution)
- Observation Match: 6.0/10 ⚠️ (Partial support)
- Assumption Validity: 7.0/10 ⚠️ (Some assumptions weak)

─────────────────────────────────────────────────────

## 1. Novelty Verification

### Novel Aspects ✓
- **Mechanism**: Uses adaptive thresholding in new regime (no prior work)
- **Rate**: Achieves n^(-2/5) where prior art has n^(-1/3)
- **Proof Technique**: Novel application of Fano's method to this setting

### Known Aspects
- **Framework**: Builds on established minimax theory (Donoho & Johnstone)
- **Setting**: High-dimensional sparse regression is well-studied

### Literature Comparison
| Our Hypothesis | Closest Prior Work | Key Difference |
|----------------|-------------------|----------------|
| Sparse regime s=O(n^(1/5)) | Jones 2023: s=O(n^(1/4)) | Our regime is sparser |
| Adaptive estimator | Smith 2022: Fixed threshold | Ours adapts to local structure |

**Novelty Score**: 8.5/10 - Substantial novel contribution

─────────────────────────────────────────────────────

## 2. Observation-Hypothesis Matching

### Predicted Phenomena
| Prediction | Observation Status | Match Score |
|------------|-------------------|-------------|
| P1: Rate scales as n^(-2/5) | ✓ Observed in simulation | 9/10 |
| P2: Adaptive > Fixed threshold | ✓ Confirmed empirically | 8/10 |
| P3: Breaks down at s>n^(1/5) | ⚠️ Partial data | 5/10 |
| P4: Robust to heavy tails | ❌ Not yet tested | 0/10 |

### Missing Pieces ⚠️
- Prediction P4 (robustness to heavy tails) not yet empirically validated
- Need additional experiments for P3 boundary behavior

**Observation Match Score**: 6.0/10 - Good match on core predictions, gaps on edge cases

─────────────────────────────────────────────────────

## 3. Assumption Decomposition

### Explicit Assumptions
| ID | Assumption | Validity | Evidence Quality |
|----|------------|----------|------------------|
| A1 | Errors are sub-Gaussian | ✓ Reasonable | Moderate (standard) |
| A2 | Sparsity s=O(n^(1/5)) | ✓ Testable | Strong (can verify) |
| A3 | Design matrix incoherence | ⚠️ Strong | Weak (needs justification) |

### Implicit Assumptions (Discovered) 🔍
| ID | Hidden Assumption | Risk Level | Action Needed |
|----|------------------|------------|---------------|
| A4 | Signal strength β_min > c log(p)/n | ⚠️ Medium | State explicitly |
| A5 | Eigenvalue bounds on Σ | ⚠️ Medium | Verify in proof |

### Red Flags 🚩
- **A3 (Incoherence)**: This is a strong assumption that may not hold in practice
  - Recommendation: Either relax assumption or provide conditions when it holds
  - Domain Standard (stats-theory): Annals reviewers will question this

**Assumption Validity Score**: 7.0/10 - Mostly valid but some need strengthening

─────────────────────────────────────────────────────

## Critical Gaps Identified

### Gap 1: Incomplete Empirical Validation 🔴 CRITICAL
**Impact**: Cannot claim hypothesis is validated without P4 evidence
**Fix**: Run simulations under heavy-tailed error distributions
**Effort**: 1-2 days
**Blocks**: Publication submission

### Gap 2: Incoherence Assumption Unjustified 🟡 IMPORTANT
**Impact**: Reviewers will request major revision
**Fix**: Either provide theoretical justification or empirical conditions
**Effort**: 1 week
**Blocks**: Annals submission (domain standard)

### Gap 3: Implicit Assumptions Not Stated 🟡 IMPORTANT
**Impact**: Proof may have gaps
**Fix**: Make A4 and A5 explicit in theorem statement
**Effort**: 2-3 days
**Blocks**: Rigorous verification

─────────────────────────────────────────────────────

## Synthesis & Recommendation

### Why This Hypothesis Has Potential ✓
- Strong novelty (8.5/10) - Advances the field
- Core mechanism well-supported by observations
- Most assumptions are reasonable

### What Needs Work ⚠️
1. Complete empirical validation (P4 testing)
2. Justify or relax incoherence assumption
3. Make all assumptions explicit

### Verdict: GAPS IDENTIFIED (Not BLOCKED)
This hypothesis is **promising but not ready** for execution/publication.

**Recommended Next Step**: `/evolve H-001` to address gaps
- Evolution agent will automatically propose fixes for each gap
- After evolution: Re-run `/stress-test H-001-v2`

**Alternative**: Address gaps manually, then re-verify

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Files Updated
- hypotheses/proposals/H-001-[slug].md (added stress-test results)
- hypotheses/verification/H-001-stress-test-[date].md
- STATE.md (updated hypothesis status)

Next: `/evolve H-001` to fix identified gaps
```

## Verdicts Explained

| Verdict | Meaning | Confidence | Next Step |
|---------|---------|------------|-----------|
| **PASS** ✅ | All checks passed, ready for execution | ≥9.0/10 | `/execute-analysis` |
| **GAPS** ⚠️ | Issues identified but fixable | 6.0-8.9/10 | `/evolve` to fix |
| **BLOCKED** 🚧 | Critical flaws, hypothesis needs major revision | <6.0/10 AND no contradictions | Reconsider or `/hypothesize` anew |
| **DISPROVED** ❌ | Hypothesis contradicted by observations/evidence | ANY score if critical contradictions detected | **STOP** - Abandon hypothesis |

### Verdict Priority Logic

**DISPROVED has highest priority** - it overrides all other verdicts:

```python
def determine_verdict(confidence_score, contradictions):
    # Step 1: Check for falsification (highest priority)
    if critical_contradictions_detected(contradictions):
        return "DISPROVED"  # Falsified by evidence

    # Step 2: Normal scoring system (if not disproved)
    if confidence_score >= 9.0:
        return "PASS"
    elif confidence_score >= 6.0:
        return "GAPS"
    else:
        return "BLOCKED"
```

**Key insight**: A hypothesis can have high novelty (9/10) and clear mechanism (8/10), but if observations **directly contradict** core predictions, it must be marked **DISPROVED**. Scientific honesty requires abandoning falsified hypotheses, not "evolving" them.

## Options

| Flag | Effect | When to Use |
|------|--------|-------------|
| `--quick` | Skip observation matching (novelty + assumptions only) | Early stage, no data yet |
| `--observations=<file>` | Specify data file for observation matching | Have experimental results |
| `--literature-cutoff=<year>` | Only check literature after year | Avoid known prior work |
| `--strict` | Apply stricter domain standards | Targeting top-tier journals |

## Integration with Other Commands

```
/hypothesize [goal]          ← Generate candidates
        ↓
/stress-test H-001           ← Verify quality (YOU ARE HERE)
        ↓
[If PASS]
/execute-analysis H-001      ← Run experiments
        ↓
[If GAPS]
/evolve H-001                ← Fix issues
        ↓
/stress-test H-001-v2        ← Re-verify
```

## Examples

### Example 1: Statistical Theory

```bash
/stress-test H-001

# Checks:
# - Novelty: vs Donoho, Wainwright, etc.
# - Observations: simulation results
# - Assumptions: sub-Gaussian, sparsity, incoherence
#
# Finds: Incoherence assumption too strong (Gap)
# Recommends: /evolve to relax assumption
```

### Example 2: Policy Research

```bash
/stress-test H-005 --observations data/state_policies.csv

# Checks:
# - Novelty: vs Kingdon, Sabatier, policy literature
# - Observations: Does it explain actual policy adoption patterns?
# - Assumptions: Rational actors, information flow, etc.
#
# Finds: Explains 80% of variation (Good)
# But: Assumes perfect information (Unrealistic)
# Recommends: /evolve to add information constraints
```

### Example 3: Quick Pre-Check

```bash
/stress-test H-003 --quick

# Early stage check before running experiments
# Skips observation matching (no data yet)
# Focuses on novelty and assumption validity
# Fast: 2-3 minutes vs 10-15 minutes for full
```

### Example 4: DISPROVED Verdict (Critical Contradiction)

```bash
/stress-test H-007 --observations data/knockout_mice.csv

# Hypothesis claims: "CXCR1/2 inhibition reduces AML progression"
#
# Observation found: CXCR1/2 knockout mice show INCREASED AML burden
#   - Smith et al. 2023, Nature
#   - KO mice survival: 15 days vs WT: 25 days (p<0.001)
#
# Contradiction:
#   Hypothesis predicts: Inhibition → Less progression
#   Observation shows:   Inhibition → MORE progression
#
# Result: DISPROVED ❌
# Recommendation: STOP - Do not proceed with this hypothesis
```

**Output**:
```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 STRESS TEST ► H-007: CXCR1/2 Inhibition for AML
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Overall Verdict: DISPROVED ❌

**Critical Issue**: Hypothesis contradicted by experimental evidence

**Confidence Score**: 4.5/10
- Novelty: 7.5/10 ✓ (Novel mechanism)
- Observation Match: 2.0/10 ❌ (Direct contradiction)
- Assumption Validity: 6.5/10 ⚠️ (Mostly valid)

─────────────────────────────────────────────────────

## 🚨 CONTRADICTION DETECTED

### Observation-Hypothesis Mismatch

**Observation**: CXCR1/2 knockout mice show INCREASED AML burden
- Source: [Smith et al. 2023, Nature]
- Data: KO mice survival: 15 days vs WT: 25 days (p<0.001)
- Quality: Tier 1 evidence (published RCT)

**Hypothesis Claim**: CXCR1/2 inhibition reduces AML progression

**Contradiction**:
Hypothesis predicts: Inhibition → Less progression
Observation shows: Inhibition (KO) → MORE progression

**Verdict**: 🚫 Disproved - Core prediction contradicted by high-quality evidence

─────────────────────────────────────────────────────

## Recommendation

❌ **DO NOT PROCEED** with this hypothesis

This hypothesis has been **falsified by experimental evidence**.

**Options**:
1. **Abandon**: Shelve this hypothesis as disproven
2. **Reframe**: Generate entirely new mechanism explaining opposite effect
3. **Conditional**: Restrict scope to contexts where KO data doesn't apply

**NOT recommended**: Using `/evolve` to "patch" the contradiction
- Evolution fixes assumptions or scope issues
- Evolution cannot fix core falsification by evidence
- Scientific honesty requires abandoning disproved hypotheses

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Next: `/hypothesize` with refined understanding (avoid this mechanism)
```

**Important**: DISPROVED is different from BLOCKED:
- **BLOCKED**: Hypothesis has logical flaws or weak support (fixable via evolution)
- **DISPROVED**: Hypothesis contradicted by empirical evidence (must be abandoned)

When a hypothesis is DISPROVED, the system automatically:
1. Marks status as "DISPROVED" in STATE.md
2. Removes from active consideration
3. Preserves file for reference (learn from failures)
4. Suggests alternative research directions

## Why This Design?

### ❌ Old (Fragmented) Approach:
```bash
# User manually runs three separate checks
task verifier-enhanced --mode=novelty "Check H-001"
# (AI loses context)
task verifier-enhanced --mode=observation "Match H-001"
# (AI doesn't remember novelty check)
task verifier-enhanced --mode=assumption "Decompose H-001"
# (AI can't synthesize across all three)
```

**Problems**:
- Broken thought chain
- No cross-check between dimensions
- User must manually synthesize
- High cognitive load

### ✅ New (Unified) Approach:
```bash
# One command, complete workflow
/stress-test H-001
```

**Benefits**:
- Continuous context throughout verification
- Agent identifies interactions (e.g., "novelty claim requires assumption A3")
- Holistic assessment with unified score
- User gets actionable verdict

## Behind the Scenes

**What You Don't See**:

1. Command spawns `verifier-enhanced` agent via Task tool
2. Agent receives complete context: hypothesis + domain + observations
3. Agent internally performs (no mode parameter exposed):
   - Literature search and novelty assessment
   - Observation matching against predictions
   - Assumption extraction and validation
   - Cross-consistency checking
4. Agent synthesizes unified report
5. System parses results, updates files, determines verdict

**Why Hidden?**
- You care about **result** ("Is my hypothesis valid?")
- Not **implementation** ("Which mode to run next?")
- Enables agent to adapt verification strategy dynamically

## Important Notes

⚠️ **Garbage in, garbage out**
- Hypothesis must be well-formulated
- DOMAIN.md should have relevant literature
- If hypothesis is vague, verification will be vague

⚠️ **Novelty ≠ Correctness**
- Novel hypothesis can still be wrong
- This checks logical consistency and empirical support
- Ultimate validation requires actual experiments

⚠️ **Treat gaps seriously**
- Domain-flagged gaps (e.g., "incoherence assumption") are publication blockers
- Don't ignore "IMPORTANT" level gaps
- Use `/evolve` rather than manual fixes (preserves thought chain)

⚠️ **Iterate expectation**
- First stress-test usually finds gaps (normal!)
- Use `/evolve` → `/stress-test` cycle
- Typically 2-3 iterations to reach PASS

🛑 **DISPROVED means STOP**
- If verdict is DISPROVED, do NOT use `/evolve` to fix
- Falsified hypotheses must be abandoned (scientific honesty)
- Learn from failure: What assumption was wrong?
- Generate new hypothesis with corrected understanding
- **Evolution cannot repair falsification by evidence**

## Troubleshooting

**"Novelty score seems wrong"**
- Check that DOMAIN.md has up-to-date literature
- Provide --literature-cutoff if field moves fast
- Recent papers (last 6 months) may not be in agent's training

**"Observation matching returns N/A"**
- Means no observations available yet
- Use --quick flag to skip this check
- Or provide --observations=<file> with your data

**"Too many assumptions flagged"**
- This is often good! Hidden assumptions are risky
- Make them explicit in hypothesis
- Use `/evolve` to address systematically

**"Verdict seems harsh"**
- Agent applies domain standards strictly
- This is feature, not bug (better to find issues now)
- Remember: Publication reviewers will be even harsher

## Comparison: /stress-test vs /verify-results

| Feature | /stress-test | /verify-results |
|---------|--------------|-----------------|
| When | Before execution (hypothesis validation) | After execution (results validation) |
| Checks | Novelty + Assumptions + Predictions | Goals achieved + Domain compliance |
| Data | Optional (can check without experiments) | Requires experimental results |
| Output | Hypothesis quality score | Goal achievement verdict |
| Use | Early quality gate | Final publication gate |

Both are verification commands but at different stages!

## Domain-Specific Behavior

The stress-test automatically adapts to your domain:

**Stats Theory**: Emphasis on lower bounds, proof completeness, assumption realism
**Policy Research**: Emphasis on identification strategy, confounders, mechanism observability
**Biomedical**: Emphasis on pathway plausibility, clinical relevance, safety considerations

This happens automatically based on DOMAIN.md content.

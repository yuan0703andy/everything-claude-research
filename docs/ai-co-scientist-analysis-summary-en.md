# AI Co-Scientist Quick Start Guide

## What You Can Do

This system helps you generate, test, and refine research hypotheses using three simple commands:

- **/hypothesize** - Generate multiple research hypotheses automatically
- **/stress-test** - Verify hypothesis quality (novelty, assumptions, predictions)
- **/evolve** - Refine hypotheses based on identified issues

No technical knowledge required. Just describe your research goal and let the system guide you.

---

## The Three Core Workflows

### 1. /hypothesize: Generate Research Hypotheses

**When to use**: You have a research question but need concrete hypotheses to test.

**What it does**:
- Generates 3-5 distinct hypotheses from different theoretical angles
- Compares them automatically on novelty and feasibility
- Identifies the top candidate to pursue
- Optional: Runs expert debate to refine the best hypothesis

**Example**:
```bash
/hypothesize "Explain why sparse regression can achieve better rates with adaptive thresholding"

# Output: 3 hypotheses
# - H-001-A: Information-theoretic approach (recommended)
# - H-001-B: Computational approach
# - H-001-C: Robust statistics approach
```

**Decision Guide**: When should I generate hypotheses?

```
START
  |
  +-- Starting a new research project?
  |    └─> Use /hypothesize to explore possibilities
  |
  +-- Have a vague research idea?
  |    └─> Use /hypothesize to make it concrete
  |
  +-- Need alternative approaches?
  |    └─> Use /hypothesize to generate 3-5 options
  |
  +-- High-stakes project (grant, major paper)?
       └─> Use /hypothesize --debate for thorough development
```

---

### 2. /stress-test: Verify Hypothesis Quality

**When to use**: You have a hypothesis and want to know if it's ready for execution.

**What it does**:
- Checks if hypothesis is truly novel vs existing literature
- Verifies that all assumptions are realistic and stated explicitly
- Tests whether hypothesis explains/predicts observed phenomena
- Returns verdict: PASS / GAPS / BLOCKED

**Example**:
```bash
/stress-test H-001-A

# Output:
# Confidence: 7.2/10 - GAPS IDENTIFIED
# - Novelty: 8.5/10 (Strong)
# - Assumptions: 7.0/10 (Some unstated assumptions)
# - Observations: 6.0/10 (Partial support)
#
# Recommendation: Fix gaps with /evolve H-001-A
```

**Decision Guide**: When should I stress-test?

```
Generated hypothesis?
  |
  ├─> ALWAYS run /stress-test before execution
  |
Received hypothesis from colleague?
  |
  ├─> Run /stress-test to identify issues
  |
Ready to write paper?
  |
  └─> Run /stress-test to ensure publication-ready
```

**Verdict Meanings**:

| Verdict | Score | Meaning | Next Step |
|---------|-------|---------|-----------|
| **PASS** | 9.0+ | Ready for execution | Run experiments or write paper |
| **GAPS** | 6.0-8.9 | Fixable issues identified | Use /evolve to address gaps |
| **BLOCKED** | <6.0 | Critical flaws found | Major revision or start over |

---

### 3. /evolve: Refine Your Hypothesis

**When to use**: Stress-test found gaps, or you received critique from reviewers.

**What it does**:
- Reads all critique points from stress-test or reviews
- Generates improved version (v2) that addresses each issue
- Preserves your core insight while fixing problems
- Creates audit trail showing what changed and why

**Example**:
```bash
/evolve H-001-A

# Output: H-001-A-v2
# - Fixed: Incoherence assumption made explicit
# - Fixed: Added missing proof step for lower bound
# - Preserved: Core adaptive thresholding mechanism
#
# Predicted re-test score: 9.2/10 (PASS likely)
```

**Decision Guide**: When should I evolve?

```
/stress-test returned GAPS?
  |
  ├─> Use /evolve to fix automatically
  |
Received reviewer feedback?
  |
  ├─> Use /evolve --review feedback.md
  |
Failed /stress-test multiple times?
  |
  └─> Consider starting fresh with /hypothesize
```

---

## Complete Research Workflows

### Workflow A: Exploring New Directions

**Goal**: Generate and compare multiple hypotheses

```
1. /hypothesize "Your research question"
   Output: H-001-A, H-001-B, H-001-C

2. /stress-test H-001-A
   Output: Score 7.5/10 - GAPS

3. /evolve H-001-A
   Output: H-001-A-v2

4. /stress-test H-001-A-v2
   Output: Score 9.3/10 - PASS

5. Execute experiments or write paper
```

**Time**: 1-2 weeks typical

---

### Workflow B: High-Stakes Hypothesis Development

**Goal**: Develop one thoroughly vetted hypothesis

```
1. /hypothesize "Your research question" --debate
   Output: H-002-A (refined through expert debate)

2. /stress-test H-002-A
   Output: Score 8.0/10 - GAPS

3. /evolve H-002-A
   Output: H-002-A-v2

4. /stress-test H-002-A-v2
   Output: Score 9.5/10 - PASS

5. Execute with high confidence
```

**Time**: 2-3 weeks typical

---

### Workflow C: Responding to Reviewer Critique

**Goal**: Address reviewer comments systematically

```
1. You have: H-003 + reviewer_feedback.md

2. /evolve H-003 --review reviewer_feedback.md
   Output: H-003-v2

3. /stress-test H-003-v2
   Output: Verify all issues addressed

4. Resubmit or iterate if needed
```

**Time**: 3-5 days typical

---

## Decision Tree: Which Command Do I Need?

```
START: What do you want to do?
  |
  +-- I have a research question but no hypothesis
  |    └─> /hypothesize
  |
  +-- I have a hypothesis and want to check quality
  |    └─> /stress-test
  |
  +-- My hypothesis has known issues
  |    └─> /evolve
  |
  +-- I want to compare multiple hypotheses
  |    └─> /hypothesize (generates 3-5 options automatically)
  |
  +-- I need to respond to reviewer feedback
  |    └─> /evolve --review feedback.md
  |
  +-- I'm not sure if my hypothesis is novel
       └─> /stress-test (includes novelty check)
```

---

## Real Examples by Research Type

### Statistical Theory Research

```bash
# Generate hypothesis about minimax rates
/hypothesize "Derive optimal rates for sparse PCA under heavy-tailed noise"

# Output: 3 hypotheses
# - H-001-A: Information-theoretic (Fano's method)
# - H-001-B: Computational (polynomial-time)
# - H-001-C: Robust approach (contamination model)

# Test top candidate
/stress-test H-001-A

# Gap found: Missing lower bound proof
/evolve H-001-A

# Re-test improved version
/stress-test H-001-A-v2
# Result: PASS - Ready to write paper
```

**Common gaps in stats theory**: Missing lower bounds, unstated assumptions, incomplete proofs

---

### Policy Research

```bash
# Generate hypothesis about policy adoption
/hypothesize "Explain why some states adopt climate policies and others don't" --debate

# Output: H-002-A (debate-refined)
# Mechanism: Political polarization + policy diffusion

# Test hypothesis
/stress-test H-002-A --observations data/state_policies.csv

# Gap found: Identification strategy unclear
/evolve H-002-A

# Re-test
/stress-test H-002-A-v2
# Result: PASS - Clear RDD identification strategy
```

**Common gaps in policy**: Weak identification, confounders, selection bias

---

### Biomedical Research

```bash
# Generate drug target hypotheses
/hypothesize "Identify novel drug targets for AML treatment"

# Output: 3 mechanisms
# - H-003-A: CXCR1/2 inhibitor
# - H-003-B: MEK pathway
# - H-003-C: Epigenetic modifiers

# Test novelty (high priority in biomedical)
/stress-test H-003-A

# Gap found: Mechanism not fully supported by observations
/evolve H-003-A

# Re-test
/stress-test H-003-A-v2
# Result: PASS - Ready for wet lab validation
```

**Common gaps in biomedical**: Weak mechanistic support, pathway plausibility, clinical relevance

---

## Command Options Summary

### /hypothesize Options

| Flag | Effect | When to Use |
|------|--------|-------------|
| `--debate` | Expert panel refinement | High-stakes projects |
| `--generate-only` | Skip comparison | Want maximum diversity |
| `--count=N` | Generate N hypotheses | Need more/fewer options |

### /stress-test Options

| Flag | Effect | When to Use |
|------|--------|-------------|
| `--quick` | Skip observation matching | No data yet |
| `--observations=<file>` | Provide data file | Have experimental results |
| `--strict` | Apply stricter standards | Top-tier journal submission |

### /evolve Options

| Flag | Effect | When to Use |
|------|--------|-------------|
| `--review=<file>` | Use specific review | Have reviewer comments |
| `--conservative` | Minimal changes | Preserve original closely |
| `--aggressive` | Allow major changes | Fundamental issues exist |

---

## Troubleshooting

### "Hypotheses are too similar"

**Problem**: /hypothesize generates very similar hypotheses

**Solutions**:
1. Make research goal more specific
2. Check that DOMAIN.md includes diverse theoretical frameworks
3. Use --count=5 to get more options

---

### "Stress-test returns BLOCKED"

**Problem**: Score below 6.0, critical flaws found

**Solutions**:
1. Read the stress-test report carefully - is the flaw fixable?
2. Try /evolve to see if it can be salvaged
3. If evolution fails, start fresh with /hypothesize using lessons learned

---

### "Evolution doesn't fix my issue"

**Problem**: /evolve output still has the same problem

**Solutions**:
1. Use --focus flag to target specific issue
2. Provide --review with detailed description of what needs fixing
3. After 3 evolution attempts, consider whether hypothesis is viable

---

### "I want more control"

**Problem**: Commands are too automated

**Solutions**:
1. This is intentional - automation ensures thought continuity
2. For manual control, edit hypothesis files directly
3. After manual edits, run /stress-test to verify

---

## Best Practices

### 1. Always stress-test before execution
Don't skip verification. Issues found early save time later.

### 2. Expect iteration
First hypothesis rarely passes stress-test. 2-3 iterations is normal.

### 3. Use debate mode for important work
High-stakes projects (grants, major papers) benefit from --debate flag.

### 4. Keep original versions
System preserves H-001 when creating H-001-v2. Review evolution history.

### 5. Trust the verdict
PASS means ready. GAPS means fixable. BLOCKED means reconsider.

### 6. Domain matters
Update DOMAIN.md with field-specific standards. System adapts automatically.

---

## What's Happening Behind the Scenes?

You don't need to know this to use the system, but for the curious:

### /hypothesize
- Spawns theorist-enhanced agent
- Loads DOMAIN.md for field standards
- Generates 3-5 candidates with different mechanisms
- Compares using weighted scoring (novelty 25%, importance 25%, testability 20%, etc.)
- Optional: Runs 5-turn expert debate simulation

### /stress-test
- Spawns verifier-enhanced agent
- Performs three checks in one unified workflow:
  1. Novelty (vs literature)
  2. Observation matching (does it explain data?)
  3. Assumption validation (are they realistic?)
- Cross-checks consistency between dimensions
- Returns holistic score and actionable gaps

### /evolve
- Spawns theorist-enhanced agent
- Reads original hypothesis + all critique
- Chooses evolution strategy (simplification, strengthening, differentiation)
- Generates v2 while preserving core contribution
- Creates audit trail of changes

**Why automated?** Maintains thought continuity. Agent remembers context across all checks, identifies interactions between issues, provides coherent solution.

---

## Integration with Existing Work

These commands work with your existing research workflow:

```
/hypothesize          New command - Generate hypotheses
    |
    v
/stress-test          New command - Verify quality
    |
    v
/evolve               New command - Fix issues
    |
    v
/execute-analysis     Existing - Run experiments
    |
    v
/verify-results       Existing - Check results
```

The new commands fill the gap between "research idea" and "execution-ready hypothesis."

---

## Next Steps

### Getting Started

1. **Read this guide** (you're almost done!)
2. **Try with real research question**: Start with /hypothesize
3. **Follow the recommendations**: System will guide you to next step
4. **Iterate**: Expect 2-3 rounds of stress-test / evolve

### Getting Help

- Stuck? Run /stress-test - it will tell you what's wrong
- Not sure which command? Use decision tree above
- Want examples? See "Real Examples by Research Type" section

### Remember

- These are assistants, not replacements for domain expertise
- Always review outputs critically
- System accelerates workflow but you make final decisions
- Document what works for your research type

---

## Quick Reference Card

```
GENERATE HYPOTHESES
  /hypothesize "Your research question"
  Add --debate for high-stakes work

VERIFY QUALITY
  /stress-test H-001
  PASS (9+) = Ready
  GAPS (6-9) = Fixable
  BLOCKED (<6) = Major issues

FIX ISSUES
  /evolve H-001
  Creates H-001-v2 addressing gaps
  Re-run /stress-test after

TYPICAL CYCLE
  hypothesize → stress-test → evolve → stress-test → PASS
  Time: 1-3 weeks depending on complexity
```

---

*This system implements research workflows from Google's AI Co-Scientist paper, adapted for general scientific research across domains.*

*Last Updated: 2026-01-27*

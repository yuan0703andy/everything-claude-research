# AI Co-Scientist User Manual

## Introduction

This manual teaches you how to use the **domain-agnostic** AI Co-Scientist system for research hypothesis development. The system works across any research field - statistical theory, policy research, biomedical science, economics, psychology, or any domain you configure.

**Key insight**: The framework provides universal research workflows (hypothesis generation, verification, evolution) that automatically adapt to your field's specific standards through DOMAIN.md configuration.

**What you'll learn**:
- How to configure DOMAIN.md for your research field
- How to use the three core commands (/hypothesize, /stress-test, /evolve)
- How the system adapts to different research domains
- Complete workflows for different research types
- Troubleshooting common issues
- Best practices from real research projects

**Time to read**: 30-45 minutes

**Supported domains**: Any research field - just configure DOMAIN.md with your standards

---

## Table of Contents

1. [Domain Configuration](#domain-configuration)
2. [Getting Started](#getting-started)
3. [The Three Commands](#the-three-commands)
4. [Complete Workflows](#complete-workflows)
5. [Domain-Specific Guides](#domain-specific-guides)
6. [Integration Patterns](#integration-patterns)
7. [Troubleshooting](#troubleshooting)
8. [Best Practices](#best-practices)
9. [Advanced Usage](#advanced-usage)

---

## Domain Configuration

**Most important section**: Read this first to understand how the system adapts to your field.

### The Domain-Agnostic Design

This framework provides **universal research workflows** that work across any field:

- **Hypothesis generation** → Adapted to your field's theoretical frameworks
- **Verification** → Adapted to your field's quality standards
- **Evolution** → Adapted to your field's common issues

The adaptation happens through a single configuration file: `DOMAIN.md`

### What DOMAIN.md Contains

Each research domain has a DOMAIN.md file defining:

1. **Core Theoretical Frameworks** - The foundations of your field
   - Stats: Minimax theory, empirical processes, information theory
   - Policy: Causal inference, institutional theory, policy diffusion
   - Biomedical: Molecular pathways, clinical trial design, drug discovery

2. **Evidence Quality Tiers** - What counts as strong evidence in your field
   - Tier 1 (Gold): Field-specific gold standard (peer-reviewed proofs, RCTs, etc.)
   - Tier 2 (Silver): Strong supporting evidence
   - Tier 3 (Bronze): Preliminary evidence
   - Tier 4 (Exploratory): Speculative or anecdotal

3. **Ranking Dimensions** - How to evaluate hypotheses in your field
   - Universal: Novelty, importance, testability
   - Domain-specific: Proof completeness (stats), identification clarity (policy), clinical relevance (biomedicine)

4. **Verification Standards** - What makes a hypothesis "ready" in your field
   - Stats: Lower bound + upper bound + explicit assumptions
   - Policy: Clear identification + confounder analysis + robustness
   - Biomedical: Mechanism validation + pathway plausibility + safety

5. **Common Pitfalls** - Field-specific issues to watch for
   - Stats: Missing lower bounds, unrealistic assumptions
   - Policy: Weak identification, selection bias
   - Biomedical: Mechanism not validated, clinical relevance unclear

### Setting Up Your Domain

**Option 1**: Use existing domain configuration
```bash
# Copy existing domain as starting point
cd /Users/andyhou/research
cp -r domains/stats-theory domains/[your-field]
```

**Option 2**: Create new domain from template
```bash
# Start with stats-theory as template
cp domains/stats-theory/DOMAIN.md domains/[your-field]/DOMAIN.md

# Edit to customize:
# 1. Replace "minimax theory" with your core frameworks
# 2. Update evidence tiers (what's Tier 1 in your field?)
# 3. Define ranking dimensions (what matters in your field?)
# 4. Specify verification standards (when is hypothesis ready?)
# 5. List common pitfalls (what goes wrong in your field?)
```

**Included domains**:
- `stats-theory/` - Statistical theory, minimax rates, asymptotic analysis
- `policy-research/` - Causal inference, policy adoption, institutional analysis
- `biomedical/` - Drug discovery, disease mechanisms, clinical translation

### How Commands Use DOMAIN.md

```bash
# When you run:
/hypothesize "Your question"

# System does:
1. Read DOMAIN.md for your project
2. Load theoretical frameworks from DOMAIN.md
3. Generate hypotheses using domain-appropriate approaches
4. Rank using domain-specific dimensions

# When you run:
/stress-test H-001-A

# System does:
1. Read DOMAIN.md verification standards
2. Apply domain-specific checks
3. Use domain evidence tiers for novelty assessment
4. Flag domain-specific pitfalls

# When you run:
/evolve H-001-A

# System does:
1. Read DOMAIN.md common issues
2. Apply domain-specific fix strategies
3. Preserve domain-important aspects
```

### Example: Same Command, Different Domains

**Statistical Theory** (uses `stats-theory/DOMAIN.md`):
```bash
/hypothesize "Optimal estimation problem"
# → Generates: Information-theoretic, computational, robust approaches
# → Focus: Minimax rates, lower bounds, assumptions

/stress-test H-001-A
# → Checks: Lower bound proof, assumption realism, rate optimality
```

**Policy Research** (uses `policy-research/DOMAIN.md`):
```bash
/hypothesize "Policy adoption variation"
# → Generates: Institutional, political economy, diffusion mechanisms
# → Focus: Causal identification, observable implications

/stress-test H-001-A
# → Checks: Identification strategy, confounders, selection bias
```

**Biomedical** (uses `biomedical/DOMAIN.md`):
```bash
/hypothesize "Therapeutic target discovery"
# → Generates: Pathway A, B, C interventions
# → Focus: Mechanism, druggability, clinical relevance

/stress-test H-001-A
# → Checks: Pathway validation, mechanism plausibility, safety
```

**The commands are identical. The domain configuration makes them adapt.**

---

## Getting Started

### Your First Hypothesis Generation

Let's walk through a simple example. **The commands are the same regardless of domain** - only DOMAIN.md differs.

**Step 1: Generate hypotheses**

```bash
# The command is identical across all domains
/hypothesize "[Your research question in any field]"
```

**What happens**:
- System reads DOMAIN.md for your field
- Generates 3-5 hypotheses using domain-appropriate frameworks
- Each hypothesis includes mechanism, predictions, and key references
- System ranks using domain-specific dimensions

**Output example** (adapts to domain):

**Statistical Theory** (from stats-theory/DOMAIN.md):
```
H-001-A: Information-theoretic approach
  Framework: Fano's method for lower bound
  Novelty: 8.5/10 (New regime)
  Proof completeness: 8/10 (Lower bound strategy clear)
  Status: TOP CANDIDATE

H-001-B: Computational complexity approach
  Framework: Oracle complexity lower bound
  Novelty: 7/10 (Extends existing)
  Feasibility: 9/10 (Constructive proof)

H-001-C: Robust statistics approach
  Framework: Contamination model
  Novelty: 9/10 (Novel framework)
  Feasibility: 5/10 (Proof challenging)
```

**Policy Research** (from policy-research/DOMAIN.md):
```
H-001-A: Institutional diffusion mechanism
  Framework: Policy diffusion + institutional theory
  Novelty: 8.5/10 (New mechanism)
  Identification: 8/10 (Spatial RDD viable)
  Status: TOP CANDIDATE

H-001-B: Political economy mechanism
  Framework: Interest group competition
  Novelty: 7/10 (Known framework)
  Identification: 6/10 (Selection concerns)

H-001-C: Fiscal federalism mechanism
  Framework: Vertical competition
  Novelty: 9/10 (Novel angle)
  Identification: 4/10 (Weak identification)
```

**Same /hypothesize command, different domain standards applied.**

**Step 2: Verify quality**

```bash
# Identical command across domains
/stress-test H-001-A
```

**What happens**:
- System reads DOMAIN.md verification standards
- Applies domain-specific quality checks
- Uses domain evidence tiers for novelty assessment
- Returns verdict with domain-relevant gaps

**Output example** (adapts to domain):

**Statistical Theory**:
```
Stress Test Results: H-001-A

Overall Score: 7.2/10 - GAPS IDENTIFIED

Novelty: 8.5/10 (Strong)
  - Novel: Adaptive thresholding in heavy-tail regime
  - Builds on: Donoho & Johnstone (1994)

Proof Completeness: 7.0/10 (Issues found)
  Gap: Lower bound proof incomplete (Fano construction missing)
  Gap: Moment assumptions not explicit (E|ε|^p for which p?)

Assumptions: 6.0/10 (Too strong)
  Gap: Incoherence μ(X) < 0.1 unrealistic
  Gap: Restricted eigenvalue condition not justified

Verdict: GAPS - Use /evolve to fix
```

**Policy Research**:
```
Stress Test Results: H-001-A

Overall Score: 7.0/10 - GAPS IDENTIFIED

Novelty: 8.0/10 (Strong)
  - Novel: Diffusion + polarization interaction
  - Builds on: Shipan & Volden (2008)

Identification: 6.5/10 (Issues found)
  Gap: Spatial RDD continuity assumption not verified
  Gap: Timing of diffusion unclear (selection bias?)

Confounders: 6.0/10 (Incomplete)
  Gap: Federal pressure omitted
  Gap: State fiscal capacity not controlled

Verdict: GAPS - Use /evolve to fix
```

**Same command, domain-specific checks applied.**

**Step 3: Fix issues**

```bash
# Identical command across domains
/evolve H-001-A
```

**What happens**:
- System reads DOMAIN.md common pitfalls
- Reads all gaps from stress-test
- Applies domain-appropriate fix strategies
- Preserves domain-important aspects

**Output example** (adapts to domain):

**Statistical Theory**:
```
Evolution Complete: H-001-A → H-001-A-v2

Domain-specific fixes:
  1. Added complete Fano lower bound derivation
  2. Made moment assumption explicit: E|ε|^p < ∞ for p ≥ 2
  3. Relaxed incoherence: μ(X) ≤ C (literature standard)
  4. Justified restricted eigenvalue via sparse eigenvalue

Core preserved:
  - Minimax rate n^(-2β/(2β+1)) unchanged
  - Adaptive thresholding mechanism intact
  - Upper bound construction preserved

Predicted re-test score: 9.2/10 (PASS likely)
```

**Policy Research**:
```
Evolution Complete: H-001-A → H-001-A-v2

Domain-specific fixes:
  1. Added spatial RDD identification strategy
  2. Included continuity tests at state borders
  3. Added federal pressure control variable
  4. Included event study for timing

Core preserved:
  - Diffusion + polarization mechanism intact
  - Observable implications unchanged
  - Theory-driven predictions preserved

Predicted re-test score: 8.8/10 (GAPS → near PASS)
```

**Same command, domain-specific evolution strategies.**

**Step 4: Re-verify**

```bash
/stress-test H-001-A-v2
```

**Output**:
```
Overall Score: 9.3/10 - PASS

All issues resolved. Hypothesis ready for execution.
```

**You're done!** This took ~15 minutes of your time, saved days of manual iteration.

---

## The Three Commands

### Command 1: /hypothesize

**Purpose**: Generate multiple research hypotheses automatically.

**Basic Usage**:
```bash
/hypothesize "Your research question or goal"
```

**What it does**:
1. Generates 3-5 hypotheses from different theoretical angles
2. Each hypothesis includes:
   - Core claim (what you're arguing)
   - Mechanism (why it works)
   - Predictions (what we should observe)
   - Testability assessment
   - Novelty score
3. Compares candidates automatically
4. Recommends top candidate

**When to use**:
- Starting new research project
- Have vague idea, need concrete hypotheses
- Want to explore multiple approaches
- Need alternatives to current approach

**Options**:

```bash
# Standard: Generate and compare
/hypothesize "Your question"

# With debate: Expert panel refinement for top candidate
/hypothesize "Your question" --debate

# Generate only: Skip comparison, get maximum diversity
/hypothesize "Your question" --generate-only

# Custom count: Generate N hypotheses (default 3-5)
/hypothesize "Your question" --count=5
```

**Output Structure**:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HYPOTHESIZE ► [Your Research Goal]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Generated Hypotheses (3):

H-XXX-A: [Title] ⭐ TOP CANDIDATE
  Angle: [Theoretical approach]
  Novelty: X/5
  Feasibility: X/5
  Core Claim: [One sentence]
  Mechanism: [How it works]

[Additional hypotheses...]

Recommendation:
  Prioritize: H-XXX-A
  Alternative: H-XXX-B
  Archive: H-XXX-C

Files Created:
  - hypotheses/proposals/H-XXX-A-[slug].md
  - hypotheses/proposals/H-XXX-B-[slug].md
  - hypotheses/proposals/H-XXX-C-[slug].md
  - Updated: hypotheses/HYPOTHESES.md

Next: /stress-test H-XXX-A
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Tips**:
- Be specific in research question - vague questions get vague hypotheses
- Use --debate for high-stakes work (grants, major papers)
- Review all candidates, not just top one - alternatives may be useful later
- Domain standards (from DOMAIN.md) automatically applied

---

### Command 2: /stress-test

**Purpose**: Verify hypothesis quality before execution.

**Basic Usage**:
```bash
/stress-test H-001
```

**What it checks**:
1. **Novelty**: Is hypothesis truly novel vs existing literature?
2. **Assumptions**: Are all assumptions stated explicitly and realistic?
3. **Observations**: Does hypothesis explain/predict known phenomena?

**Returns**: Unified score + verdict + specific gaps

**When to use**:
- Before executing experiments (always!)
- After receiving hypothesis from colleague
- Before writing paper
- After making major revisions

**Options**:

```bash
# Standard: Full verification (all three checks)
/stress-test H-001

# Quick: Novelty + assumptions only (no data needed)
/stress-test H-001 --quick

# With data: Include observation matching
/stress-test H-001 --observations data/results.csv

# Strict mode: Apply top-tier journal standards
/stress-test H-001 --strict
```

**Verdicts Explained**:

| Verdict | Score | What it means | What to do |
|---------|-------|---------------|------------|
| **PASS** | 9.0+ | Ready for execution | Proceed with experiments or writing |
| **GAPS** | 6.0-8.9 | Fixable issues found | Use /evolve to address |
| **BLOCKED** | <6.0 | Critical flaws | Major revision or start over |

**Output Structure**:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STRESS TEST ► H-001: [Title]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Overall Verdict: [PASS / GAPS / BLOCKED]
Confidence Score: X.X/10

1. Novelty Verification (X/10)
   Novel Aspects:
     - [What's new]
   Known Aspects:
     - [What exists]
   Literature Comparison:
     [Table comparing to prior work]

2. Observation Matching (X/10)
   Predictions vs Evidence:
     [Table of predictions and support]
   Missing Pieces:
     [What's not yet validated]

3. Assumption Decomposition (X/10)
   Explicit Assumptions:
     [List with validity assessment]
   Implicit Assumptions (Discovered):
     [Hidden assumptions found]
   Red Flags:
     [Critical issues]

Critical Gaps Identified:
  Gap 1: [Description]
    Impact: [Why it matters]
    Fix: [How to address]
    Effort: [Time estimate]

Recommendation:
  [Next step based on verdict]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Tips**:
- Don't skip stress-test even if hypothesis "looks good"
- First stress-test usually finds gaps - this is normal
- GAPS verdict means fixable - use /evolve
- BLOCKED verdict means fundamental issues - reconsider approach
- Domain-specific standards automatically applied

---

### Command 3: /evolve

**Purpose**: Automatically refine hypothesis based on critique.

**Basic Usage**:
```bash
/evolve H-001
```

**What it does**:
1. Reads all critique from stress-test (or provided review)
2. Analyzes each gap's severity and type
3. Generates improved version (H-001-v2) addressing issues
4. Preserves core insight
5. Creates audit trail of changes

**When to use**:
- Stress-test returned GAPS verdict
- Received reviewer feedback
- Have critique from colleagues
- Want systematic refinement

**Options**:

```bash
# Standard: Evolve based on most recent stress-test
/evolve H-001

# With review: Use specific critique document
/evolve H-001 --review feedback/reviewer_comments.md

# Conservative: Minimal changes only
/evolve H-001 --conservative

# Aggressive: Allow major restructuring
/evolve H-001 --aggressive

# Focused: Target specific aspect
/evolve H-001 --focus=assumptions
```

**Evolution Strategies** (automatically selected):

| Gap Type | Strategy | What Changes | What Preserves |
|----------|----------|--------------|----------------|
| Feasibility | Simplification | Methods, design | Core claim |
| Rigor | Strengthening | Assumptions, proofs | Mechanism |
| Novelty | Differentiation | Positioning | Technical content |
| Scope | Refinement | Boundaries | Central contribution |

**Output Structure**:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
EVOLVE ► H-001 → H-001-v2
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Evolution Summary:
  Strategy: [Type of improvement]
  Critiques Addressed: [Number]

Critiques Addressed:

  1. [Critique description]
     Original Problem: [What was wrong]
     Evolution: [How it was fixed]
     Status: ✅ Resolved

  [Additional critiques...]

What Changed:
  [Detailed list of modifications]

What Was Preserved:
  ✓ [Core insights kept intact]

Evolution Metrics:
  [Before vs After comparison table]

Files Created:
  - hypotheses/proposals/H-001-v2-[slug].md
  - hypotheses/evolution/H-001-evolution-log.md
  - Updated: hypotheses/HYPOTHESES.md

Next: /stress-test H-001-v2

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Tips**:
- Original hypothesis (H-001) preserved, can always compare
- Typically 1-2 evolution cycles to reach PASS
- After 3 cycles without PASS, reconsider if hypothesis is viable
- Evolution can't fix fundamentally flawed hypotheses
- Review evolved version critically - automation isn't perfect

---

## Complete Workflows

### Workflow 1: Exploratory Research

**Scenario**: Starting new project, exploring multiple directions.

**Goal**: Generate diverse candidates, identify most promising.

**Steps**:

```bash
# Week 1: Generate diverse hypotheses
/hypothesize "Your broad research question"
# Output: H-001-A, H-001-B, H-001-C

# Week 1-2: Quick screen all candidates
/stress-test H-001-A --quick
/stress-test H-001-B --quick
/stress-test H-001-C --quick
# Identify: H-001-A has highest potential

# Week 2: Full verification of top candidate
/stress-test H-001-A
# Result: 7.5/10 - GAPS

# Week 2-3: Fix issues
/evolve H-001-A
# Output: H-001-A-v2

# Week 3: Re-verify
/stress-test H-001-A-v2
# Result: 9.2/10 - PASS

# Week 3+: Execute experiments
[Your experimental work]
```

**Timeline**: 3-4 weeks from question to execution-ready hypothesis

**Key decisions**:
- Use standard /hypothesize (not --debate) for speed
- Use --quick for initial screening (saves time)
- Focus effort on top 1-2 candidates
- Keep alternatives documented for later

---

### Workflow 2: High-Stakes Development

**Scenario**: Grant proposal, major paper, high-impact project.

**Goal**: One thoroughly vetted hypothesis with maximum confidence.

**Steps**:

```bash
# Week 1-2: Generate through expert debate
/hypothesize "Your specific research question" --debate
# Output: H-002-A (debate-refined, higher quality)

# Week 2-3: Full verification with strict standards
/stress-test H-002-A --strict
# Result: 8.0/10 - GAPS (strict mode finds more)

# Week 3-4: Address all issues
/evolve H-002-A
# Output: H-002-A-v2

# Week 4: Re-verify with strict standards
/stress-test H-002-A-v2 --strict
# Result: 9.5/10 - PASS

# Week 4-5: Additional validation (optional)
# - Share with colleagues for manual review
# - Run pilot experiments to test feasibility
# - Check against recent literature manually

# Week 5+: Execute with high confidence
[Your experimental work]
```

**Timeline**: 5-6 weeks from question to execution

**Key decisions**:
- Use --debate flag for hypothesis generation
- Use --strict for stress-testing
- Expect multiple evolution cycles
- Invest time upfront for confidence later
- Consider manual validation steps

---

### Workflow 3: Responding to Reviewers

**Scenario**: Paper revision, addressing reviewer critique.

**Goal**: Systematically address all reviewer comments.

**Steps**:

```bash
# You have: H-003 + reviewer_comments.md

# Day 1: Understand issues
# Read reviewer_comments.md
# Identify which comments are about hypothesis vs execution

# Day 1-2: Evolve based on specific feedback
/evolve H-003 --review reviewer_comments.md
# Output: H-003-v2

# Day 2-3: Verify all issues addressed
/stress-test H-003-v2
# Check that specific reviewer concerns are resolved

# If PASS: Update paper
# If GAPS: Iterate
  /evolve H-003-v2
  /stress-test H-003-v3

# Day 3-5: Rewrite affected sections
[Update paper based on H-003-v2]

# Day 5: Final check
/stress-test H-003-v2 --strict
# Ensure revision meets high standards
```

**Timeline**: 3-5 days typical

**Key decisions**:
- Use --review flag with reviewer comments
- Check stress-test output against specific comments
- May need manual refinement for some issues
- Re-run experiments if hypothesis fundamentally changed

---

### Workflow 4: Iterative Refinement

**Scenario**: Working hypothesis that needs continuous improvement.

**Goal**: Incremental improvement through multiple cycles.

**Steps**:

```bash
# Cycle 1
/hypothesize "Initial question"
# → H-004-A

/stress-test H-004-A
# → Score 6.5/10 - Multiple gaps

/evolve H-004-A
# → H-004-A-v2

# Cycle 2
/stress-test H-004-A-v2
# → Score 7.8/10 - Fewer gaps

/evolve H-004-A-v2
# → H-004-A-v3

# Cycle 3
/stress-test H-004-A-v3
# → Score 9.0/10 - PASS

# If still not PASS after 3 cycles:
# Reconsider whether hypothesis is viable
# May need to start fresh with /hypothesize
```

**Timeline**: 1-3 weeks depending on issues

**Key decisions**:
- Limit to 3 evolution cycles
- After 3 cycles without PASS, reconsider approach
- Each cycle should show improvement
- Track what's changing across versions

---

## Domain-Specific Guides

### Statistical Theory Research

**Common research questions**:
- Derive minimax optimal rates
- Prove lower bounds
- Design optimal estimators
- Establish computational complexity

**Typical workflow**:

```bash
# Generate with theory emphasis
/hypothesize "Derive minimax rates for [problem]"

# Hypotheses will include:
# - Upper bound construction
# - Lower bound strategy (Fano, Le Cam, etc.)
# - Proof sketch
# - Assumption list

# Stress-test focuses on:
/stress-test H-001-A
# - Proof completeness
# - Assumption realism
# - Rate tightness
# - Computational tractability

# Common gaps in stats theory:
# - Missing lower bound
# - Assumptions not stated
# - Proof steps incomplete
# - No computational complexity analysis

# Evolution typically adds:
/evolve H-001-A
# - Missing proof steps
# - Explicit assumptions
# - Computational analysis
# - Comparison to prior rates
```

**Domain-specific tips**:
- Always check if lower bound included (required for minimax claims)
- Explicit assumption listing critical (Annals standard)
- Proof completeness matters more than novelty score
- Consider computational-statistical tradeoffs

**Example end-to-end**:

```bash
# Research goal: Optimal rates for sparse PCA
/hypothesize "Derive minimax optimal rates for sparse PCA under heavy-tailed noise"

# Generated hypotheses focus on:
# H-001-A: Information-theoretic approach (Fano's method)
# H-001-B: Computational approach (poly-time algorithm)
# H-001-C: Robust statistics approach

# Test top candidate
/stress-test H-001-A

# Typical gaps:
# - Gap 1: Lower bound proof incomplete
# - Gap 2: Heavy-tail assumption (moment bound) not specified
# - Gap 3: Restricted eigenvalue condition not justified

# Fix systematically
/evolve H-001-A

# H-001-A-v2 includes:
# - Complete Fano lower bound derivation
# - Explicit moment assumptions (E|ε|^p < ∞ for p≥2)
# - Conditions where restricted eigenvalue holds

/stress-test H-001-A-v2
# Result: PASS - Ready for writeup
```

---

### Policy Research

**Common research questions**:
- Explain policy adoption patterns
- Identify causal effects of policies
- Understand policy diffusion mechanisms
- Assess policy effectiveness

**Typical workflow**:

```bash
# Generate with causal emphasis
/hypothesize "Explain why [policy outcome varies]"

# Hypotheses will include:
# - Causal mechanism
# - Identification strategy
# - Confounders to address
# - Observable implications

# Stress-test with observations
/stress-test H-002-A --observations data/policy_data.csv

# Common gaps in policy:
# - Identification strategy unclear
# - Selection bias not addressed
# - Confounders omitted
# - Mechanism not testable

# Evolution typically adds:
/evolve H-002-A
# - Clear identification (RDD, IV, DID)
# - Confounder analysis
# - Robustness checks
# - Observable mechanism steps
```

**Domain-specific tips**:
- Identification strategy critical (APSR standard)
- Address selection bias explicitly
- Include robustness checks in hypothesis
- Test against actual policy data using --observations flag

**Example end-to-end**:

```bash
# Research goal: Climate policy adoption
/hypothesize "Explain why some states adopt climate policies and others don't" --debate

# Debate-refined hypothesis (H-002-A):
# Mechanism: Political polarization + policy diffusion
# Claim: Republican states adopt only if neighbor states do
# Identification: Spatial RDD at state borders

# Test with data
/stress-test H-002-A --observations data/state_climate_policies.csv

# Typical gaps:
# - Gap 1: RDD continuity assumption not verified
# - Gap 2: Policy diffusion timing unclear
# - Gap 3: Omitted variable: federal pressure

# Fix identification issues
/evolve H-002-A

# H-002-A-v2 includes:
# - Explicit continuity tests
# - Event study for timing
# - Federal pressure control
# - Placebo tests

/stress-test H-002-A-v2
# Result: PASS - Clear causal identification
```

---

### Biomedical Research

**Common research questions**:
- Identify drug targets
- Explain disease mechanisms
- Discover biomarkers
- Understand pathway interactions

**Typical workflow**:

```bash
# Generate with mechanism emphasis
/hypothesize "Identify novel therapeutic targets for [disease]"

# Hypotheses will include:
# - Molecular mechanism
# - Pathway interactions
# - Druggability assessment
# - Clinical relevance

# Stress-test for novelty (critical in bio)
/stress-test H-003-A

# Common gaps in biomedical:
# - Mechanism not fully supported
# - Pathway plausibility weak
# - Clinical relevance unclear
# - Safety concerns not addressed

# Evolution typically adds:
/evolve H-003-A
# - Detailed pathway steps
# - Supporting evidence from literature
# - Clinical trial considerations
# - Safety profile
```

**Domain-specific tips**:
- Novelty check critical (crowded field)
- Mechanism must be biologically plausible
- Clinical relevance matters for funding
- Consider druggability early

**Example end-to-end**:

```bash
# Research goal: AML drug targets
/hypothesize "Identify novel drug targets for AML treatment"

# Generated mechanisms:
# H-003-A: CXCR1/2 inhibitor (immune pathway)
# H-003-B: MEK pathway targeting
# H-003-C: Epigenetic modifiers

# Test novelty + mechanism
/stress-test H-003-A

# Typical gaps:
# - Gap 1: CXCR1/2 in AML not well-established
# - Gap 2: Immune mechanism unclear for AML
# - Gap 3: Clinical feasibility (safety in patients)

# Add mechanistic detail
/evolve H-003-A

# H-003-A-v2 includes:
# - CXCR1/2 expression data in AML
# - Immune cell trafficking mechanism
# - Safety data from other cancers
# - Combination therapy potential

/stress-test H-003-A-v2
# Result: PASS - Ready for wet lab validation
```

---

## Integration Patterns

### Pattern 1: Sequential Integration (Standard Flow)

**When to use**: Normal research workflow

```
/hypothesize
    ↓
/stress-test
    ↓
/evolve (if needed)
    ↓
/stress-test (re-verify)
    ↓
[Execution: experiments, analysis, writing]
```

**Commands work together**:
- /hypothesize output feeds directly to /stress-test
- /stress-test critique feeds to /evolve
- /evolve output ready for /stress-test

**No manual file management needed** - system tracks everything.

---

### Pattern 2: Parallel Exploration

**When to use**: Comparing multiple hypotheses

```
/hypothesize → H-A, H-B, H-C
    ↓
/stress-test H-A --quick   ┐
/stress-test H-B --quick   ├─ Parallel
/stress-test H-C --quick   ┘
    ↓
Select best → /stress-test H-A (full)
    ↓
/evolve H-A → /stress-test H-A-v2
```

**Key points**:
- Use --quick for initial screening
- Run tests in parallel (saves time)
- Full test only on top candidates
- Keep alternatives documented

---

### Pattern 3: Iterative Refinement Loop

**When to use**: Complex hypotheses needing multiple cycles

```
/hypothesize → H-001
    ↓
    ┌─────────────────┐
    │ /stress-test    │
    │       ↓         │
    │ Gaps found?     │
    │       ↓         │
    │ /evolve         │
    │       ↓         │
    └─────────────────┘
         ↓
    PASS → Execute
```

**Termination conditions**:
- PASS verdict → Stop, proceed to execution
- 3 cycles without PASS → Reconsider hypothesis
- Score not improving → Check if fixable

---

### Pattern 4: Review-Driven Evolution

**When to use**: Responding to external feedback

```
[Existing hypothesis] + [External review]
    ↓
/evolve H-001 --review feedback.md
    ↓
/stress-test H-001-v2
    ↓
Verify review concerns addressed
    ↓
If yes → Update work
If no → /evolve again with focused critique
```

**Sources of external review**:
- Peer reviewers
- Colleague feedback
- Lab meeting notes
- Advisor comments

---

### Pattern 5: Multi-Stage Pipeline

**When to use**: Complete research project from start to finish

```
Research Question
    ↓
/hypothesize (exploration)
    ↓
/stress-test (quality gate)
    ↓
/evolve (refinement)
    ↓
/stress-test (re-gate)
    ↓
[Execute experiments] (your work)
    ↓
/verify-results (check results meet goals)
    ↓
[Write paper] (your work)
    ↓
/stress-test (final check before submission)
```

**Quality gates**:
- Gate 1: After hypothesis generation (pass stress-test)
- Gate 2: Before experiments (PASS verdict)
- Gate 3: After experiments (verify-results)
- Gate 4: Before submission (final stress-test)

---

## Troubleshooting

### Issue 1: "Hypotheses are too similar"

**Symptoms**:
- /hypothesize generates 3 hypotheses that differ only in minor details
- All have same mechanism, just different framing

**Diagnosis**:
```bash
# Check your research question
# Too broad? "Study machine learning"
# Or too specific? "Prove Theorem 3.2 using Lemma 5"
```

**Solutions**:

1. **Make question more specific but not overly constrained**:
```bash
# Bad: Too vague
/hypothesize "Study regression"

# Bad: Too specific
/hypothesize "Use Fano's lemma with ε=0.1"

# Good: Specific goal, flexible method
/hypothesize "Derive minimax optimal rates for sparse regression under heavy tails"
```

2. **Check DOMAIN.md has diverse frameworks**:
```bash
# If DOMAIN.md only mentions one approach,
# all hypotheses will use that approach
# Add multiple theoretical frameworks
```

3. **Increase diversity explicitly**:
```bash
/hypothesize "Your question" --count=5
# More candidates = more diversity
```

---

### Issue 2: "Stress-test always returns GAPS"

**Symptoms**:
- /stress-test H-001-A → GAPS (7.5/10)
- /evolve H-001-A
- /stress-test H-001-A-v2 → GAPS (7.8/10)
- /evolve H-001-A-v2
- /stress-test H-001-A-v3 → GAPS (8.0/10)
- Never reaches PASS

**Diagnosis**:
```bash
# Check stress-test reports
# Are different gaps found each time?
# Or same gaps persisting?
```

**Solutions**:

1. **If different gaps each time → System is working**:
   - Each evolution fixes old gaps but reveals new ones
   - This is normal for complex hypotheses
   - Continue until PASS (typically 3-4 cycles)

2. **If same gaps persist → Evolution not effective**:
```bash
# Manual intervention needed
# Read gap description carefully
# Edit hypothesis file directly to address gap
# Then re-test

# Example: Gap says "identification strategy unclear"
# But /evolve keeps generating vague strategies
# You need to specify exact identification approach
```

3. **If approaching PASS but not quite (8.5-8.9) → Use focused evolution**:
```bash
/evolve H-001-A-v3 --focus=assumptions
# Target specific remaining issues
```

---

### Issue 3: "Evolution makes hypothesis worse"

**Symptoms**:
- H-001-A score: 8.0/10
- /evolve H-001-A
- H-001-A-v2 score: 7.5/10 (worse!)

**Diagnosis**:
```bash
# Read evolution log carefully
# What changed between versions?
# Was core insight preserved or lost?
```

**Solutions**:

1. **Check if evolution was too aggressive**:
```bash
# Use conservative mode
/evolve H-001-A --conservative
# Makes minimal changes only
```

2. **Review evolution log**:
```bash
# Look at "What Changed" section
# If core mechanism changed → Bad
# If only details refined → Good
```

3. **Revert to previous version**:
```bash
# Original H-001-A is preserved
# Can discard H-001-A-v2
# Try different evolution approach:
/evolve H-001-A --focus=specific-issue
```

---

### Issue 4: "Stress-test says hypothesis not novel, but I think it is"

**Symptoms**:
- /stress-test returns "Low novelty (3/10)"
- But you believe hypothesis is genuinely novel
- Cites papers that are different from your work

**Diagnosis**:
```bash
# Check literature comparison in stress-test output
# Does it cite the right papers?
# Or is it comparing to irrelevant work?
```

**Solutions**:

1. **Clarify what's novel in hypothesis**:
```bash
# Edit hypothesis file
# Add section: "Distinction from prior work"
# Explicitly state what's new

# Then re-test
/stress-test H-001-A
```

2. **Update DOMAIN.md with recent literature**:
```bash
# If field moves fast, agent may not know latest work
# Add recent papers to DOMAIN.md
# System will use this context
```

3. **Provide literature cutoff**:
```bash
# Only check against recent literature
/stress-test H-001-A --literature-cutoff=2023
```

4. **Accept if truly not novel**:
```bash
# Sometimes we think work is novel but it exists
# Stress-test caught this before wasting effort
# Pivot to different approach:
/hypothesize "Modified question focusing on genuinely new angle"
```

---

### Issue 5: "System is too slow"

**Symptoms**:
- /stress-test takes 10+ minutes
- Multiple commands needed for one hypothesis
- Research velocity decreases

**Diagnosis**:
```bash
# Where is time spent?
# Hypothesis generation? (1-2 min typical)
# Stress-testing? (5-10 min typical)
# Evolution? (2-3 min typical)
```

**Solutions**:

1. **Use --quick for screening**:
```bash
# Skips observation matching (fastest)
/stress-test H-001-A --quick
# 2-3 minutes instead of 10
```

2. **Generate fewer hypotheses**:
```bash
/hypothesize "Question" --count=3
# Instead of default 3-5
```

3. **Skip debate for routine work**:
```bash
# Don't use --debate unless high-stakes
/hypothesize "Question"  # Standard, not debate
```

4. **Batch similar operations**:
```bash
# If comparing hypotheses, collect all data first
# Then run stress-tests
# Avoid back-and-forth
```

---

### Issue 6: "I don't understand why stress-test failed"

**Symptoms**:
- /stress-test returns BLOCKED (5.5/10)
- Report is technical or unclear
- Don't know how to fix

**Diagnosis**:
```bash
# Read "Critical Gaps" section carefully
# Look for:
# - Impact (why it matters)
# - Fix (how to address)
```

**Solutions**:

1. **Try automatic fix first**:
```bash
/evolve H-001-A
# System may be able to fix automatically
```

2. **Check domain-specific issues**:
```bash
# Stats theory: Missing lower bound?
# Policy: Weak identification?
# Biomedical: Mechanism not plausible?
```

3. **Ask for clarification** (manual):
```bash
# Read hypothesis file + stress-test report together
# Identify specific sentences that are problematic
# Edit those sections
# Re-test
```

4. **Start fresh if fundamentally flawed**:
```bash
# If BLOCKED with score < 5.0
# And /evolve fails to salvage
# Better to start over:
/hypothesize "Revised question based on learnings"
```

---

## Best Practices

### Practice 1: Always Stress-Test Before Execution

**Why**: Issues found early save time later.

**Bad practice**:
```bash
/hypothesize "Question"
# → H-001-A looks good!
# → Start experiments immediately
# → 2 weeks later: realize assumption was wrong
```

**Good practice**:
```bash
/hypothesize "Question"
# → H-001-A

/stress-test H-001-A
# → GAPS: Assumption X unrealistic
# → Fix now (15 min) instead of after failed experiments

/evolve H-001-A
/stress-test H-001-A-v2
# → PASS

# → Now start experiments with confidence
```

**Time saved**: Days to weeks

---

### Practice 2: Expect 2-3 Iterations

**Why**: First hypothesis rarely perfect.

**Bad expectation**:
```bash
/hypothesize → /stress-test → Should be PASS
# If not PASS, something wrong with system
```

**Good expectation**:
```bash
/hypothesize → /stress-test → GAPS (normal)
    ↓
/evolve → /stress-test → GAPS (still normal)
    ↓
/evolve → /stress-test → PASS (achieved)
```

**Typical cycles**: 2-3 for routine work, 3-4 for complex

---

### Practice 3: Use Debate Mode for High-Stakes Work

**Why**: Extra upfront time yields better quality.

**When to use debate**:
- Grant proposals
- Major journal submissions
- Dissertation hypotheses
- High-risk experiments (expensive, time-consuming)

**When not needed**:
- Exploratory research
- Pilot studies
- Quick feasibility checks

**Time tradeoff**:
```bash
# Standard mode: 5-10 min → Good quality
/hypothesize "Question"

# Debate mode: 15-20 min → Higher quality
/hypothesize "Question" --debate
```

**ROI**: Extra 10 min upfront can save weeks of revision later.

---

### Practice 4: Keep Version History

**Why**: Can always compare or revert.

**System does this automatically**:
```bash
/hypothesize → H-001-A (original)
    ↓
/evolve → H-001-A-v2 (H-001-A preserved)
    ↓
/evolve → H-001-A-v3 (all previous preserved)
```

**Your responsibility**:
```bash
# Review evolution logs
hypotheses/evolution/H-001-A-evolution-log.md

# Compare versions if needed
# Original: hypotheses/proposals/H-001-A-*.md
# Evolved: hypotheses/proposals/H-001-A-v2-*.md
```

**Use cases**:
- Reviewer asks "why did you change X?" → Check log
- Evolved version feels wrong → Revert to earlier
- Multiple approaches viable → Keep all versions

---

### Practice 5: Trust the Verdict, But Verify

**Why**: System is helpful but not infallible.

**What "trust" means**:
- PASS → Hypothesis is ready by formal standards
- GAPS → Issues identified are real
- BLOCKED → Serious problems exist

**What "verify" means**:
- Read stress-test report yourself
- Check if gaps align with your domain knowledge
- Ensure novel aspects are truly novel
- Validate that fixes actually address issues

**Balance**:
```bash
# Don't blindly accept:
/stress-test H-001-A
# → PASS
# → Submit paper immediately without reading (BAD)

# Don't completely distrust:
/stress-test H-001-A
# → GAPS: Assumption X unrealistic
# → "System is wrong, assumption is fine" (RISKY)

# Good balance:
/stress-test H-001-A
# → GAPS: Assumption X unrealistic
# → Read gap description
# → Check if valid concern
# → If yes: /evolve
# → If no: Manual edit + re-test
```

---

### Practice 6: Update DOMAIN.md Regularly

**Why**: DOMAIN.md is how the system adapts to your field. It's the single most important configuration file.

**What to include in DOMAIN.md**:
1. **Core Theoretical Frameworks** - Foundations of your field
   - Stats: Minimax theory, empirical processes, information theory
   - Policy: Causal inference, institutional theory, policy diffusion
   - Your field: [Your frameworks]

2. **Evidence Quality Tiers** - What's strong evidence in your field
   - Tier 1 (Gold): Peer-reviewed proofs, RCTs, meta-analyses
   - Tier 2-4: Adapt to your field

3. **Ranking Dimensions** - How to evaluate work in your field
   - Universal: Novelty, importance, testability
   - Domain-specific: Add your criteria

4. **Verification Standards** - When hypothesis is "ready"
   - Stats: Lower bound + upper bound + assumptions
   - Policy: Identification + confounders + robustness
   - Your field: [Your standards]

5. **Common Pitfalls** - What goes wrong in your field
   - Stats: Missing lower bounds, unrealistic assumptions
   - Policy: Weak identification, selection bias
   - Your field: [Your pitfalls]

**Update frequency**:
- Starting new domain → Create DOMAIN.md first
- New major paper → Add to frameworks section
- Reviewer feedback → Add to pitfalls section
- Field consensus shifts → Update standards section

**Impact on system behavior**:
```bash
# Without DOMAIN.md:
/hypothesize → Generic, field-agnostic hypotheses
/stress-test → Generic quality checks
/evolve → Generic fix strategies

# With basic DOMAIN.md:
/hypothesize → Domain-appropriate theoretical angles
/stress-test → Domain-specific quality checks
/evolve → Domain-aware fix strategies

# With comprehensive, updated DOMAIN.md:
/hypothesize → Hypotheses meeting current field standards
/stress-test → Checks aligned with top journals
/evolve → Fixes addressing known field issues
```

**Template**: See `domains/stats-theory/DOMAIN.md` for comprehensive example.

---

### Practice 7: Document What Works for Your Research Type

**Why**: Build personal best practices over time.

**What to document**:
```bash
# Create: research/docs/my_workflows.md

## What I learned:

### For stats theory projects:
- Always use --debate for theorems
- Check lower bound first in stress-test
- Evolution typically needs 3 cycles
- Focus on assumption justification

### For policy projects:
- Include --observations flag in stress-test
- Identification is most common gap
- Manual data analysis helps evolution
- RDD/IV/DID frameworks work best

### Common issues I hit:
- Hypotheses too similar when question too vague
- Need --strict for top-tier journals
- Evolution struggles with mechanism design (do manually)
```

**Use this document** to refine your workflow over time.

---

## Advanced Usage

### Advanced Pattern 1: Custom Evolution Focus

**Scenario**: Stress-test finds multiple gaps, but you want to fix specific one first.

```bash
/stress-test H-001-A
# → Gap 1: Assumptions (priority)
# → Gap 2: Novelty claim (minor)
# → Gap 3: Observations (can address later)

# Fix assumptions only
/evolve H-001-A --focus=assumptions

# Re-test
/stress-test H-001-A-v2
# → Gap 1: Fixed ✓
# → Gap 2: Still present
# → Gap 3: Still present

# Now fix novelty
/evolve H-001-A-v2 --focus=novelty

# Iterative focused fixes
```

**When useful**:
- Complex hypotheses with many issues
- Want to verify each fix independently
- Avoid cascading changes

---

### Advanced Pattern 2: Multi-Hypothesis Tournament

**Scenario**: Need to select best from many candidates.

```bash
# Generate many candidates
/hypothesize "Question" --count=5
# → H-001-A, B, C, D, E

# Quick screen (parallel)
for h in A B C D E; do
  /stress-test H-001-$h --quick &
done
wait

# Results:
# A: 8.5/10
# B: 7.2/10
# C: 6.8/10
# D: 8.0/10
# E: 5.5/10

# Full test on top 2
/stress-test H-001-A  # → 8.2/10 GAPS
/stress-test H-001-D  # → 7.8/10 GAPS

# Evolve both
/evolve H-001-A  # → H-001-A-v2
/evolve H-001-D  # → H-001-D-v2

# Final comparison
/stress-test H-001-A-v2  # → 9.3/10 PASS
/stress-test H-001-D-v2  # → 8.7/10 GAPS

# Winner: H-001-A-v2
```

**Timeline**: 1-2 days for comprehensive comparison

---

### Advanced Pattern 3: Incremental Hypothesis Building

**Scenario**: Building complex hypothesis in stages.

```bash
# Stage 1: Core mechanism
/hypothesize "Core mechanism question"
# → H-001-A (basic version)

/stress-test H-001-A
# → PASS

# Stage 2: Add complexity
[Manually edit H-001-A to add detail]

/stress-test H-001-A
# → GAPS (new details broke something)

/evolve H-001-A
# → H-001-A-v2 (complex version fixed)

# Stage 3: Add empirical predictions
[Edit H-001-A-v2 to add predictions]

/stress-test H-001-A-v2
# → PASS (comprehensive hypothesis)
```

**When useful**:
- Very complex hypotheses
- Interdisciplinary work
- Novel theoretical frameworks

---

### Advanced Pattern 4: Cross-Validation with External Tools

**Scenario**: Combine system with other validation.

```bash
# System validation
/stress-test H-001-A
# → 8.5/10 GAPS

# External validation (you do this):
# - Run pilot experiment
# - Check with domain expert
# - Manual literature review

# If external validation finds issues:
# Create review document
[Write: external_review.md]

# Evolve based on combined feedback
/evolve H-001-A --review external_review.md

# Re-validate
/stress-test H-001-A-v2
# → PASS
```

**Benefits**:
- Catches issues system might miss
- Builds confidence
- Combines AI + human judgment

---

### Advanced Pattern 5: Domain Transfer

**Scenario**: Adapt hypothesis from one domain to another.

```bash
# Original hypothesis in Domain A
# H-001-A: Stats theory hypothesis

# Want to apply insight to Domain B (machine learning)

# Update DOMAIN.md to Domain B standards
[Edit DOMAIN.md to ML context]

# Stress-test with new domain
/stress-test H-001-A
# → GAPS (doesn't meet ML standards)

# Evolve for new domain
/evolve H-001-A
# → H-001-A-v2 (adapted to ML)

/stress-test H-001-A-v2
# → PASS in ML domain
```

**When useful**:
- Translational research
- Cross-disciplinary work
- Applying theory to practice

---

## Conclusion

You now have complete knowledge of the AI Co-Scientist system. Key takeaways:

**The Three Commands**:
- /hypothesize - Generate hypotheses
- /stress-test - Verify quality
- /evolve - Fix issues

**Core Workflow**:
```
hypothesize → stress-test → evolve → stress-test → PASS → Execute
```

**Remember**:
1. Always stress-test before execution
2. Expect 2-3 iterations
3. Trust the system but verify outputs
4. Document what works for your domain
5. Update DOMAIN.md regularly

**Next Steps**:
1. Start with a real research question
2. Run through complete workflow once
3. Note what works and what needs adjustment
4. Build your personal best practices
5. Share learnings with research group

**Getting Help**:
- Stuck on hypothesis generation? Check "Domain-Specific Guides"
- Stress-test confusing? See "Troubleshooting" section
- Want advanced workflows? Review "Advanced Usage"
- System not working as expected? Review "Best Practices"

**System Files**:
- Commands: /Users/andyhou/research/commands/*.md (domain-agnostic)
- Your hypotheses: /Users/andyhou/research/hypotheses/
- Domain configuration: /Users/andyhou/research/domains/[your-field]/DOMAIN.md (domain-specific)

**Domain Configuration Examples**:
- Statistical Theory: /Users/andyhou/research/domains/stats-theory/DOMAIN.md
- Policy Research: /Users/andyhou/research/domains/policy-research/DOMAIN.md
- Biomedical: /Users/andyhou/research/domains/biomedical/DOMAIN.md

---

## Key Insight: Domain-Agnostic Design

This system implements **universal research workflows** that adapt to any field:

**Universal (same across all domains)**:
- Commands: /hypothesize, /stress-test, /evolve
- Workflow: generate → verify → fix → verify → PASS
- Quality gates: PASS (9+), GAPS (6-9), BLOCKED (<6)

**Domain-specific (configured in DOMAIN.md)**:
- Theoretical frameworks (minimax vs causal inference vs molecular pathways)
- Evidence tiers (proofs vs RCTs vs pathway validation)
- Verification standards (lower bounds vs identification vs mechanism)
- Common pitfalls (missing assumptions vs selection bias vs safety)

**Result**: Same commands work for proving theorems, designing policies, or discovering drugs. Only DOMAIN.md differs.

---

*This manual documents the AI Co-Scientist system implementing workflows from Google's research paper, adapted as a domain-agnostic framework for scientific research across all fields.*

*Last Updated: 2026-01-27*
*Version: 2.0 - Domain-Agnostic Edition*

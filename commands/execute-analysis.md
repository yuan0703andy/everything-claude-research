---
name: execute-analysis
description: Execute analysis and document process (corresponds to GSD execute-phase). Domain-aware execution with field-specific validation.
argument-hint: <hypothesis-id>
allowed-tools: Read, Bash, Glob, Grep, Write, Task
---

<execute_analysis_command>

# /execute-analysis Command

Execute research analysis with complete process documentation.

## Purpose

Execute analysis under a clear plan:
- Follow pre-defined analysis plan
- Record all steps and decisions
- Preserve intermediate results
- Generate phase summary
- **Apply domain-specific execution standards**

## Prerequisites

- [ ] Completed `/review-hypothesis` (with clear verification plan)
- [ ] Data prepared
- [ ] Analysis scripts planned
- [ ] Verification standards defined (`/eval define`)
- [ ] Domain-specific requirements understood

## Workflow

```
User: /execute-analysis H-001
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Load Context                   │
    │  - Read hypothesis verification plan    │
    │  - Load DOMAIN.md for domain standards  │
    │  - Check current phase in STATE.md      │
    │  - Verify prerequisites met              │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Spawn Experimentalist          │
    │  - Inject domain knowledge              │
    │  - Provide verification plan            │
    │  - Execute analysis with domain checks  │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Document Execution             │
    │  - Record all steps                     │
    │  - Save results and artifacts           │
    │  - Generate phase-N-EXECUTE.md          │
    │  - Note domain-specific outcomes        │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 4: Update State                   │
    │  - Update STATE.md                      │
    │  - Update hypothesis file               │
    │  - Prepare for verification             │
    └─────────────────────────────────────────┘
```

---

## Execution Steps

### Step 1: Context Loading (Automatic Execution)

**CRITICAL**: Before executing any analysis, load full context including domain knowledge.

```markdown
# STEP 1A: Identify hypothesis and domain
Read hypothesis file: hypotheses/proposals/H-XXX-*.md
Extract domain field from hypothesis frontmatter
Extract verification plan from hypothesis

# STEP 1B: Load domain knowledge (MUST READ FULL FILE)
Read /Users/andyhou/research/domains/{domain}/DOMAIN.md

This file contains (600+ lines):
- Core theoretical frameworks
- Verification standards
- Domain-specific success criteria
- Common pitfalls
- Execution best practices

# STEP 1C: Load project context
Read PROJECT.md for project constraints
Read STATE.md for current status

# STEP 1D: Verify prerequisites
Check that:
- Hypothesis has been reviewed and approved
- Verification plan is complete
- Data/materials are ready
- Domain-specific prerequisites met
```

**Output**: Context package containing:
- Complete hypothesis with verification plan
- Full DOMAIN.md content (600+ lines)
- Project constraints
- Current project state
- Domain-specific execution requirements

### Step 2: Execute Analysis with Domain Awareness

**Spawn Experimentalist** with domain knowledge injection:

```markdown
<task>
## Domain Knowledge (AUTO-INJECTED - FULL CONTENT)

[INJECT COMPLETE DOMAIN.md FILE HERE - ALL 600+ LINES]

This includes:
- Verification standards (e.g., what makes a valid proof for stats, what constitutes credible identification for policy)
- Common pitfalls in the domain
- Best practices for execution
- Quality standards

## Hypothesis to Execute

**Hypothesis ID**: H-XXX
**Domain**: [stats-theory | policy-making]
**Verification Plan**: [From hypothesis file]

[Complete hypothesis proposal from hypotheses/proposals/H-XXX.md]

## Project Context

[From PROJECT.md - constraints, timeline, resources]

## Request

Execute the analysis according to the verification plan, applying domain-specific standards.

### For Statistical Theory Projects:

**Execution Requirements**:
1. **Proof Implementation**:
   - Implement estimator exactly as theorem specifies
   - Code all proof steps that claim "constructive"
   - Document all assumptions in code comments

2. **Simulation Design**:
   - Test parameter regimes specified in theorem
   - Include regimes where assumptions break down
   - Verify theoretical rate empirically
   - Check constants in rates (not just asymptotic order)

3. **Numerical Validation**:
   - Convergence checks for iterative algorithms
   - Stability analysis for numerical procedures
   - Compare against theoretical predictions
   - Verify lower bound tightness

4. **Domain-Specific Checks**:
   - Does simulation match theorem statement?
   - Are all assumptions tested empirically?
   - Does empirical rate match theoretical rate?
   - Is computational complexity as claimed?

### For Policy Research Projects:

**Execution Requirements**:
1. **Data Validation**:
   - Verify data matches identification requirements
   - Check for manipulation at cutoff (if RDD)
   - Validate measurement of key constructs
   - Confirm sample has required variation

2. **Identification Strategy Execution**:
   - Implement identification as specified
   - Test identifying assumptions
   - Run robustness checks for threats to validity
   - Document deviations from plan

3. **Mechanism Testing**:
   - Test observable implications of mechanism
   - Check intermediate steps (X → M → Y)
   - Rule out alternative mechanisms
   - Validate measurement of mediator

4. **Domain-Specific Checks**:
   - Is identification strategy correctly implemented?
   - Are robustness checks comprehensive?
   - Is mechanism evidence convincing?
   - Are scope conditions tested?

**Output Format**: execution_report following standard structure (see below)
</task>
```

#### 2.1 Data Validation (Domain-Specific)

```r
# General Checks
- Sample size meets requirements?
- No unexpected missing values?
- Variable distributions reasonable?
- Assumptions satisfied?

# Stats Theory Checks
- Parameter regimes cover theorem scope?
- Boundary cases included?
- Assumptions testable in simulation?

# Policy Research Checks
- Identifying variation present?
- No evidence of manipulation?
- Constructs measured validly?
- Confounders available in data?
```

#### 2.2 Main Analysis

```
- Execute pre-specified analysis
- Record all parameter choices
- Save intermediate results
- Generate figures
- **Apply domain-specific validation**
```

#### 2.3 Robustness Checks (Domain-Aware)

```
General:
- Alternative specifications
- Subsample analysis
- Sensitivity analysis

Stats Theory:
- Different starting values (if iterative)
- Different parameter regimes
- Boundary case testing
- Compare to naive methods

Policy Research:
- Alternative identifying assumptions
- Different confounder sets
- Subsample by scope conditions
- Placebo tests
```

### Step 3: Document Process

Lab Manager generates `.planning/phases/phase-N-EXECUTE.md`:

```markdown
# Phase N - Execute: H-XXX Analysis

**Start Time**: [YYYY-MM-DD HH:MM]
**End Time**: [YYYY-MM-DD HH:MM]
**Executor**: Experimentalist
**Domain**: [stats-theory | policy-making]

## Analysis Goal
[What this analysis aims to verify]

## Domain Context
**Domain Standards Applied**: [Annals of Statistics | APSR | etc.]
**Domain-Specific Requirements**: [List key requirements from DOMAIN.md]

## Data Validation

### General Checks
- [x] Sample size: N = 1000 (meets requirements)
- [x] Missing values: 0.5% (acceptable)
- [x] Variable distributions: Normal

### Domain-Specific Checks

**For Stats Theory**:
- [x] Parameter regimes: Covers n=100 to n=10000, sparsity s=1 to s=sqrt(n)
- [x] Assumptions: Gaussian noise, sub-Gaussian covariates verified
- [x] Boundary cases: Included s=1 and s=sqrt(n)

**For Policy Research**:
- [x] Identifying variation: Policy threshold at 50, observations on both sides
- [x] McCrary test: No evidence of manipulation (p=0.45)
- [x] Balance tests: Covariates balanced at threshold
- [x] Mechanism measurement: Validated survey instrument

### Identified Issues
- [Issue description]
- Resolution: [How it was handled]

## Execution Steps

### 1. Data Preparation
- [x] Load raw data
- [x] Clean and transform
- [x] Generate analysis dataset

### 2. Main Analysis
- [x] Descriptive statistics
- [x] Main regression/estimation
- [x] Generate tables and figures

### 3. Robustness Checks
- [x] Alternative specification 1: [Result]
- [x] Alternative specification 2: [Result]
- [x] Subsample analysis: [Result]

### 4. Domain-Specific Validation

**For Stats Theory**:
- [x] Empirical rate: n^(-0.4) matches theoretical n^(-2/5) ✓
- [x] Lower bound gap: 5% (tight)
- [x] Computational complexity: O(n log n) as claimed ✓
- [x] Assumptions: Verified in all tested regimes ✓

**For Policy Research**:
- [x] Identification: RDD estimates stable around threshold ✓
- [x] Robustness: Effect persists with alternative bandwidths ✓
- [x] Mechanism: Mediator analysis supports X → M → Y ✓
- [x] Placebo tests: No effect at false thresholds ✓

## Preliminary Results

### Main Findings
1. [Finding 1]: [Description and statistics]
2. [Finding 2]: [Description and statistics]

### Comparison to Predictions
- ✅ Consistent with hypothesis predictions
- ⚠️ Effect size smaller than expected
- ❌ One prediction not supported

### Domain-Specific Assessment

**Stats Theory**:
- **Minimax optimality**: Empirical rate matches theoretical (within constants)
- **Lower bound**: Gap ~5%, likely due to finite sample
- **Proof validation**: All constructive steps implemented and verified

**Policy Research**:
- **Causal identification**: RDD assumptions hold
- **Mechanism**: Evidence supports proposed pathway
- **External validity**: Effects heterogeneous as predicted by theory

### Robustness
- Results stable across different specifications
- Subsample analysis consistent
- Domain-specific robustness checks pass

## Generated Files

- `analysis/01_main_analysis.R` - Main analysis script
- `results/main_results.csv` - Main results
- `results/robustness_results.csv` - Robustness results
- `results/domain_checks.csv` - Domain-specific validation
- `figures/fig1_main_effect.png` - Main effect figure
- `figures/fig2_robustness.png` - Robustness figure

## Issues Encountered

### Issue 1: [Description]
- **Severity**: Low/Medium/High
- **Resolution**: [How it was resolved]
- **Impact**: [Effect on results]
- **Domain implication**: [If relevant]

## Code Quality

- [x] Random seed set
- [x] Code commented
- [x] Results reproducible
- [x] No hard-coded values
- [x] Domain assumptions documented in code

## Domain Compliance Check

### Stats Theory (if applicable)
| Criterion | Status | Notes |
|-----------|--------|-------|
| Estimator matches theorem | ✓ | Implemented exactly as specified |
| Simulation covers regimes | ✓ | n=100 to n=10000, s=1 to sqrt(n) |
| Rate empirically verified | ✓ | n^(-0.4) observed, n^(-2/5) predicted |
| Lower bound gap small | ✓ | ~5% gap, likely finite-sample |
| Assumptions tested | ✓ | Verified in all regimes |

### Policy Research (if applicable)
| Criterion | Status | Notes |
|-----------|--------|-------|
| Identification strategy implemented | ✓ | RDD at threshold=50 |
| Assumptions testable | ✓ | McCrary, balance tests pass |
| Robustness comprehensive | ✓ | Bandwidth, placebo tests |
| Mechanism tested | ✓ | Mediator analysis supports theory |
| Scope conditions checked | ✓ | Heterogeneity as predicted |

## Next Steps

- [ ] Proceed to `/verify-results` phase
- [ ] Additional robustness checks needed? [If yes, specify]
- [ ] Hypothesis revision needed? [If yes, why]
- [ ] Domain concerns to address? [If any]

## Notes

[Other important information, especially domain-specific observations]
```

### Step 4: Update State

Automatic execution:
```
- Update STATE.md (record completed analysis)
- Update hypotheses/H-XXX.md (record execution status)
- Create phase-N-EXECUTE.md in .planning/phases/
- Flag domain compliance status
```

---

## Output

1. **Phase Execution Record**: `.planning/phases/phase-N-EXECUTE.md`
2. **Analysis Results**: Files in `results/` directory
3. **Figures**: Files in `figures/` directory
4. **Domain Validation**: Domain-specific checks documented
5. **Updated STATE.md**

---

## Success Criteria (Domain-Aware)

### General Criteria
- [ ] All planned analyses executed
- [ ] Results recorded completely
- [ ] Generated files saved
- [ ] Issues and resolutions documented
- [ ] Code reproducible

### Domain-Specific Criteria

**Stats Theory**:
- [ ] Estimator implements theorem exactly
- [ ] Simulation covers all required parameter regimes
- [ ] Empirical rate matches theoretical rate
- [ ] Lower bound gap quantified
- [ ] Assumptions verified empirically
- [ ] Computational complexity confirmed

**Policy Research**:
- [ ] Identification strategy correctly implemented
- [ ] Identifying assumptions tested
- [ ] Robustness checks comprehensive
- [ ] Mechanism evidence collected
- [ ] Scope conditions tested
- [ ] Threats to validity addressed

---

## Integration with Other Commands

```
/review-hypothesis        Define verification plan
        ↓
/eval define H-XXX       Set success criteria
        ↓
/execute-analysis        ← Current step (with domain awareness)
        ↓
/verify-results          Verify analysis results (domain-aware)
        ↓
/update-state            Update research state
```

---

## Usage Example

```bash
# Basic usage
/execute-analysis H-001

# System automatically:
# 1. Identifies domain from H-001 metadata (e.g., stats-theory)
# 2. Loads /Users/andyhou/research/domains/stats-theory/DOMAIN.md
# 3. Injects domain knowledge into Experimentalist context
# 4. Executes analysis with domain-specific validation
# 5. Documents domain compliance in execution record

# User sees progress:
"Loading H-001 verification plan..."
"Domain: stats-theory"
"Loading domain standards from DOMAIN.md (640 lines)..."
"Spawning Experimentalist with domain knowledge..."
"Executing analysis with minimax optimality checks..."
"Simulation covering n=100 to n=10000..."
"Empirical rate: n^(-0.4), theoretical: n^(-2/5) ✓"
"Lower bound gap: 5% (acceptable) ✓"
"Domain compliance: PASS"
"Documentation saved to .planning/phases/phase-3-EXECUTE.md"
```

---

## Domain-Specific Best Practices

### For Stats Theory Execution

**Simulation Design**:
- Test at least 5 different sample sizes (n)
- Cover full range of sparsity (s)
- Include boundary cases where assumptions barely hold
- Run enough Monte Carlo reps (≥1000) for stable estimates

**Rate Verification**:
- Plot log(error) vs log(n) to verify rate visually
- Estimate slope via OLS, compare to theoretical rate
- Report confidence interval for empirical rate
- Quantify gap to lower bound

**Assumptions Testing**:
- For each assumption, design test showing when it breaks
- Report simulation performance when assumptions violated
- This validates that assumptions are not vacuous

### For Policy Research Execution

**RDD Implementation**:
- Run McCrary density test at threshold
- Check balance of covariates at threshold
- Vary bandwidth (0.5x to 2x optimal)
- Placebo tests at false thresholds
- Report results visually (RDD plot)

**Mechanism Testing**:
- Estimate X → M and M → Y separately
- Mediation analysis (Baron & Kenny or modern methods)
- Collect direct mechanism evidence when possible
- Check mechanism operates as theorized

**Robustness**:
- Alternative identifying assumptions (if possible)
- Different confounder specifications
- Subsample by scope conditions
- Time placebo (if panel data)

---

## Important Notes

⚠️ **Follow the plan**
- Don't deviate from pre-specified analysis without documentation
- If changes needed, document reason and get approval

⚠️ **Domain standards are requirements, not suggestions**
- A stats paper without rate verification is incomplete
- A policy paper without identification tests is incomplete
- These are not "nice to have" - they are publication requirements

⚠️ **Document everything**
- Future you will thank present you
- Reviewers will ask for details
- Reproducibility requires complete documentation

⚠️ **Quality over speed**
- Take time to verify domain standards are met
- Rushing through execution creates more work later
- Most revisions are due to insufficient validation

⚠️ **When in doubt, ask Methodologist**
- Domain standards can be subtle
- Better to clarify before executing than to redo later

</execute_analysis_command>

---
name: meta-review
description: Portfolio-level hypothesis analysis to detect collective biases, research blind spots, and systemic risks across multiple hypotheses
argument-hint: [optional: hypothesis-pattern like "H-00*"]
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

# /meta-review - Research Portfolio Health Check

Analyze your collection of hypotheses to identify systemic issues invisible at the individual level.

## Purpose

While individual commands (`/stress-test`, `/review-hypothesis`) verify **single hypotheses**, `/meta-review` examines the **entire portfolio** to detect:

- **Collective biases**: Over-reliance on specific mechanisms or assumptions
- **Research blind spots**: Unexplored theoretical angles
- **Groupthink risks**: Consensus without contradictory perspectives
- **Portfolio imbalance**: Too risky or too conservative

**Key insight**: A set of individually excellent hypotheses can still form a fragile portfolio if they share hidden weaknesses.

---

## Usage

### Basic Usage (Scan All Hypotheses)

```bash
/meta-review
```

Analyzes all active hypotheses in `hypotheses/` directory.

### Targeted Analysis

```bash
/meta-review H-001*     # Only H-001 variants (A, B, C, ...)
/meta-review H-00[1-3]  # Hypotheses H-001, H-002, H-003
```

Useful when you have multiple hypothesis families and want to check specific subsets.

---

## What Happens Internally

```mermaid
User: /meta-review
  ↓
1. Scan Hypothesis Directory
   - Find all hypothesis files
   - Filter out DISPROVED (already eliminated)
   - Load hypothesis content
  ↓
2. Load Domain Configuration
   - Extract meta-review keywords from DOMAIN.md
   - Get theoretical angles to check coverage
   - Load common mechanism/assumption lists
  ↓
3. Spawn meta-reviewer Agent
   - Run 4 bias analyses:
     a) Mechanism concentration
     b) Assumption clustering
     c) Testability balance
     d) Coverage gaps
   - Perform antithesis mining if consensus detected (>80%)
   - Generate portfolio health summary
  ↓
4. Display Report
   - Critical/high/medium issues with priorities
   - Specific recommendations with commands
   - Portfolio strengths
   - Action plan
```

**You see**: Portfolio health report with actionable recommendations
**You don't see**: Internal keyword extraction, statistical analysis

---

## Output Format

### Example Output

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 META-REVIEW ► Research Direction Analysis
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Analyzed: 5 active hypotheses (H-001-A through H-001-E)
Overall Portfolio Health: ⚠️ CONCERNING

## 🚩 Critical Issues

### Issue 1: Universal Assumption Dependency
**Pattern**: All 5 hypotheses assume sub-Gaussian errors (100%)
**Risk**: Single point of failure - if assumption fails, entire portfolio collapses

**Impact**: CRITICAL 🔴

**Recommendation**:
Generate robustness hypothesis relaxing sub-Gaussian requirement.

```bash
/hypothesize "Derive convergence rate under heavy-tailed errors"
```

─────────────────────────────────────────────────────────────

## 🔴 High-Priority Issues

### Issue 2: Mechanism Over-Concentration
**Pattern**: 4/5 hypotheses (80%) rely on same theoretical approach
**Risk**: Mechanistic monoculture - lacks diversity

**Details**:
- Minimax rate approach: 4/5 hypotheses
- Computational approach: 1/5 hypotheses
- Bayesian approach: 0/5 hypotheses ⚠️

**Recommendation**:
Diversify by exploring Bayesian or computational angle.

```bash
/hypothesize "Computational barriers to achieving minimax rate"
```

### Issue 3: Antithesis Missing 🆕
**Pattern**: Consensus detected - 4/5 hypotheses predict same direction
**Risk**: Groupthink - no contradictory perspective to test assumptions

**Consensus**: "Adaptive thresholding improves convergence rate"

**Generated Antithesis**: H-001-ANTI
**Claim**: "Adaptive methods introduce instability, fixed threshold optimal"
**Plausibility**: MEDIUM (has theoretical support)

**Recommendation**:
Test antithesis to validate consensus before resource commitment.

```bash
/stress-test H-001-ANTI
```

─────────────────────────────────────────────────────────────

## 🟡 Medium-Priority Issues

### Issue 4: Testability Imbalance
**Distribution**:
- High testability (≥8): 1/5 (20%)
- Medium testability: 1/5 (20%)
- Low testability (≤4): 3/5 (60%) 🚩

**Assessment**: Portfolio too ambitious

**Recommendation**:
Add near-term testable hypothesis for steady progress.

```bash
/hypothesize "Prove rate under standard assumptions with existing tools"
```

─────────────────────────────────────────────────────────────

## ✅ Portfolio Strengths

### Strength 1: No DISPROVED Hypotheses
All active hypotheses passed initial verification ✓

### Strength 2: Good Novelty Balance
- High novelty: 3/5 (60%)
- Medium novelty: 2/5 (40%)
Healthy risk/reward distribution ✓

─────────────────────────────────────────────────────────────

## 📊 Recommended Action Plan

**Priority 1: Eliminate Single Point of Failure** 🔴 CRITICAL
```bash
/hypothesize "Heavy-tailed robust version"
```
Impact: Portfolio survives if sub-Gaussian assumption fails

**Priority 2: Test Antithesis** 🔴 HIGH
```bash
/stress-test H-001-ANTI
```
Impact: Validates consensus or reveals groupthink

**Priority 3: Diversify Mechanisms** 🔴 HIGH
```bash
/hypothesize "Computational approach to same problem"
```
Impact: Reduces mechanism concentration from 80% to 60%

**Priority 4: Add Testable Hypothesis** 🟡 MEDIUM
Impact: Ensures near-term progress

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Next**: Execute Priority 1 before committing resources to current portfolio.
```

---

## When to Use `/meta-review`

### ✅ Use `/meta-review` When:

1. **After generating multiple hypotheses**
   - Have 3+ hypotheses via `/hypothesize`
   - Want to ensure diversity before proceeding
   - Need portfolio health check

2. **Before major resource commitment**
   - About to start experiments on multiple hypotheses
   - Allocating team members to different tracks
   - Want to avoid "all eggs in one basket"

3. **Periodically during long projects**
   - Monthly or quarterly review
   - Prevent gradual drift toward groupthink
   - Ensure continued portfolio balance

4. **When feeling "stuck" or uncertain**
   - All hypotheses seem similar
   - Unsure which theoretical angle to explore next
   - Need strategic direction

### ❌ Don't Use `/meta-review` When:

1. **Only 1-2 hypotheses exist** → Not enough for portfolio analysis
2. **Need individual quality check** → Use `/stress-test` instead
3. **Need pairwise comparison** → Use `/rank` instead
4. **Just started project** → Generate hypotheses first

---

## Meta-Review vs Other Commands

| Command | Focus | Question Answered |
|---------|-------|-------------------|
| `/stress-test` | Single hypothesis quality | "Is H-001 scientifically sound?" |
| `/rank` | Pairwise comparison | "Is H-001 better than H-002?" |
| `/meta-review` | Portfolio health | "Is our COLLECTION of hypotheses wise?" |
| `/lab-meeting` | Team coordination | "What should we do this week?" |

**Mental model**:
- `/stress-test` = Individual health checkup
- `/rank` = Head-to-head competition
- `/meta-review` = Population health study
- `/lab-meeting` = Weekly team meeting

---

## Key Features

### 1. Collective Bias Detection

Identifies patterns invisible when reviewing hypotheses individually:

```yaml
Individual review:
  H-001: Novel ✓
  H-002: Novel ✓
  H-003: Novel ✓

Meta-review:
  All three use same mechanism ⚠️
  If mechanism is wrong, all fail simultaneously
```

### 2. Assumption Clustering Analysis

Finds shared assumptions creating single points of failure:

```yaml
Universal assumptions (100% of hypotheses):
  - Sub-Gaussian errors
  → If fails: Entire portfolio invalid

Majority assumptions (>75%):
  - Sparsity s = O(n^α)
  → High-risk concentration
```

### 3. Antithesis Mining 🆕

**Unique feature**: Generates scientifically plausible **opposite hypothesis** to:
- Break groupthink
- Test hidden assumptions
- Identify critical distinguishing experiments

**Example**:

**Consensus** (4/5 hypotheses): "Method X improves performance"

**Generated Antithesis**: "Method X introduces instability, hurts performance"

**Purpose**: Force explicit defense of consensus. If antithesis passes stress-test, consensus may be wrong.

### 4. Coverage Gap Identification

Maps theoretical space to find unexplored angles:

```yaml
Covered:
  - Information-theoretic bounds ✓
  - Robustness analysis ✓

Gaps:
  - Computational complexity ✗
  - Bayesian approaches ✗
  → Opportunities for new hypotheses
```

### 5. Testability Balance Assessment

Prevents portfolio from being too risky or too conservative:

```yaml
Current distribution:
  High testability: 1/5 (20%)
  Low testability: 3/5 (60%)

Assessment: TOO RISKY
Recommendation: Add near-term testable hypotheses
```

---

## Domain Adaptation

Meta-review automatically adapts to your field via `DOMAIN.md`:

### Example: Stats Theory Domain

```yaml
# domains/stats-theory/DOMAIN.md
meta_review_keywords:
  common_mechanisms:
    - "Fano's method"
    - "Minimax theory"
    - "Sub-Gaussian assumption"

  common_assumptions:
    - "Sparsity"
    - "Incoherence"
    - "Random design"

  theoretical_angles:
    - "Information-theoretic bounds"
    - "Computational complexity"
    - "Robustness analysis"
    - "Adaptive/sequential methods"
```

Meta-review will:
- Check for over-use of "Fano's method"
- Detect universal "sparsity" assumption
- Identify gaps in "computational complexity" angle

### Example: Biomedical Domain

```yaml
# domains/biomedical/DOMAIN.md
meta_review_keywords:
  common_mechanisms:
    - "ER stress pathway"
    - "Mitochondrial dysfunction"
    - "Autophagy"

  theoretical_angles:
    - "Molecular mechanism"
    - "Systems biology"
    - "Clinical translation"
```

Same `/meta-review` command, different domain-specific patterns detected.

---

## Integration with Other Commands

### Complete Research Workflow

```bash
# Week 1: Generate diverse hypotheses
/hypothesize "Research goal A"
→ H-001-A, H-001-B, H-001-C

# Week 2: Portfolio health check
/meta-review
→ Detects: All use mechanism X (80% concentration)
→ Recommends: Generate hypothesis using mechanism Y

# Week 3: Diversify portfolio
/hypothesize "Research goal A using mechanism Y"
→ H-001-D

# Week 4: Re-check portfolio
/meta-review
→ Portfolio now balanced (50% mechanism X, 50% mechanism Y)

# Week 5: Verify quality
/stress-test H-001-A  → PASS
/stress-test H-001-D  → PASS

# Week 6: Prioritize
/rank H-001-A H-001-D
→ Winner: H-001-A

# Week 7: Execute
/execute-analysis H-001-A
```

### Auto-Triggered in `/lab-meeting`

`/lab-meeting` automatically runs meta-review when ≥3 hypotheses exist:

```bash
/lab-meeting
→ Progress review
→ Elo tournament (/rank)
→ Meta-review (if ≥3 hypotheses) ← Automatic
→ Next week planning
```

---

## Advanced Usage

### Focused Meta-Review

Analyze specific hypothesis subset:

```bash
# Review only minimax-rate hypotheses
/meta-review H-001*

# Review first three hypothesis families
/meta-review H-00[1-3]
```

### Periodic Health Checks

Establish regular cadence:

```
Month 1: /hypothesize → /meta-review → Diversify
Month 2: /hypothesize → /meta-review → Check balance
Month 3: /hypothesize → /meta-review → Final portfolio
```

Prevents gradual drift toward groupthink over time.

---

## Troubleshooting

### Warning: "Insufficient hypotheses for meta-review"

**Cause**: <3 hypotheses in directory

**Action**: Generate more hypotheses first
```bash
/hypothesize "Research goal A"
/hypothesize "Research goal B"
# Then:
/meta-review
```

### Info: "No critical issues detected"

**Meaning**: Portfolio is healthy (rare but possible)

**Output will show**:
```markdown
Overall Portfolio Health: ✅ HEALTHY

No critical or high-priority issues detected.

Portfolio shows:
- Mechanism diversity ✓
- Assumption diversity ✓
- Balanced testability ✓
- Good coverage ✓

Recommendation: Proceed with current portfolio.
```

### Warning: "Antithesis generation failed"

**Cause**: No clear consensus detected (<80% agreement)

**Meaning**: Portfolio already has diversity of perspectives

**Action**: No antithesis needed

---

## FAQs

### Q: How many hypotheses do I need for meaningful meta-review?

**A**: Minimum 3, optimal 5-7.

- **1-2 hypotheses**: Not enough for pattern detection
- **3-4 hypotheses**: Basic analysis possible
- **5-7 hypotheses**: Ideal for robust pattern detection
- **8+ hypotheses**: Consider splitting into subgroups

### Q: How often should I run meta-review?

**A**: Recommended frequency:

- **After major hypothesis generation** (3+ new hypotheses)
- **Monthly** for ongoing projects
- **Before resource allocation** (team assignments, experiments)
- **When feeling "stuck"** (all ideas seem similar)

**Don't overuse**: Like any health check, too frequent is noise.

### Q: What if meta-review says "all hypotheses use same assumption"?

**A**: This is often **correct** and **necessary**.

Example:
```
All hypotheses assume: "Data comes from controlled experiment"
→ This is fine if your project IS about controlled experiments
```

**Key question**: Is the shared assumption **essential** (part of problem definition) or **accidental** (unexamined habit)?

Meta-review flags the pattern. You decide if it's a problem.

### Q: Does meta-review modify hypothesis files?

**A**: **No**, meta-review is **read-only**.

It may **generate** new files:
- `H-XXX-ANTI` (antithesis hypothesis)
- Updated `STATE.md` (portfolio health record)

But original hypotheses remain unchanged.

### Q: Can meta-review detect if a hypothesis is WRONG?

**A**: **No**. That's `/stress-test`'s job.

**Meta-review detects**:
- Portfolio-level risks (collective bias)
- Strategic gaps (unexplored angles)
- Balance issues (too risky/conservative)

**Stress-test detects**:
- Individual hypothesis quality
- Contradictions with evidence
- Logical flaws

Use both, different purposes.

---

## Implementation Notes (for Developers)

### Internal Workflow

```typescript
async function executeMetaReview(pattern?: string) {
  // Step 1: Find hypotheses
  const pattern_glob = pattern || "H-*"
  const hypothesis_files = await glob(`hypotheses/${pattern_glob}.md`)

  if (hypothesis_files.length < 3) {
    console.warn("Insufficient hypotheses for meta-review (need ≥3)")
    return
  }

  // Step 2: Filter out DISPROVED
  const active_hypotheses = []
  for (const file of hypothesis_files) {
    const status = await getHypothesisStatus(file)
    if (status !== 'DISPROVED') {
      active_hypotheses.push(file)
    }
  }

  // Step 3: Load domain config
  const domainConfig = await readDomainConfig()
  const meta_keywords = domainConfig.meta_review_keywords

  // Step 4: Extract structured data
  const hypothesis_data = await Promise.all(
    active_hypotheses.map(async (file) => ({
      id: extractHypothesisID(file),
      content: await readFile(file),
      mechanism: extractMechanism(await readFile(file)),
      assumptions: extractAssumptions(await readFile(file)),
      scores: await getStressTestScores(file)
    }))
  )

  // Step 5: Spawn meta-reviewer
  const metaReviewResult = await spawnAgent({
    type: 'meta-reviewer',
    prompt: `
      Analyze this hypothesis portfolio for systemic issues:

      Domain keywords: ${JSON.stringify(meta_keywords)}
      Hypotheses (${hypothesis_data.length}):
      ${JSON.stringify(hypothesis_data, null, 2)}

      Provide full portfolio analysis with:
      - Collective bias detection
      - Assumption clustering
      - Testability balance
      - Coverage gaps
      - Antithesis mining (if consensus >80%)
    `,
    model: 'opus'  // Complex analysis requires strong model
  })

  // Step 6: Generate antithesis if recommended
  if (metaReviewResult.antithesis_generated) {
    await writeAntithesisHypothesis(metaReviewResult.antithesis_content)
  }

  // Step 7: Update STATE.md
  await updateState({
    meta_review_date: new Date().toISOString(),
    portfolio_health: metaReviewResult.overall_health,
    critical_issues: metaReviewResult.critical_issues
  })

  // Step 8: Display report
  return formatMetaReviewOutput(metaReviewResult)
}
```

---

## Summary

**What `/meta-review` does**:
- Analyzes hypothesis collection for systemic issues
- Detects collective biases and shared assumptions
- Generates antithesis to break groupthink
- Identifies unexplored theoretical angles
- Assesses portfolio risk balance

**What `/meta-review` doesn't do**:
- Verify individual hypothesis quality (use `/stress-test`)
- Compare two hypotheses (use `/rank`)
- Generate new hypotheses automatically (use `/hypothesize`)

**Key principle**: `/meta-review` is **strategic portfolio management**, not tactical quality control. It asks "Are we thinking about this wisely?" not "Is this specific idea correct?"

**Mental model**: Think of your hypotheses as an investment portfolio. Individual stocks (hypotheses) may all be good, but a portfolio of only tech stocks is risky. Diversification matters.

**Unique feature**: **Antithesis Mining** - No other system generates scientifically plausible opposite hypotheses to test groupthink. This is your insurance against collective blind spots.

---

**Example usage reminder**:

```bash
/meta-review              # Scan all hypotheses
/meta-review H-001*       # Scan H-001 family only
```

Strategic. Portfolio-level. Groupthink prevention. That's the `/meta-review` way.

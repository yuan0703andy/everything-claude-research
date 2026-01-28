---
name: rank
description: Compare two hypotheses head-to-head using adversarial debate across novelty, verifiability, clarity, and coherence dimensions
argument-hint: <hypothesis-id-1> <hypothesis-id-2>
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

# /rank - Pairwise Hypothesis Comparison

Automated scientific debate to determine which hypothesis is superior.

## Purpose

The `/rank` command implements **pairwise hypothesis comparison** through structured adversarial debate. It determines which of two competing hypotheses should receive higher priority based on:

- **Novelty**: Theoretical/empirical advancement
- **Verifiability**: Feasibility of proving/testing
- **Clarity**: Mechanism specificity and falsifiability
- **Coherence**: Integration with existing knowledge

**Why it exists**: Your team can generate many hypotheses (`/hypothesize`), verify they're sound (`/stress-test`), but still need to decide **"which one should we actually pursue?"** This command provides that decision through rigorous comparison.

---

## Usage

### Basic Usage

```bash
/rank H-001-A H-001-B
```

Compare two hypotheses (A vs B) and determine the winner.

### Full Syntax

```bash
/rank <hypothesis-1> <hypothesis-2> [--weights=custom]
```

**Arguments**:
- `hypothesis-1`: First hypothesis ID (e.g., `H-001-A`)
- `hypothesis-2`: Second hypothesis ID (e.g., `H-001-B`)

**Options**:
- `--weights=custom`: Override DOMAIN.md weights (advanced use only)

---

## What Happens Internally

This is a **high-level intent command**. You don't control the modes—the system orchestrates everything:

```mermaid
User: /rank H-001-A H-001-B
  ↓
1. Load DOMAIN.md
   - Extract ranking weights (novelty, verifiability, clarity, coherence)
   - Extract dimension aliases (e.g., "provability" vs "testability")
  ↓
2. Validate Hypotheses
   - Check both hypothesis files exist
   - Read full content of each
  ↓
3. Spawn ranking-agent
   - Simulate adversarial debate for each dimension
   - Score both hypotheses (0-10 per dimension)
   - Calculate weighted overall scores
   - Determine winner + confidence
  ↓
4. Update Coordinator
   - Apply Elo adjustments (+/- based on margin)
   - Record comparison in STATE.md
  ↓
5. Display Report
   - Show dimensional breakdown
   - Explain winner with evidence
   - Provide recommendation
```

**You see**: Final report with clear winner and reasoning
**You don't see**: Internal debate simulation, score calculations

---

## Output Format

### Example Output

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 PAIRWISE RANKING ► H-003-A vs H-003-B
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Overall Winner: H-003-A ✅

**Confidence**: High
**Weighted Score**: 7.8 vs 6.2 (Δ = 1.6)
**Elo Adjustment**: H-003-A +25 / H-003-B -10

**Executive Summary**:
H-003-A is superior due to significantly higher novelty and clarity.
While H-003-B has better near-term verifiability, A's theoretical
contribution and precise mechanism specification outweigh this
advantage. The novelty gap (8.5 vs 6.0) is decisive given the
domain weights (novelty: 35%, verifiability: 20%).

**Dimensional Breakdown**:
- Won by A: Novelty ✅, Clarity ✅
- Won by B: Verifiability ✅
- Tied: Coherence ⚖️

─────────────────────────────────────────────────────────────

## Dimension Summaries

### Novelty (Weight: 35%) → Winner: H-003-A ✅
Scores: A: 8.5 | B: 6.0

H-003-A proposes minimax optimal rate n^(-2/5) vs B's n^(-1/3).
Literature shows B's rate already achieved [Yang & Barber 2019],
while A's rate has only lower bound proof, not construction.
A's novelty is providing explicit construction achieving bound.

### Verifiability (Weight: 20%) → Winner: H-003-B ✅
Scores: A: 6.0 | B: 8.0

B uses standard LASSO (glmnet), testable in 3-6 months.
A requires custom threshold tuning, estimated 12-18 months.
B's approach significantly more feasible for verification.

### Clarity (Weight: 30%) → Winner: H-003-A ✅
Scores: A: 9.0 | B: 7.0

A specifies exact threshold λ = σ√(log p / n).
B states "adaptive regularization" without specifics.
A's precision enables direct falsification.

### Coherence (Weight: 15%) → Winner: Tie ⚖️
Scores: A: 7.5 | B: 7.5

Both consistent with known minimax theory.
Neither contradicts established results.

─────────────────────────────────────────────────────────────

## Recommendation

✅ **Prioritize H-003-A** for execution

**Rationale**: Novelty (Δ=2.5) and clarity (Δ=2.0) advantages
outweigh verifiability disadvantage (Δ=2.0) given domain weights.

**Caveat**: H-003-A carries higher risk (longer timeline).
Consider keeping H-003-B as backup if resources allow.

**Next Steps**:
1. Elo rankings updated in STATE.md
2. Assign H-003-A to experimentalist for detailed protocol
3. Defer H-003-B but preserve for future comparison

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## When to Use `/rank`

### ✅ Use `/rank` When:

1. **Multiple candidates need prioritization**
   - Generated 3-5 hypotheses via `/hypothesize`
   - All passed `/stress-test` (PASS or GAPS)
   - Need to decide which gets resources first

2. **Elo tournament required**
   - Part of `/lab-meeting` workflow
   - Building hypothesis rankings over time
   - Want systematic comparison, not gut feeling

3. **Close decision needed**
   - Two hypotheses seem equally good
   - Need evidence-based tiebreaker
   - Want to document decision rationale

### ❌ Don't Use `/rank` When:

1. **Only one hypothesis exists** → No comparison needed
2. **One hypothesis is DISPROVED** → Already eliminated
3. **Need multi-hypothesis analysis** → Use `/meta-review` instead
4. **Haven't run `/stress-test` yet** → Verify quality first

---

## Integration with Other Commands

### Complete Workflow

```bash
# Week 1: Generate candidates
/hypothesize "Derive minimax rate for sparse regression"
→ Produces: H-003-A, H-003-B, H-003-C

# Week 2: Verify quality
/stress-test H-003-A  → PASS ✅
/stress-test H-003-B  → PASS ✅
/stress-test H-003-C  → GAPS ⚠️

# Week 3: Compare top candidates
/rank H-003-A H-003-B
→ Winner: H-003-A

# Week 4: Evolve the winner (optional)
/evolve H-003-A  → H-003-A-v2

# Week 5: Execute
/execute-analysis H-003-A-v2
```

### Relationship to `/lab-meeting`

`/lab-meeting` internally uses `/rank` to run Elo tournaments:

```bash
/lab-meeting
→ Automatically runs round-robin ranking
→ Updates Elo leaderboard
→ Identifies top hypothesis
```

You can also run `/rank` standalone for specific comparisons outside lab meetings.

---

## Domain Adaptation

The ranking dimensions and weights **automatically adapt** to your research domain via `DOMAIN.md`:

### Example: Stats Theory

```yaml
# domains/stats-theory/DOMAIN.md
ranking_weights:
  novelty: 0.35        # Theory prizes novelty highly
  verifiability: 0.20  # "Provability" - proof feasibility
  clarity: 0.30        # Rigor and precision critical
  coherence: 0.15      # Theoretical consistency

dimension_aliases:
  verifiability: "Provability"
  coherence: "Theoretical Consistency"
```

Output will show:
```
### Provability (Weight: 20%)  ← Uses alias
### Theoretical Consistency (Weight: 15%)  ← Uses alias
```

### Example: Biomedical Research

```yaml
# domains/biomedical/DOMAIN.md
ranking_weights:
  novelty: 0.25
  verifiability: 0.35  # "Testability" - experimental feasibility
  clarity: 0.20
  coherence: 0.20      # "Observation Fit"

dimension_aliases:
  verifiability: "Testability"
  coherence: "Observation Fit"
```

**Same `/rank` command, different standards applied automatically.**

---

## Advanced Usage

### Custom Weights (Expert Mode)

Override DOMAIN.md weights for specific comparisons:

```bash
/rank H-001-A H-001-B --weights=custom
```

**When prompted, provide**:
```yaml
custom_weights:
  novelty: 0.50       # Temporarily prioritize novelty
  verifiability: 0.10
  clarity: 0.30
  coherence: 0.10
```

**Use cases**:
- Exploratory phase (boost novelty)
- Near deadline (boost verifiability)
- Theoretical purity (boost clarity + coherence)

**Warning**: Custom weights should be **rare**. Default domain weights reflect long-term research strategy. Only override for specific tactical reasons.

---

## Troubleshooting

### Error: "Hypothesis file not found"

**Cause**: Hypothesis ID doesn't exist or typo in path

**Fix**:
```bash
# Check what hypotheses exist
ls hypotheses/

# Use exact filename
/rank H-001-A H-001-B  ✅
/rank H001A H001B      ❌ (wrong format)
```

### Error: "Cannot rank DISPROVED hypothesis"

**Cause**: One hypothesis was flagged DISPROVED by `/stress-test`

**Fix**: Don't rank disproved hypotheses. They're eliminated.

```bash
# This will fail if H-001-B is DISPROVED:
/rank H-001-A H-001-B  ❌

# Instead:
/rank H-001-A H-001-C  ✅ (compare to different hypothesis)
```

### Warning: "Insufficient information for accurate ranking"

**Cause**: Hypothesis files lack detail (sparse descriptions, no citations)

**System behavior**: Ranking proceeds but with lower confidence scores

**Recommendation**: Enhance hypothesis documentation before ranking

---

## FAQs

### Q: Can I rank more than two hypotheses at once?

**A**: No. `/rank` is strictly **pairwise**. For multi-hypothesis comparison:

- **Option 1**: Run multiple `/rank` calls
  ```bash
  /rank H-001-A H-001-B
  /rank H-001-A H-001-C
  /rank H-001-B H-001-C
  ```

- **Option 2**: Use `/lab-meeting` for automatic round-robin tournament

- **Option 3**: Use `/meta-review` for portfolio-level analysis (different purpose)

### Q: What if the scores are very close (tie)?

**A**: Ranking-agent uses tie-breaker criteria:
1. Higher novelty (if weights are balanced)
2. Fewer assumptions (simplicity principle)
3. Earlier generation (first-mover advantage)

Even in ties, you'll get a decision with documented rationale.

### Q: How often should I run `/rank`?

**A**: Recommended frequency:

- **Weekly**: As part of `/lab-meeting` (automatic)
- **Ad-hoc**: When choosing between 2 top candidates
- **After evolution**: Compare `H-001` vs `H-001-v2`

**Don't overuse**: Ranking has diminishing returns. Focus on generating and refining hypotheses first.

### Q: Does `/rank` modify hypothesis files?

**A**: **No**. `/rank` is **read-only** for hypotheses. It only updates:
- `STATE.md` (Elo scores, comparison records)
- Coordinator's internal registry

Original hypothesis files remain unchanged.

### Q: Can I compare hypotheses from different research goals?

**A**: Technically yes, but **not recommended**.

```bash
# BAD: Different goals
/rank H-001-A H-005-B  ❌
# (H-001 = minimax rates, H-005 = robust estimation)
```

**Why**: Comparison assumes similar objectives. Cross-goal ranking produces meaningless scores.

**Exception**: If hypotheses address sub-problems of larger goal, comparison can be valid. Use judgment.

---

## Implementation Notes (for Developers)

### Internal Workflow

```typescript
async function executeRank(hypothesis_a: string, hypothesis_b: string) {
  // Step 1: Validate inputs
  const pathA = `hypotheses/${hypothesis_a}.md`
  const pathB = `hypotheses/${hypothesis_b}.md`

  if (!fileExists(pathA) || !fileExists(pathB)) {
    throw new Error("Hypothesis file not found")
  }

  // Step 2: Check for DISPROVED status
  const statusA = await getHypothesisStatus(hypothesis_a)
  const statusB = await getHypothesisStatus(hypothesis_b)

  if (statusA === 'DISPROVED' || statusB === 'DISPROVED') {
    throw new Error("Cannot rank DISPROVED hypothesis")
  }

  // Step 3: Load DOMAIN.md configuration
  const domainConfig = await readDomainConfig()
  const weights = domainConfig.ranking_weights
  const aliases = domainConfig.dimension_aliases

  // Step 4: Spawn ranking-agent
  const rankingResult = await spawnAgent({
    type: 'ranking-agent',
    prompt: `
      Compare these two hypotheses using domain-adaptive weights:

      Domain weights: ${JSON.stringify(weights)}
      Dimension aliases: ${JSON.stringify(aliases)}

      Hypothesis A (${hypothesis_a}):
      ${readFile(pathA)}

      Hypothesis B (${hypothesis_b}):
      ${readFile(pathB)}

      Provide full adversarial debate and decision.
    `,
    model: 'opus'  // Ranking requires careful reasoning
  })

  // Step 5: Update coordinator
  await updateCoordinator({
    action: 'update_elo',
    hypothesis_a: {
      id: hypothesis_a,
      elo_delta: rankingResult.elo_adjustment[hypothesis_a]
    },
    hypothesis_b: {
      id: hypothesis_b,
      elo_delta: rankingResult.elo_adjustment[hypothesis_b]
    },
    comparison_record: {
      date: new Date().toISOString(),
      winner: rankingResult.overall_decision.better_hypothesis,
      margin: rankingResult.overall_decision.confidence,
      dimensions: rankingResult.dimensional_breakdown
    }
  })

  // Step 6: Display formatted output
  return formatRankingOutput(rankingResult)
}
```

### Error Handling

- Invalid hypothesis IDs → Friendly error with `ls hypotheses/` suggestion
- DISPROVED hypothesis → Explain why comparison is invalid
- Missing DOMAIN.md → Fall back to balanced weights (0.25 each)
- Tie-breaking → Always provide decision, never "cannot decide"

---

## Summary

**What `/rank` does**:
- Compares two hypotheses through adversarial debate
- Applies domain-specific weights from DOMAIN.md
- Calculates winner and updates Elo rankings
- Provides evidence-based recommendation

**What `/rank` doesn't do**:
- Modify hypothesis files (read-only)
- Compare >2 hypotheses at once (pairwise only)
- Replace `/stress-test` (different purpose: quality vs priority)

**Key principle**: `/rank` is about **resource allocation**, not **truth verification**. It assumes both hypotheses are already verified (via `/stress-test`) and helps you decide which deserves priority.

**Next command**: After ranking, typically use `/execute-analysis` on the winner or `/evolve` to refine it further.

---

**Example usage reminder**:

```bash
/rank H-003-A H-003-B
```

Simple, decisive, evidence-based. That's the `/rank` way.

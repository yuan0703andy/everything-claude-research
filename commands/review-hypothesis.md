---
name: review-hypothesis
description: Launch hypothesis review cycle. Experimentalist assesses feasibility, Methodologist reviews methods.
argument-hint: <hypothesis-id>
allowed-tools: Read, WebSearch, Bash, Glob, Grep, Write, Task
---

<review_hypothesis_command>

# /review-hypothesis Command

Start multi-agent review cycle for a hypothesis with domain-specific evaluation.

## Workflow

```
User: /review-hypothesis H-001
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Load Context                   │
    │  - Read hypothesis proposal             │
    │  - Load FULL DOMAIN.md (600+ lines)     │
    │  - Load project context                 │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Parallel Review (spawn both)   │
    │  - Experimentalist: Feasibility         │
    │  - Methodologist: Methods review        │
    │  (Both receive full DOMAIN.md)          │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Aggregate Feedback             │
    │  - Combine reviews                      │
    │  - Identify critical issues             │
    │  - Check domain-specific requirements   │
    │  - Prepare revision guidance            │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 4: Present to User                │
    │  - Show consolidated feedback           │
    │  - Recommend action                     │
    │  - Update Elo based on review           │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 5: Route Next Steps               │
    │  - Approved → Ready for testing         │
    │  - Revise → Back to Theorist            │
    │  - Reject → Archive with reason         │
    └─────────────────────────────────────────┘
```

## Execution Steps

### Step 1: Context Loading (Automatic Execution)

**CRITICAL**: This step must be executed, not just commented. The following files MUST be read:

```markdown
# STEP 1A: Identify hypothesis and domain
Read hypothesis file: hypotheses/proposals/H-XXX-*.md
Extract domain field from hypothesis frontmatter

# STEP 1B: Load domain knowledge (MUST READ FULL FILE)
Read /Users/andyhou/research/domains/{domain}/DOMAIN.md
Store entire content for injection into agent context

# STEP 1C: Load project context
Read PROJECT.md for project-specific information

# STEP 1D: Load current state
Read STATE.md for hypothesis status and history
```

**Output**: Context package containing:
- Complete hypothesis proposal
- Full DOMAIN.md content (600+ lines for stats-theory)
- Project constraints and goals
- Hypothesis review history (if any)

### Step 2: Spawn Parallel Reviews

**IMPORTANT**: Spawn both agents **simultaneously** for efficiency. Each agent receives **full domain knowledge**.

#### To Experimentalist:

```markdown
<task>
## Domain Knowledge (AUTO-INJECTED - FULL CONTENT)

[INJECT COMPLETE DOMAIN.md FILE HERE - ALL 600+ LINES]

This includes:
- Core theoretical frameworks / Methodological traditions
- Proof techniques / Research methods
- Publication standards
- Feasibility evaluation criteria
- Common pitfalls and red flags

## Hypothesis to Assess
[Complete hypothesis proposal from hypotheses/proposals/H-XXX.md]

## Project Context
[From PROJECT.md - constraints, timeline, resources]

## Request
Provide a **Feasibility Report** following your standard format.

Apply domain-specific feasibility framework from DOMAIN.md:

**For stats-theory projects**:
1. Algorithmic feasibility (is estimator constructive?)
2. Simulation design (parameter regimes to test)
3. Numerical considerations (convergence, stability)
4. Verification criteria (upper bound, lower bound, assumptions)

**For policy-making projects**:
1. Data feasibility (observable and accessible?)
2. Identification strategy (clear causal identification?)
3. Measurement (theoretical constructs measurable?)
4. Case selection (if qualitative)

Focus on:
1. Decomposition into testable sub-hypotheses
2. Data requirements and availability
3. Resource estimate (time, computation, data)
4. Technical risks
5. Domain-specific concerns (e.g., computational complexity for stats)
6. Overall feasibility verdict

**Output**: feasibility_report in YAML format per your agent specification
</task>
```

#### To Methodologist:

```markdown
<task>
## Domain Knowledge (AUTO-INJECTED - FULL CONTENT)

[INJECT COMPLETE DOMAIN.md FILE HERE - ALL 600+ LINES]

This includes:
- Core theoretical frameworks / Policy theories
- Proof techniques / Causal inference methods
- Publication standards (Annals, JRSSB / APSR, AJPS)
- Review criteria with examples
- Evaluation checklists (6-dimension for stats, causal framework for policy)
- Red flags (🚩 automatic rejection criteria)

## Hypothesis to Review
[Complete hypothesis proposal from hypotheses/proposals/H-XXX.md]

## Project Context
[From PROJECT.md - field, goals, constraints]

## Request
Provide a **Methods Review Report** following your standard format.

Apply domain-specific evaluation framework from DOMAIN.md:

**For stats-theory projects** (6-Dimension Framework):
1. Proof Completeness (theorem precise? assumptions stated? proof rigorous?)
2. Minimax Optimality (rate derived? lower bound? comparison to existing?)
3. Assumptions (realistic? verifiable? justified?)
4. Computational Considerations (complexity? implementable?)
5. Proof Technique (Fano? Assouad? Le Cam? appropriate?)
6. Contribution Clarity (novel? positioned in literature?)

**For policy-making projects** (Causal Inference Framework):
1. Causal Mechanism (X → M → Y clear? observable?)
2. Identification Strategy (RDD? IV? DID? assumptions?)
3. Internal Validity (selection bias? endogeneity? confounders?)
4. External Validity (scope conditions? generalizability?)
5. Measurement (constructs measurable? validity?)
6. Design Quality (appropriate? justified? powered?)

**Check against DOMAIN.md standards**:
- For stats: Lower bound required? Proof complete? Assumptions stated?
- For policy: Identification strategy specified? Confounders addressed?

Provide clear verdict: **ACCEPT / MINOR REVISION / MAJOR REVISION / REJECT**

**Output**: methods_review in YAML format per your agent specification
</task>
```

### Step 3: Aggregate Results

After both agents complete, synthesize their findings:

```markdown
# Review Aggregation for H-XXX

## Domain Context
**Domain**: [stats-theory | policy-making]
**Domain Standards Applied**: [List key standards checked]

## Feasibility Assessment (Experimentalist)
**Verdict**: [🟢 GREEN / 🟡 YELLOW / 🔴 RED]
**Feasibility Score**: [1-5]
**Key Points**:
- [Summary of main feasibility concerns]
- [Data/resource requirements]
- [Technical risks identified]

**Domain-Specific Concerns**:
- [For stats]: Computational complexity, numerical stability, implementation gap
- [For policy]: Data access, measurement validity, case availability

**Showstoppers**: [Any critical blockers?]

## Methods Review (Methodologist)
**Verdict**: [✅ ACCEPT / ⚠️ MINOR REVISION / 🔧 MAJOR REVISION / ❌ REJECT]
**Rigor Score**: [1-5]
**Key Points**:
- [Summary of methodological assessment]
- [Domain standard compliance]

**Domain-Specific Evaluation**:
- [For stats]: Proof completeness [X/5], Minimax optimality [X/5], Assumptions [X/5]
- [For policy]: Causal mechanism [X/5], Identification [X/5], Internal validity [X/5]

**Critical Issues from DOMAIN.md**:
🚩 **Red Flags** (if any):
- [For stats]: No lower bound? Proof gaps? Assumptions unstated?
- [For policy]: No identification strategy? Confounders ignored? Mechanism not observable?

✅ **Acceptance Signals** (if any):
- [For stats]: Tight rates? Matching lower bound? Complete proof?
- [For policy]: Clear identification? Mechanism observable? Threats addressed?

## Consolidated Verdict

| Criterion | Status | Score | Notes |
|-----------|--------|-------|-------|
| Feasibility (Experimentalist) | [GREEN/YELLOW/RED] | [X/5] | [Key concern] |
| Rigor (Methodologist) | [ACCEPT/REVISE/REJECT] | [X/5] | [Key concern] |
| Domain Standards Compliance | [PASS/FAIL] | - | [Specific issues] |
| **Overall** | [**READY/REVISE/REJECT**] | - | [Rationale] |

### Decision Logic:
- **READY**: Both GREEN + ACCEPT, domain standards met
- **REVISE**: YELLOW or MINOR REVISION, fixable issues
- **REJECT**: RED or REJECT or critical domain standard violated
```

### Step 4: Output Presentation

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 REVIEW ► H-XXX: [Hypothesis Title]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**Domain**: [stats-theory | policy-making]

## Feasibility Assessment (Experimentalist)
**Verdict**: 🟢 GREEN / 🟡 YELLOW / 🔴 RED
**Score**: [X/5]

[2-3 sentence summary of feasibility assessment]

**Key Concerns**:
- [Concern 1]
- [Concern 2]

**Resource Estimate**:
- Time: [Estimate]
- Data: [Requirements]
- Computation: [Requirements]

## Methods Review (Methodologist)
**Verdict**: ✅ ACCEPT / ⚠️ MINOR REVISION / 🔧 MAJOR REVISION / ❌ REJECT
**Score**: [X/5]

[2-3 sentence summary of methodological assessment]

**Domain-Specific Evaluation**:
[For stats-theory]:
- Proof Completeness: [X/5] - [Brief comment]
- Minimax Optimality: [X/5] - [Brief comment]
- Lower Bound: [✓ Provided / ✗ Missing]

[For policy-making]:
- Causal Mechanism: [X/5] - [Brief comment]
- Identification Strategy: [X/5] - [Brief comment]
- Identification Clarity: [✓ Clear / ✗ Unclear]

─────────────────────────────────────────────────────

## Critical Issues to Address

[If any critical issues:]

1. **[Issue Category]**: [Specific issue]
   - **Severity**: [CRITICAL / MODERATE / MINOR]
   - **From Domain Standards**: [Which DOMAIN.md requirement]
   - **Impact**: [What this affects]
   - **Fix**: [Suggested resolution]

2. **[Issue Category]**: [Specific issue]
   - **Severity**: [CRITICAL / MODERATE / MINOR]
   - **Impact**: [What this affects]
   - **Fix**: [Suggested resolution]

[If no critical issues:]
No critical issues identified. Hypothesis meets domain standards.

─────────────────────────────────────────────────────

## Domain Standard Compliance

**For stats-theory**:
- [✓/✗] Lower bound provided
- [✓/✗] Assumptions explicitly stated
- [✓/✗] Proof rigorous (no hand-waving)
- [✓/✗] Comparison to existing rates
- [✓/✗] Computational complexity discussed

**For policy-making**:
- [✓/✗] Identification strategy specified
- [✓/✗] Causal mechanism clear
- [✓/✗] Threats to validity discussed
- [✓/✗] Measurement approach justified
- [✓/✗] Scope conditions stated

─────────────────────────────────────────────────────

## Recommendation: [✅ APPROVE / ⚠️ REVISE / ❌ REJECT]

[2-3 sentences rationale for recommendation based on both reviews and domain standards]

## Elo Update
**Previous**: [Old Elo] → **New**: [New Elo] ([+/-] [points])
**Reason**: [Review outcome]

Elo adjustment:
- APPROVE (both): +30
- APPROVE (one), REVISE (one): +10
- REVISE (both): -10
- REJECT (any): -30
- Domain red flag: additional -20

─────────────────────────────────────────────────────

## Next Steps

[If APPROVE]:
✅ **Hypothesis approved for testing**
→ `/execute-analysis H-XXX` to proceed with verification
→ Hypothesis moved to "Ready for Testing" status

[If REVISE]:
⚠️ **Revisions needed before proceeding**
→ Address [N] critical issues listed above
→ "Revise H-XXX" to send back to Theorist with feedback
→ Or address specific issues: "Fix issue 1 and 2 for H-XXX"
→ Maximum 3 revision iterations allowed

[If REJECT]:
❌ **Hypothesis not viable in current form**
→ Critical domain standards violated or fundamental flaws
→ "Archive H-XXX" to document and move on
→ Or "Salvage H-XXX" to extract useful elements for new hypothesis

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

**⏸️ WAITING FOR EXPLICIT APPROVAL**

The system will NOT automatically proceed. User must explicitly respond:

**To approve and proceed**:
- "approve" or "yes" → Proceed to execution phase
- "approve H-XXX" → Approve and move to ready status

**To request revisions**:
- "revise" or "needs changes" → Send back to Theorist with feedback
- "fix issue 1 and 2" → Specify which issues to address
- "major revision needed" → Comprehensive rework required

**To reject**:
- "reject" or "archive" → Archive hypothesis with documented reason
- "salvage H-XXX" → Extract useful elements for new hypothesis

**To modify recommendation**:
- "discuss" → Open discussion before deciding
- "consult Methodologist" → Get additional expert opinion
- "explain [specific issue]" → Clarify specific concern

**CRITICAL**: No code execution, no state changes until explicit approval received.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Step 5: State Update (After Approval)

Update tracking files:

```markdown
# Update hypotheses/HYPOTHESES.md

## Status Change
H-XXX: Draft/Under Review → [Ready for Testing / Revision Requested / Archived]

## Elo Update
H-XXX: [Old Elo] → [New Elo]

# Update STATE.md

## Hypothesis Pipeline

### [New Status Section]
| ID | Title | Review Outcome | Domain Issues | Next Step |
|----|-------|----------------|---------------|-----------|
| H-XXX | [Title] | [APPROVE/REVISE/REJECT] | [Domain flags] | [Action] |

# Create review record

Save to: reviews/H-XXX-review-[date].md
- Complete Experimentalist report
- Complete Methodologist report
- Aggregated feedback
- Decision rationale
- Domain standard compliance check
```

## Review Outcome Actions

| Outcome | Elo Change | Status Change | Action |
|---------|------------|---------------|--------|
| APPROVE (both) + domain ✓ | +30 | → Ready for Testing | Proceed to execution |
| APPROVE (one), REVISE (one) | +10 | → Revision Requested | Minor fixes needed |
| REVISE (both) | -10 | → Revision Requested | Send back to Theorist |
| REJECT (any) OR domain ✗ | -30 | → Archived | Document reason, shelve |

## Revision Loop

If REVISE:
```
1. Consolidate feedback for Theorist
   - Include domain-specific requirements
   - Cite specific DOMAIN.md standards violated
2. Theorist revises hypothesis
3. Re-run /review-hypothesis H-XXX
4. Repeat until APPROVE or REJECT
```

**Maximum iterations**: 3 revisions
**After 3 revisions**: Escalate to PI for decision

## Domain-Specific Review Notes

### For Statistical Theory Projects:

**Common rejection reasons**:
- No minimax lower bound (CRITICAL - cite DOMAIN.md)
- Proof has gaps or hand-waving
- Assumptions not stated explicitly
- Computational complexity ignored

**Approval requires**:
- Clear theorem statement
- Minimax rate derived
- Lower bound provided (Fano, Assouad, or Le Cam)
- Assumptions stated and justified
- Proof complete and rigorous

### For Policy Research Projects:

**Common rejection reasons**:
- No identification strategy (CRITICAL - cite DOMAIN.md)
- Selection bias not addressed
- Confounders ignored
- Mechanism not observable

**Approval requires**:
- Clear causal mechanism (X → M → Y)
- Identification strategy specified (RDD, IV, DID, etc.)
- Threats to validity discussed
- Measurement approach justified
- Scope conditions stated

## Best Practices

1. **Always inject full DOMAIN.md** - Agents need complete domain knowledge
2. **Spawn reviewers in parallel** - Saves time
3. **Check domain standards explicitly** - Use DOMAIN.md checklists
4. **Document domain concerns** - Flag violations clearly
5. **Cite specific DOMAIN.md sections** - When giving feedback
6. **Be domain-aware** - "No lower bound" is critical for stats, not policy
7. **Update Elo based on domain compliance** - Violations incur penalties

## Example Domain-Specific Feedback

### Stats Theory:
```
❌ REJECT

**Critical Issue from DOMAIN.md**:
No minimax lower bound provided. Per Annals of Statistics standards (DOMAIN.md section 3.2),
claims of optimality require information-theoretic lower bounds via Fano, Assouad, or Le Cam.

**Required Action**:
Derive lower bound proving that no estimator can achieve rate better than n^(-2/5) under
sparsity s = O(n^(1/5)). Suggest using Fano's method over hypercube construction.
```

### Policy Research:
```
⚠️ MAJOR REVISION

**Critical Issue from DOMAIN.md**:
Identification strategy unclear. Per APSR standards (DOMAIN.md section 4.3), causal claims
require explicit identification strategy with stated assumptions.

**Required Action**:
Specify identification approach (suggest regression discontinuity using policy threshold).
State identifying assumptions clearly. Discuss how to rule out confounders. Add McCrary
density test to verify no manipulation at threshold.
```

</review_hypothesis_command>

---
name: verify-results
description: Execute goal-backward verification. Check if research truly achieves its goals. Domain-aware verification with field-specific standards.
argument-hint: <hypothesis-id or "phase">
allowed-tools: Read, Bash, Glob, Grep, Write, Task
---

<verify_results_command>

# /verify-results Command

Run goal-backward verification to ensure research achieves its intended goals AND meets domain-specific publication standards.

## Purpose

Verify that:
- Research goals are actually achieved (not just tasks completed)
- All necessary conditions are met
- **Domain standards are satisfied** (lower bounds for stats, identification for policy)
- Results are publication-ready

## Workflow

```
User: /verify-results H-001
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 1: Load Context                   │
    │  - Original hypothesis and goals        │
    │  - Load DOMAIN.md for standards         │
    │  - Research design                      │
    │  - Execution artifacts                  │
    │  - Results/outputs                      │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 2: Spawn Verifier                 │
    │  - Inject domain knowledge              │
    │  - Goal extraction (domain-aware)       │
    │  - Must-have derivation (domain-specific)│
    │  - Verification checks (domain standards)│
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 3: Present Results                │
    │  - Verification report                  │
    │  - Gaps identified (including domain)   │
    │  - Domain compliance status             │
    │  - Recommendation                       │
    └─────────────────────────────────────────┘
          │
          ▼
    ┌─────────────────────────────────────────┐
    │  Step 4: Route Next Steps               │
    │  - VERIFIED → Complete hypothesis       │
    │  - GAPS → Create fix plans              │
    │  - BLOCKED → Escalate to PI             │
    │  - Domain issues → Consult Methodologist│
    └─────────────────────────────────────────┘
```

## Execution

### Step 1: Context Loading (Automatic)

**CRITICAL**: Load full context including domain knowledge before verification.

```markdown
# STEP 1A: Identify hypothesis and domain
Read hypothesis file: hypotheses/proposals/H-XXX-*.md
Extract domain field from hypothesis frontmatter
Extract original goals and success criteria

# STEP 1B: Load domain knowledge (MUST READ FULL FILE)
Read /Users/andyhou/research/domains/{domain}/DOMAIN.md

This file contains (600+ lines):
- Domain-specific success criteria
- Publication standards
- Verification checklists
- Red flags (e.g., "no lower bound" for stats)
- Quality standards

# STEP 1C: Load execution artifacts
Read .planning/phases/phase-N-EXECUTE.md
Read results files from execution
Gather all outputs and documentation

# STEP 1D: Load project context
Read PROJECT.md for constraints
Read STATE.md for current status
```

**Output**: Context package containing:
- Complete hypothesis with goals
- Full DOMAIN.md content (600+ lines)
- Execution artifacts
- Results and outputs
- Domain-specific requirements

### Step 2: Spawn Verifier with Domain Knowledge

```markdown
<task>
## Domain Knowledge (AUTO-INJECTED - FULL CONTENT)

[INJECT COMPLETE DOMAIN.md FILE HERE - ALL 600+ LINES]

This includes:
- Domain-specific verification criteria
- Publication standards (Annals, APSR, etc.)
- Red flags (automatic BLOCKED verdicts)
- Must-have requirements for publication
- Common gaps in the domain

## Verification Target
**Hypothesis ID**: H-XXX
**Domain**: [stats-theory | policy-making]
**Verification Type**: [Design / Pre-Execution / Post-Execution / Results]

## Original Goals
[From hypothesis proposal]

**Success Criteria**:
- General: [What would constitute an answer]
- Domain-Specific: [What standards must be met for publication]

## Research Design
[From approved design]

**Domain Requirements**:
- For stats-theory: Lower bound strategy, proof approach, simulation plan
- For policy-making: Identification strategy, confounder control, mechanism tests

## Execution Artifacts
[What was done - data collected, analyses run]

**Domain-Specific Outputs**:
- For stats-theory: Simulations, rate verification, lower bound proof
- For policy-making: Identification tests, robustness checks, mechanism evidence

## Results
[Findings/outputs]

## Request
Perform domain-aware goal-backward verification:

1. **Extract goals and success criteria**
   - What was the research question?
   - What are the domain-specific requirements?

2. **Derive must-haves from goals (domain-aware)**
   - General must-haves for this type of research
   - Domain-specific must-haves from DOMAIN.md:
     * Stats-theory: Lower bound, proof completeness, assumptions stated
     * Policy-making: Identification strategy, confounders addressed, mechanism observable

3. **Check each must-have against artifacts**
   - Is it present?
   - Is it done correctly?
   - Does it meet domain standards?

4. **Apply domain-specific verification checklists**
   - Use evaluation frameworks from DOMAIN.md
   - Check for domain red flags
   - Verify publication readiness

5. **Identify gaps (general + domain-specific)**
   - What's missing?
   - What domain standards are not met?
   - What would cause desk rejection?

6. **Provide verdict with domain compliance**
   - VERIFIED / GAPS IDENTIFIED / BLOCKED
   - Domain compliance: MEETS STANDARDS / PARTIAL / FAILS

Output: Full Verification Report (domain-aware format per verifier.md)
</task>
```

### Step 3: Output Presentation

```markdown
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 VERIFY ► H-001: [Title]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## Domain Context
**Domain**: [stats-theory | policy-making]
**Publication Standards**: [Annals of Statistics | APSR | etc.]
**Domain Knowledge Source**: `/Users/andyhou/research/domains/{domain}/DOMAIN.md`

## Goal Verification

**Primary Goal**: [What we were trying to learn]
**Success Criteria**:
- General: [What constitutes success]
- Domain-Specific: [Field requirements for publication]

### Must-Haves Check
| Must-Have | Domain Req? | Status | Notes |
|-----------|-------------|--------|-------|
| [MH-1] | No | ✅ | [Verified how] |
| [MH-2] | **Yes** | ✅ | [Verified how] |
| [MH-3] | **Yes** | ⚠️ | [Partial - issue] |
| [MH-4] | **Yes** | ❌ | [Missing - critical] |

### Domain-Specific Compliance

**Stats Theory** (if applicable):
| Criterion | Status | Notes |
|-----------|--------|-------|
| Lower bound proven | ✅/⚠️/❌ | [Details] |
| Proof complete | ✅/⚠️/❌ | [Details] |
| Assumptions stated | ✅/⚠️/❌ | [Details] |
| Rate minimax optimal | ✅/⚠️/❌ | [Details] |
| Computational complexity | ✅/⚠️/❌ | [Details] |

**Policy Making** (if applicable):
| Criterion | Status | Notes |
|-----------|--------|-------|
| Identification strategy specified | ✅/⚠️/❌ | [Details] |
| Causal mechanism clear | ✅/⚠️/❌ | [Details] |
| Confounders addressed | ✅/⚠️/❌ | [Details] |
| Threats to validity discussed | ✅/⚠️/❌ | [Details] |
| Scope conditions stated | ✅/⚠️/❌ | [Details] |

### Logic Chain
```
Goal: [State goal]
  ├── MH-1 (General): ✅
  ├── MH-2 (Domain): ✅
  ├── MH-3 (Domain - CRITICAL): ⚠️ (partial)
  └── MH-4 (Domain - CRITICAL): ❌ (missing)

Chain Status: INCOMPLETE
Domain Standards: PARTIAL
```

─────────────────────────────────────────────────────

## Verdict

### Overall Status: [VERIFIED / GAPS IDENTIFIED / BLOCKED]
### Domain Compliance: [MEETS STANDARDS / PARTIAL / FAILS STANDARDS]

[If VERIFIED]:
✅ All must-haves satisfied. Goal achieved. Domain standards met.
**Confidence**: [High/Medium/Low]
**Domain Status**: Ready for submission to [journal]
**Remaining risks**: [Any residual concerns]

[If GAPS IDENTIFIED]:
⚠️ Gaps found that need addressing:

### Gap 1: [Description]
- **Impact**: [What this affects]
- **Criticality**: Critical / Important / Minor
- **Domain Issue**: Yes/No - [If yes, which standard]
- **Fix**: [How to address]
- **Effort**: [Estimate]

### Gap 2: [Description]
[Same structure]

**Domain-Specific Gaps**:

For stats-theory:
- 🚩 No lower bound (CRITICAL) - Cannot claim minimax optimality
- 🚩 Proof gap in Lemma 3 - Reviewers will reject
- ⚠️ Assumptions not stated - Required for publication

For policy-making:
- 🚩 No identification strategy (CRITICAL) - Causal claims unsupported
- 🚩 Selection bias unaddressed - Estimates biased
- ⚠️ Mechanism not tested - Theory incomplete

[If BLOCKED]:
❌ Critical issues prevent goal achievement:
- [Issue description]
- **Domain Red Flag**: [Which critical standard violated]
- Requires: [What's needed - likely PI decision or major revision]

─────────────────────────────────────────────────────

## Next Steps

[If VERIFIED]:
✅ **Hypothesis verified and domain-compliant**
→ `/complete-hypothesis H-001` to record success
→ Update Elo: +50 for verified hypothesis
→ Ready for manuscript preparation
→ Publication target: [Journal based on domain]

[If GAPS]:
⚠️ **Address gaps before proceeding**
→ "Fix gaps" to create remediation tasks
→ After fixes: re-run `/verify-results H-001`
→ Prioritize domain-critical gaps first
→ Consult Methodologist if domain standards unclear

**Priority Order**:
1. Domain-critical gaps (red flags) - Must fix for publication
2. General critical gaps - Must fix for goal achievement
3. Non-critical gaps - Should fix for quality

**Domain-Specific Remediation**:
- Stats-theory: [e.g., "Derive lower bound using Fano's method"]
- Policy-making: [e.g., "Specify RDD identification at policy threshold"]

[If BLOCKED]:
❌ **Escalation needed**
→ Requires PI decision: [Specific question]
→ Possible actions:
  - Major hypothesis revision
  - Change domain approach
  - Archive hypothesis (document reason)
→ Consult Methodologist for domain standard interpretation

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

## Verification Types

| Command | Scope | Domain Awareness |
|---------|-------|------------------|
| `/verify-results H-001` | Single hypothesis | Full domain standards check |
| `/verify-results design H-001` | Verify design only (before execution) | Domain design requirements |
| `/verify-results phase-1` | Entire research phase | Aggregate domain compliance |
| `/verify-results all` | Full project verification | Cross-domain if applicable |

## Gap Resolution Flow

```
/verify-results H-001
    │
    │ [GAPS IDENTIFIED]
    │ Domain compliance: PARTIAL
    ▼
"Fix gaps"
    │
    ▼
System creates fix tasks (prioritized by domain criticality):
- Fix-001: [DOMAIN-CRITICAL] Derive lower bound
- Fix-002: [CRITICAL] Complete proof of Lemma 3
- Fix-003: [IMPORTANT] State assumptions explicitly
    │
    ▼
Execute fixes (domain-critical first)
    │
    ▼
/verify-results H-001 (re-verify with domain check)
    │
    │ [VERIFIED]
    │ Domain compliance: MEETS STANDARDS
    ▼
/complete-hypothesis H-001
```

## Domain-Aware Verification Scenarios

### Scenario 1: Stats Theory - Missing Lower Bound

```
Verdict: BLOCKED
Domain Compliance: FAILS STANDARDS

🚩 Domain Red Flag: No minimax lower bound

Impact: Cannot claim minimax optimality, paper will be desk-rejected
Criticality: CRITICAL (publication blocker)
Fix: Derive information-theoretic lower bound using:
  - Fano's method (recommended for this problem structure)
  - Assouad's lemma (if localized estimation)
  - Le Cam's two-point method (if testing problem)
Effort: 1-2 weeks
Consultation: Recommend discussing approach with Theorist

Required Action: Derive lower bound before re-verification
Cannot proceed to publication without this
```

### Scenario 2: Policy Research - Weak Identification

```
Verdict: GAPS IDENTIFIED
Domain Compliance: PARTIAL

⚠️ Domain Issue: Identification strategy unclear

Impact: Causal claims not credible, reviewers will request major revision
Criticality: CRITICAL (publication blocker)
Fix: Specify identification strategy:
  - Propose RDD at policy threshold (threshold = X)
  - State identifying assumptions (continuity, no manipulation)
  - Plan robustness checks (McCrary test, placebo thresholds)
Effort: Design clarification (1 week) + implementation (2 weeks)
Consultation: Recommend review with Methodologist

Required Action: Clarify identification before execution
This is blocking execution, not just publication
```

## Integration with Other Commands

```
/execute-analysis H-001        Execute with domain standards
        ↓
/verify-results H-001          ← Current step (domain-aware verification)
        ↓
[If VERIFIED]
/complete-hypothesis H-001     Record success

[If GAPS]
Fix gaps (prioritize domain-critical)
        ↓
/verify-results H-001          Re-verify
```

## Best Practices

⚠️ **Verify early and often**
- Run design verification before execution
- Don't wait until end to check domain standards
- Early detection of domain issues saves time

⚠️ **Domain standards are not negotiable**
- "No lower bound" is not a minor issue for stats theory
- "No identification" is not a detail for policy research
- These are publication requirements, not preferences

⚠️ **Distinguish general vs domain gaps**
- General gaps affect goal achievement
- Domain gaps affect publication readiness
- Prioritize domain-critical gaps (red flags)

⚠️ **Consult Methodologist for domain standards**
- When domain compliance is unclear
- When alternative approaches may meet standards
- Better to clarify than to assume

⚠️ **Document everything in verification**
- Verification reports become part of research record
- Useful for manuscript preparation
- Demonstrates thoroughness to reviewers

</verify_results_command>

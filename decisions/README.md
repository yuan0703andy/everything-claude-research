# Methodological Decision Records (MDRs)

This directory contains records of significant methodological decisions made during the research process.

## Purpose

MDRs serve as an audit trail for important research decisions, documenting:
- **What** decision was made
- **Why** it was made (context and rationale)
- **What alternatives** were considered
- **What consequences** (positive and negative) we accept

## When to Create an MDR

Create an MDR when:
- Choosing between different proof techniques (e.g., Fano vs Assouad)
- Selecting identification strategies (e.g., RDD vs IV vs DID)
- Making assumptions (e.g., sub-Gaussian noise vs bounded moments)
- Choosing estimators (e.g., ℓ₁ vs ℓ₀ penalization)
- Deciding on simulation parameters
- Selecting robustness check approaches
- Any decision that significantly affects research validity or approach

## MDR Naming Convention

```
MDR-[NNN]-[short-slug].md

Examples:
- MDR-001-fano-lower-bound.md
- MDR-002-rdd-identification.md
- MDR-003-subgaussian-assumption.md
```

## MDR Template

See `MDR-000-TEMPLATE.md` for the standard format.

## MDR Lifecycle

1. **Proposed** - Draft MDR during decision-making
2. **Accepted** - Decision finalized and implemented
3. **Superseded** - Replaced by a newer decision (document which MDR supersedes this)
4. **Rejected** - Decision not taken (document why)

## Integration with Research Workflow

- Reference MDRs in hypothesis proposals: "Per MDR-001, we use Fano's method"
- Link MDRs in verification reports: "Assumption justified in MDR-003"
- Update MDRs if consequences become apparent during execution
- Review MDRs during manuscript preparation

## Domain-Specific Decisions

### Stats-Theory Common MDRs
- Lower bound technique selection
- Proof strategy (direct vs reduction)
- Assumption strength trade-offs
- Computational complexity vs statistical optimality
- Simulation parameter choices

### Policy-Making Common MDRs
- Identification strategy selection
- Confounder set specification
- Mechanism measurement approach
- Scope condition boundaries
- Robustness check priorities

---

## Example MDR

See `MDR-000-TEMPLATE.md` for full template, or quick example:

```markdown
# MDR-001: Use Fano's Inequality for Minimax Lower Bound

**Date**: 2025-01-27
**Status**: Accepted
**Domain**: stats-theory
**Related Hypotheses**: H-003

## Context
Need to prove minimax lower bound for sparse linear regression with p >> n.

## Decision
Use Fano's inequality over hypercube packing construction.

## Rationale
- Problem is global estimation (not local)
- Fano gives tight rates for this structure
- Well-established in sparse estimation literature
- Matches existing work for comparability

## Alternatives Considered

### Assouad's Lemma
- **Pros**: Tighter for local minimax problems
- **Cons**: Requires local neighborhoods, complex for global estimation
- **Why not chosen**: Problem is global estimation

### Le Cam's Two-Point Method
- **Pros**: Simpler proof
- **Cons**: Often gives loose bounds
- **Why not chosen**: Expect log factors that Le Cam wouldn't capture

## Consequences

### Positive
- Tight lower bound matching upper bound
- Standard technique, reviewers familiar
- Connects to existing literature

### Negative
- Requires careful packing construction
- More complex than Le Cam
- Need to verify mutual information bounds

## Implementation Notes
- Use support set of size s as the packing parameter
- Binary {±1} values for simplicity
- KL divergence bounded by signal strength

## References
- Tsybakov (2009) "Introduction to Nonparametric Estimation"
- Raskutti et al. (2011) "Minimax Rates of Estimation for High-Dimensional Linear Regression"
```

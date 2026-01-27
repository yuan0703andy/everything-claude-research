# MDR-001: Use Fano's Inequality for Minimax Lower Bound (EXAMPLE)

**Date**: 2025-01-27
**Status**: Accepted (Example)
**Domain**: stats-theory
**Related Hypotheses**: H-001 (example)
**Authors**: Research Team

---

## Context

Working on hypothesis H-001 about sparse linear regression in high dimensions (p >> n). We need to prove a minimax lower bound to claim our estimator is optimal.

**Research stage**: Theoretical development, before simulation
**Constraints**: Limited time (2 weeks for proof), need technique reviewers will accept
**Decision needed**: Which lower bound technique to use?

---

## Decision

**Use Fano's inequality over hypercube packing construction** to derive minimax lower bound for sparse estimation.

### Detailed Decision
- Construct finite hypothesis class over support sets of size s
- Use binary {±1}^s signal values for simplicity
- Bound mutual information I(Y; J) where J indexes hypotheses
- Apply Fano to get minimax rate n^(-1) min(s, √(s log p))

---

## Rationale

Key factors leading to this choice:

1. **Problem structure**: This is a **global estimation** problem (estimate entire β), not local minimax. Fano is designed for global problems.

2. **Literature precedent**: Raskutti et al. (2011), Verzelen (2012), and other sparse estimation papers use Fano. Using same technique makes our work comparable.

3. **Tightness**: For global estimation with structural assumptions (sparsity), Fano gives tight rates that match upper bounds.

4. **Reviewer acceptance**: Annals reviewers are familiar with Fano proofs in this setting. Well-established technique reduces reviewer skepticism.

---

## Alternatives Considered

### Alternative 1: Assouad's Lemma
**Description**: Use Assouad's lemma with local hypercube construction

**Pros**:
- Can give tighter bounds for **local** minimax problems
- Elegant proof structure
- Captures local adaptivity

**Cons**:
- Designed for local neighborhoods, not global estimation
- More complex construction for global problems
- Requires defining "neighborhood" structure

**Why not chosen**: Our problem is global estimation (estimate all of β), not local (estimate β at a point). Assouad is overkill and inappropriate for this structure.

### Alternative 2: Le Cam's Two-Point Method
**Description**: Use Le Cam's method with two carefully chosen parameter values

**Pros**:
- Simplest proof (just two hypotheses)
- Easy to understand
- Quick to verify

**Cons**:
- Often gives **loose bounds** (misses log factors)
- Cannot capture structure (sparsity)
- Typically sub-optimal for structured problems

**Why not chosen**: For sparse problems, we expect logarithmic factors (log p). Le Cam typically misses these and gives polynomial rates only. Would result in sub-optimal bound.

### Alternative 3: Metric Entropy / Packing Numbers
**Description**: Direct metric entropy approach

**Pros**:
- General framework
- Works for any parameter space

**Cons**:
- Requires computing metric entropy (can be difficult)
- Less direct than Fano

**Why not chosen**: Fano is essentially metric entropy applied with information theory, but more streamlined for our setting. No advantage to direct entropy computation.

---

## Consequences

### Positive Consequences
- **Tight lower bound**: Matches our upper bound up to constants (n^(-1) √(s log p))
- **Established technique**: Reviewers familiar, reduces proof verification burden
- **Correct for problem type**: Fano designed for global problems like ours
- **Literature alignment**: Can directly compare to existing sparse estimation results

### Negative Consequences
- **Proof complexity**: More complex than Le Cam (but manageable)
- **Packing argument needed**: Must construct explicit packing set (technical but standard)
- **Mutual information bounds**: Need careful KL divergence calculations

### Risks
- **Loose constants**: Fano can give loose constant factors (only get asymptotic rates)
  - **Mitigation**: Document that we prove rate optimality, not constant optimality
- **Technical errors**: Mutual information calculation could have errors
  - **Mitigation**: Follow standard template from Tsybakov (2009), double-check KL bounds

---

## Domain-Specific Considerations

### Stats-Theory
- ✅ **Minimax optimality**: Fano directly gives minimax lower bound
- ✅ **Assumptions**: Standard assumptions (sub-Gaussian noise) sufficient for Fano
- ✅ **Proof technique**: Well-accepted in statistical theory community
- ✅ **Publication standards**: Meets Annals/JRSSB requirements for lower bounds

**Standards alignment**:
- DOMAIN.md requires information-theoretic lower bound ✓
- Must use Fano, Assouad, or Le Cam ✓ (chosen Fano)
- Lower bound must be tight ✓ (matches upper bound rate)

---

## Implementation Notes

### Construction Details
1. **Parameter space**: Θ = {β : ||β||₀ ≤ s, β_j ∈ {±1} for j in support}
2. **Packing set size**: M ≥ (p choose s) × 2^s ≥ (p/s)^s × 2^s
3. **Signal strength**: Set μ = σ√(log M / n) to balance signal/noise

### Proof Steps
1. Define packing set {β₁, ..., β_M}
2. Calculate KL divergence: KL(P_i || P_j) ≤ ||β_i - β_j||² / σ² ≤ 4s μ²
3. Bound average KL: (1/M) Σ KL(P_i || P̄) ≤ 2s μ² ≤ 2 log M
4. Apply Fano: Minimax risk ≥ (distance × (1 - (log M + log 2) / log M))
5. Simplify: Get rate n^(-1) √(s log p)

### Verification Strategy
- Check against Tsybakov (2009) Theorem 2.7
- Compare to Raskutti et al. (2011) proof
- Verify with Methodologist before claiming optimality

### Timeline
- Week 1: Construct proof, verify calculations
- Week 2: Write up formally, cross-check with references

---

## Success Criteria

How we'll know this decision was correct:

- [x] **Proof complete**: All steps verified without gaps
- [x] **Rate matches upper bound**: Lower bound n^(-1)√(s log p) matches our estimator
- [x] **Methodologist approval**: Passes domain standards check
- [ ] **Simulation confirms**: Empirical rate matches theoretical (after simulation)
- [ ] **Reviewer acceptance**: If we submit, reviewers accept the lower bound

---

## References

### Academic Literature
1. **Tsybakov (2009)** "Introduction to Nonparametric Estimation" - Standard reference for Fano proofs
2. **Raskutti et al. (2011)** JMLR "Minimax Rates of Estimation for High-Dimensional Linear Regression" - Uses Fano for exact same problem
3. **Yu (1997)** "Assouad, Fano, and Le Cam" - Comparison of lower bound techniques
4. **Verzelen (2012)** EJS - Another Fano proof in sparse setting

### Related MDRs
- None yet (this is first MDR)

### Related Hypotheses
- H-001 (example): Minimax optimal sparse estimation

---

## Review History

| Date | Reviewer | Status | Comments |
|------|----------|--------|----------|
| 2025-01-27 | Methodologist | Proposed | Fano appropriate for global problem |
| 2025-01-27 | Theorist | Accepted | Proof outline verified |

---

## Updates

No updates yet.

---

## Notes

- This is an **EXAMPLE MDR** to demonstrate format
- In practice, create MDR when actually making methodological decisions
- Update status to "Accepted" after proof is complete and verified
- If simulation shows rate doesn't match, may need to revisit (update this MDR)

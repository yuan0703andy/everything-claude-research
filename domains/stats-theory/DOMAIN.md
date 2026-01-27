# Statistical Theory Domain Knowledge

**Last Updated**: 2026-01-27
**Maintained By**: Research Team

---

## Domain Overview

Statistical theory studies the mathematical properties and fundamental limits of inference methods. The core question is: Given a finite sample, how accurately can we infer unknown parameters or test hypotheses? The key concept is **minimax optimality** - best performance under worst-case scenarios.

---

## Core Theoretical Frameworks

### 1. Decision Theory (Wald, Le Cam)
**Core idea**: Statistical inference as a decision problem
- **Parameter space** Θ
- **Decision space** D
- **Loss function** L(θ, d)
- **Risk** R(θ, δ) = E[L(θ, δ(X))]
- **Minimax risk**: inf_δ sup_θ R(θ, δ)

**Key references**:
- Wald (1950) - Statistical Decision Functions
- Le Cam (1986) - Asymptotic Methods in Statistical Decision Theory

### 2. Minimax Theory (Donoho, Johnstone)
**Core idea**: Optimality in the worst case
- **Minimax rate**: n^(-α) where α depends on problem smoothness
- **Lower bound**: Prove that no estimator can be faster than this
- **Upper bound**: Construct an estimator that achieves this rate

**Typical results**:
- Nonparametric regression: n^(-2β/(2β+d)) where β=smoothness, d=dimension
- High-dimensional sparse estimation: (s log p)/n where s=sparsity, p=dimension

**Key references**:
- Donoho & Johnstone (1994) - Ideal spatial adaptation via wavelet shrinkage
- Donoho & Liu (1991) - Geometrizing rates of convergence, I-III

### 3. High-Dimensional Statistics (Wainwright, Bühlmann)
**Core idea**: Statistical inference when p >> n
- **Sparsity**: Only s << p variables are important
- **Restricted eigenvalue condition**: Guarantees uniqueness
- **Incoherence**: Low correlation in design matrix

**Typical methods**:
- Lasso: ℓ1 penalty
- Dantzig selector: ℓ∞ constraint
- SCAD, MCP: Non-convex penalties

**Key references**:
- Wainwright (2019) - High-Dimensional Statistics: A Non-Asymptotic Viewpoint
- Bühlmann & van de Geer (2011) - Statistics for High-Dimensional Data

### 4. Empirical Process Theory (van der Vaart, Wellner)
**Core idea**: Uniform laws for stochastic processes
- **Glivenko-Cantelli**: Uniform LLN
- **Donsker**: Functional CLT
- **Bracketing & covering numbers**: Complexity measures

**Applications**:
- M-estimation
- Maximum likelihood
- Nonparametric methods

**Key references**:
- van der Vaart & Wellner (1996) - Weak Convergence and Empirical Processes

### 5. Information Theory (Fano, Assouad)
**Core idea**: Information-theoretic lower bounds
- **Fano's inequality**: H(θ|X) ≥ log|Θ| - I(θ;X) - 1
- **Assouad's lemma**: Hypercube construction
- **Le Cam's method**: Two-point testing

**Applications**: Proving minimax lower bounds

**Key references**:
- Yu (1997) - Assouad, Fano, and Le Cam
- Tsybakov (2009) - Introduction to Nonparametric Estimation

---

## Proof Techniques Toolbox

### Concentration Inequalities

**Hoeffding's Inequality**:
```
P(|X̄ - μ| ≥ t) ≤ 2 exp(-2nt²/(b-a)²)
```
Use case: Bounded random variables

**Bernstein's Inequality**:
```
P(|X̄ - μ| ≥ t) ≤ 2 exp(-nt²/(2σ² + 2bt/3))
```
Use case: Sub-exponential tails

**McDiarmid's Inequality**:
```
P(|f(X₁,...,Xₙ) - E[f]| ≥ t) ≤ 2 exp(-2t²/Σcᵢ²)
```
Use case: Functions with bounded differences

**Usage scenarios**:
- Proving estimator concentration
- Oracle inequalities
- Deviation bounds

### Empirical Process Techniques

**Symmetrization**:
```
E[sup_f |Pₙf - Pf|] ≤ 2E[sup_f |Pₙf - P'ₙf|]
```

**Chaining**:
```
E[sup_{f∈F} |Pₙf|] ≤ C∫₀^∞ √log N(ε, F, L₂(Pₙ)) dε
```

**Usage scenarios**:
- Uniform convergence
- M-estimation consistency
- Donsker theorems

### Information-Theoretic Lower Bounds

**Fano's Method**:
```
1. Construct parameter set {θ₁,...,θₘ} with large m
2. Compute KL divergence: KL(Pθᵢ || Pθⱼ) ≤ I
3. Apply Fano: P(error) ≥ 1 - (I + log 2)/log m
```

**Assouad's Lemma**:
```
1. Construct hypercube {θᵥ : v ∈ {0,1}ᵈ}
2. Ensure ||θᵤ - θᵥ|| ≥ Δ when Hamming distance = 1
3. Lower bound: Rₙ ≥ cΔ²d × avg error in single coordinate testing
```

**Le Cam's Two-Point Method**:
```
1. Construct two hypotheses H₀: θ = θ₀ vs H₁: θ = θ₁
2. Compute total variation: TV(P₀, P₁) = 1 - exp(-KL/2)
3. Lower bound: minimax risk ≥ (||θ₀-θ₁||²/4) × (1 - TV)
```

**Usage scenarios**:
- Proving minimax lower bounds
- Establishing fundamental limits
- Proving optimality of methods

### Change of Measure

**Girsanov-type arguments**:
```
dQ/dP = exp(stuff)
Use Q for favorable analysis, then change back
```

**Usage scenarios**:
- Sequential analysis
- Change point detection
- Testing with nuisance parameters

---

## Methodological Standards

### Three Levels of Theoretical Contribution

#### Gold Standard (Top-tier contribution)
✅ **Must include**:
1. **New phenomenon identification**: Discover new statistical phenomena
2. **Minimax rate derivation**: Derive the minimax rate
3. **Lower bound proof**: Prove lower bound (Fano/Assouad/Le Cam)
4. **Optimal procedure**: Construct a method that achieves the rate
5. **Phase transition characterization**: Characterize fundamental limits

**Example**: Donoho & Johnstone (1994) on wavelet thresholding
- Phenomenon: Near-ideal spatial adaptation
- Rate: n^(-2β/(2β+1))
- Lower bound: Proved via oracle inequality
- Optimal: VisuShrink, SureShrink
- Phase transition: Risk ellipse geometry

#### Good (Excellent contribution)
✅ **Must include**:
1. **New method**: Propose a new method
2. **Rate analysis**: Analyze convergence rate
3. **Comparison**: Compare with existing methods
4. **Simulations**: Numerical evidence
5. **Assumptions justified**: Clearly explain assumptions

#### Acceptable
✅ **Must include**:
1. **Extension**: Extend known results to new settings
2. **Rigorous proof**: Rigorous proofs
3. **Non-trivial**: Non-obvious extension

---

## Publication Standards

### Top Journals (Tier 1)

**Annals of Statistics**
- Highest standard, requires major theoretical contribution
- Expectation: Minimax optimality + lower bound
- Review cycle: 6-12 months
- Typical article length: 30-50 pages

**Journal of the Royal Statistical Society Series B (JRSSB)**
- Emphasizes methodology with theory
- Expectation: New method + asymptotic properties
- Review cycle: 4-8 months

**Biometrika**
- Classical statistical theory
- Expectation: Rigorous asymptotic analysis

### Second Tier

**Electronic Journal of Statistics (EJS)**
- Open access, faster review
- Still requires rigorous proofs

**Bernoulli**
- Probability + Statistics interface

**Journal of Machine Learning Research (JMLR)**
- Accepts statistical learning theory

---

## Review Standards and Common Comments

### Major Concerns (Leading to rejection)

🚩 **No lower bound**
```
"The authors claim their method is optimal, but provide no lower bound.
Without a matching lower bound, we cannot assess optimality."
```
**How to avoid**: Always include a lower bound, or at least compare with known lower bounds

🚩 **Unrealistic assumptions**
```
"The assumption of sub-Gaussian errors with known variance is too strong.
Real data rarely satisfies this."
```
**How to avoid**:
- Clearly explain the necessity of assumptions
- Provide robustness analysis
- Consider weaker assumptions

🚩 **Proof has gaps**
```
"In the proof of Theorem 2, the authors claim that term T₃ = o(n⁻¹/²)
without justification. This is not obvious and requires proof."
```
**How to avoid**:
- Prove all non-trivial steps
- Use "By [standard result], we have..." with citations
- Provide detailed proofs in appendix

🚩 **Contribution unclear**
```
"This result follows easily from [existing paper] by standard techniques.
The contribution is incremental."
```
**How to avoid**:
- Clearly state novelty
- Explicitly compare with existing results
- Emphasize technical challenges

🚩 **Simulation contradicts theory**
```
"Figure 1 shows the estimator performs worse than predicted by Theorem 1.
This suggests the constants in the rate may be wrong."
```
**How to avoid**:
- Simulations should be consistent with theory
- Explain discrepancies (constants, finite sample effects)
- Don't cherry-pick results

### Minor Concerns (Requiring revision)

⚠️ **Notation inconsistent**
```
"The authors use θ for both the parameter and an angle in Section 3."
```

⚠️ **Some edge cases not discussed**
```
"What happens when p = n? The theorem assumes p < n but many
applications have p ≥ n."
```

⚠️ **Comparison incomplete**
```
"The authors compare with [Method A] but not [Method B] which is
more relevant."
```

⚠️ **Writing unclear**
```
"The sentence 'It follows that...' on line 234 is ambiguous."
```

### Positive Signals (Leading to acceptance)

✅ **Novel phenomenon**
```
"The identification of the phase transition boundary is new and insightful."
```

✅ **Tight bounds**
```
"The matching upper and lower bounds establish the exact minimax rate."
```

✅ **Innovative technique**
```
"The proof technique using empirical process theory is novel and
may find applications elsewhere."
```

✅ **Well-positioned**
```
"The paper clearly positions itself in the literature and addresses
a genuine gap."
```

---

## Notation and Terminology Conventions

### Standard Notation

**Samples and parameters**:
- `n`: Sample size
- `p`: Dimension
- `d`: Dimension (sometimes used for spatial dimension)
- `θ`: Parameter (typically a vector)
- `θ₀`: True parameter
- `θ̂`: Estimator

**Distributions and expectations**:
- `P`, `Q`: Probability measures
- `P_θ`: Parameterized distribution
- `E_θ`: Expectation under P_θ
- `X ~ P`: X follows distribution P

**Risk and loss**:
- `L(θ, d)`: Loss function
- `R(θ, δ)`: Risk of estimator δ
- `ℓ_p` norms: `||·||_p`

**Sequence notation**:
- `a_n = O(b_n)`: a_n ≤ Cb_n for some constant C
- `a_n = o(b_n)`: a_n/b_n → 0
- `a_n ≍ b_n`: a_n = Θ(b_n), i.e., C₁b_n ≤ a_n ≤ C₂b_n
- `a_n ≲ b_n`: a_n ≤ Cb_n (weak inequality)

**Probability notation**:
- `P(event)`: Probability
- `Pₙ`: Empirical measure Pₙf = (1/n)Σf(Xᵢ)
- `Gₙ`: Empirical process Gₙf = √n(Pₙ - P)f

### Domain-Specific Terms

**Chinese → English Mapping**:
- 估計量 → Estimator
- 收斂速度 → Rate of convergence
- 漸近效率 → Asymptotic efficiency
- 極小極大最優 → Minimax optimal
- 自適應 → Adaptive
- 稀疏性 → Sparsity
- 正則化 → Regularization

---

## Typical Research Problem Patterns

### Pattern 1: Estimation Problem

**Structure**:
1. **Setup**: Observe X ~ P_θ, want to estimate θ
2. **Loss**: L(θ, θ̂) = ||θ - θ̂||²
3. **Goal**: Characterize minimax risk inf_θ̂ sup_θ E[L(θ, θ̂)]

**Key Questions**:
- What is the minimax rate?
- Can we achieve it adaptively?
- What is the price of adaptation?

**Example**: Nonparametric regression
- Observe: Yᵢ = f(xᵢ) + εᵢ
- Estimate: f ∈ Σ(β, L) (Hölder class)
- Rate: n^(-2β/(2β+d))

### Pattern 2: Testing Problem

**Structure**:
1. **Hypotheses**: H₀: θ ∈ Θ₀ vs H₁: θ ∈ Θ₁
2. **Separation**: inf{||θ₀ - θ₁|| : θ₀ ∈ Θ₀, θ₁ ∈ Θ₁} = Δₙ
3. **Goal**: Characterize detection boundary Δₙ

**Key Questions**:
- At what separation Δₙ can we distinguish H₀ from H₁?
- Is there a phase transition?
- What is the optimal test statistic?

**Example**: Sparse signal detection
- H₀: θ = 0 vs H₁: ||θ||₀ ≤ s, ||θ||₂² ≥ Δₙ
- Detection boundary: Δₙ = √(s log(p/s)/n)

### Pattern 3: Adaptation Problem

**Structure**:
1. **Unknown smoothness**: f ∈ Σ(β, L) but β unknown
2. **Goal**: Construct estimator that adapts to unknown β
3. **Price**: Compare adaptive rate to oracle rate

**Key Questions**:
- Can we achieve oracle rate without knowing β?
- What is the price of adaptation (log factor)?
- Is full adaptation possible?

---

## Research Proposal Evaluation Standards

### Theorist's Checklist

When proposing a new hypothesis, self-check:

- [ ] **Problem well-defined**: Parameter space, model, objectives clear
- [ ] **Novelty clear**: What distinguishes it from existing literature
- [ ] **Minimax formulation**: Can it be formulated as a minimax problem
- [ ] **Rate prediction**: What is the expected minimax rate
- [ ] **Lower bound strategy**: How to prove the lower bound (Fano? Assouad? Le Cam?)
- [ ] **Upper bound strategy**: How to construct an estimator that achieves the rate
- [ ] **Assumptions reasonable**: Are the assumptions too strong
- [ ] **Edge cases identified**: Boundary cases (p=n? s=0? β=∞?)

### Experimentalist's Checklist

Assessing feasibility:

- [ ] **Computational complexity**: O(?) - Is it practically feasible
- [ ] **Algorithm well-defined**: Is there a clear algorithm
- [ ] **Simulation plan**: How to verify the theoretical rate
- [ ] **Comparison baselines**: Which existing methods to compare with
- [ ] **Data availability**: What data is needed for verification
- [ ] **Implementation difficulty**: Assessment of implementation difficulty

### Methodologist's Checklist

Methodological review:

- [ ] **Proof technique identified**: What techniques are used for proof
- [ ] **Technical conditions verified**: Check regularity conditions
- [ ] **Constants tracked**: Track not just rates, but also constants
- [ ] **Uniform vs pointwise**: Are results uniform or pointwise
- [ ] **Adaptation considered**: Is the adaptive setting considered
- [ ] **Reproducibility**: Can the proof be reproduced by others

---

## Common Pitfalls (Red Flags)

### 🚩 Theoretical Pitfalls

**"Hand-waving" proof**:
```
❌ "By standard concentration inequalities, we have..."
   (Doesn't specify which inequality, whether conditions are satisfied)

✅ "By Hoeffding's inequality (since εᵢ are bounded in [-1,1]),
   P(|X̄ - μ| ≥ t) ≤ 2exp(-2nt²)"
```

**Missing constants**:
```
❌ "The rate is O(n⁻¹/²)"
   (Constants may be large, leading to poor practical performance)

✅ "||θ̂ - θ||² ≤ (σ²p/n)(1 + o(1))"
   (Provides leading constant)
```

**Claiming optimality without proof**:
```
❌ "Our method is minimax optimal."
   (No lower bound)

✅ "Our method achieves the minimax rate n⁻²ᵝ/(²ᵝ⁺ᵈ),
   matching the lower bound of [Ref]."
```

### 🚩 Methodological Pitfalls

**Over-claiming**:
```
❌ "Our method works for all distributions."
   (Usually requires some regularity)

✅ "Our method works for distributions satisfying [specific condition]."
```

**Ignoring computational cost**:
```
❌ Proposing NP-hard optimization without discussing computational aspect

✅ "While the optimization is NP-hard in worst case, we show that
   under [condition], a polynomial-time algorithm achieves the rate."
```

### 🚩 Experimental Pitfalls

**Unrealistic simulations**:
```
❌ Only test on Gaussian noise when theory assumes sub-Gaussian

✅ Test on: Gaussian, t-distribution, contaminated Gaussian
```

**Cherry-picked results**:
```
❌ Only show cases where your method wins

✅ Show failure modes and explain why
```

---

## Recommended Learning Path

### Beginner Must-Reads (Fundamentals)
1. **Van der Vaart (1998)** - Asymptotic Statistics
2. **Lehmann & Casella (1998)** - Theory of Point Estimation
3. **Wasserman (2006)** - All of Nonparametric Statistics

### Advanced (Minimax Theory)
1. **Tsybakov (2009)** - Introduction to Nonparametric Estimation
2. **Giné & Nickl (2016)** - Mathematical Foundations of Infinite-Dimensional Statistical Models

### High-Dimensional Statistics
1. **Wainwright (2019)** - High-Dimensional Statistics: A Non-Asymptotic Viewpoint
2. **Bühlmann & van de Geer (2011)** - Statistics for High-Dimensional Data

### Empirical Processes
1. **Van der Vaart & Wellner (1996)** - Weak Convergence and Empirical Processes

---

## Summary: Core Principles

Golden rules for statistical theory research:

1. **Minimax is the gold standard** - Always think about worst-case
2. **Lower bounds matter** - Don't claim optimality without proof
3. **Constants matter** - Not just rates
4. **Assumptions must be justified** - And tested
5. **Rigorous proofs required** - No hand-waving
6. **Comparison with literature essential** - Position your work
7. **Reproducibility paramount** - Others should be able to verify

**Keys to success in this field**:
- Solid mathematical foundation (measure theory, functional analysis)
- Innovative proof techniques
- Deep understanding of classical results
- Connection to applied problems

---

**This domain knowledge should be injected into all agents working on statistical theory projects.**

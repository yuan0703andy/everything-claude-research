# Causal Inference Rules

**Foundational Reference**: Hernán & Robins "Causal Inference: What If" (2020)

---

## Core Principles

### The Fundamental Problem

**You can NEVER observe both** Y(1) and Y(0) for the same unit:
- Y(1) = Outcome if treated
- Y(0) = Outcome if not treated

**Causal effect** = Y(1) - Y(0) [unobservable for individual units]

**What we CAN estimate**: E[Y(1) - Y(0)] = ATE (Average Treatment Effect)

---

## The Causal Inference Checklist

Before claiming causality, verify ALL:

- [ ] Clear causal question defined
- [ ] Target trial specified (if observational)
- [ ] Causal diagram (DAG) drawn
- [ ] Identification assumptions stated
- [ ] Positivity assumption checked
- [ ] Consistency assumption justified
- [ ] No interference assumption verified
- [ ] Estimation method appropriate
- [ ] Sensitivity analysis conducted
- [ ] Results interpreted causally (with caveats)

---

## Step 1: Define the Causal Question

### WRONG: Vague Questions

```python
# BAD: Association question masquerading as causal
"Does exercise correlate with longevity?"

# BAD: Unclear treatment
"Does education affect income?"
# → What education? High school vs college? Years? Quality?
```

### CORRECT: Well-Defined Causal Contrasts

```python
# GOOD: Precise treatment definition
"""
Causal Question:
What is the average causal effect of completing a 4-year college degree
vs stopping after high school on annual income at age 35?

Treatment: A = 1 if completed 4-year degree, 0 if stopped after HS
Outcome: Y = annual income (USD) at age 35
Population: US residents born 1985-1990
"""

# Document this BEFORE analysis
CAUSAL_QUESTION = """
Treatment: Exercise ≥150 min/week vs <150 min/week
Outcome: All-cause mortality (10-year follow-up)
Target population: Adults aged 40-65, free of CVD at baseline
Comparison: What would happen if everyone exercised ≥150 min/week
            vs if everyone exercised <150 min/week?
"""
```

---

## Step 2: Draw Your DAG (Directed Acyclic Graph)

### Mandatory for EVERY Causal Analysis

```python
import networkx as nx
import matplotlib.pyplot as plt

# Example: Effect of exercise on mortality
def draw_dag():
    """
    Draw causal DAG for exercise → mortality.

    Nodes:
    - A: Exercise (treatment)
    - Y: Mortality (outcome)
    - L: Confounders (age, baseline health, SES)
    - U: Unmeasured confounders
    """
    G = nx.DiGraph()

    # Add edges (arrows in DAG)
    G.add_edges_from([
        ('Age', 'Exercise'),
        ('Age', 'Mortality'),
        ('Baseline_Health', 'Exercise'),
        ('Baseline_Health', 'Mortality'),
        ('SES', 'Exercise'),
        ('SES', 'Mortality'),
        ('Exercise', 'Mortality'),
        ('Unmeasured', 'Exercise'),  # Motivation, genetics
        ('Unmeasured', 'Mortality')
    ])

    # Layout
    pos = {
        'Exercise': (0, 0),
        'Mortality': (2, 0),
        'Age': (1, 1),
        'Baseline_Health': (0, 1),
        'SES': (0.5, 1.5),
        'Unmeasured': (1, -1)
    }

    plt.figure(figsize=(10, 6))
    nx.draw(G, pos, with_labels=True, node_color='lightblue',
            node_size=3000, font_size=10, arrows=True)
    plt.title("Causal DAG: Exercise → Mortality")
    plt.savefig('figures/dag_exercise_mortality.png', dpi=300, bbox_inches='tight')
    plt.close()

    return G

# CRITICAL: Use DAG to identify confounders
dag = draw_dag()
```

### Identify Confounders from DAG

**Confounder** = Variable that affects BOTH treatment and outcome

```python
def identify_confounders(dag, treatment='Exercise', outcome='Mortality'):
    """
    Identify minimal adjustment set from DAG.

    Confounders are variables on BACKDOOR paths:
    - Path from treatment to outcome that enters treatment via arrow INTO treatment
    """
    # Use backdoor criterion (simplified)
    # In practice, use dagitty or DoWhy library

    confounders = []
    for node in dag.nodes():
        if node != treatment and node != outcome:
            # Check if node is on backdoor path
            if (dag.has_edge(node, treatment) and
                dag.has_edge(node, outcome)):
                confounders.append(node)

    print("Confounders to adjust for:")
    for conf in confounders:
        print(f"  - {conf}")

    return confounders

confounders = identify_confounders(dag)
# Output: Age, Baseline_Health, SES
# Note: Unmeasured is also a confounder (problem!)
```

---

## Step 3: State Identification Assumptions

### The Big Three Assumptions

#### 1. Exchangeability (No Unmeasured Confounding)

**Assumption**: Y(a) ⊥ A | L

"Conditional on measured covariates L, treatment assignment is as-if random"

```python
# CRITICAL: This is UNTESTABLE from data
# You must argue based on:
# - Subject matter knowledge
# - DAG completeness
# - Sensitivity analysis

EXCHANGEABILITY_ASSUMPTION = """
Assumption: Conditional on measured covariates (age, baseline health, SES),
treatment assignment (exercise level) is independent of potential outcomes.

Justification:
- We measured all major confounders from epidemiological literature
- DAG reviewed by domain experts
- Sensitivity analysis to assess unmeasured confounding (see below)

Limitations:
- Cannot measure motivation, genetics perfectly
- Residual confounding likely present
- Results should be interpreted cautiously
"""
```

#### 2. Positivity (Overlap)

**Assumption**: P(A=a | L=l) > 0 for all l with P(L=l) > 0

"Every covariate pattern has non-zero probability of receiving each treatment level"

```python
import numpy as np
from scipy.stats import gaussian_kde

def check_positivity(treatment, propensity_scores, plot=True):
    """
    Check positivity assumption via propensity score overlap.

    Args:
        treatment: Binary treatment (n,)
        propensity_scores: P(A=1|L) estimated from data (n,)
        plot: Whether to plot distributions

    Returns:
        dict with diagnostics
    """
    treated = propensity_scores[treatment == 1]
    control = propensity_scores[treatment == 0]

    # Check extremes
    n_extreme = np.sum((propensity_scores < 0.01) | (propensity_scores > 0.99))
    pct_extreme = 100 * n_extreme / len(propensity_scores)

    # Check non-overlap regions
    treated_min, treated_max = treated.min(), treated.max()
    control_min, control_max = control.min(), control.max()
    overlap_min = max(treated_min, control_min)
    overlap_max = min(treated_max, control_max)

    n_nonoverlap = np.sum((propensity_scores < overlap_min) |
                          (propensity_scores > overlap_max))
    pct_nonoverlap = 100 * n_nonoverlap / len(propensity_scores)

    diagnostics = {
        'n_extreme': n_extreme,
        'pct_extreme': pct_extreme,
        'pct_nonoverlap': pct_nonoverlap,
        'overlap_range': (overlap_min, overlap_max)
    }

    # Warnings
    if pct_extreme > 5:
        print(f"⚠️  WARNING: {pct_extreme:.1f}% extreme propensity scores")
        print("   Consider trimming or alternative methods")

    if pct_nonoverlap > 10:
        print(f"⚠️  WARNING: {pct_nonoverlap:.1f}% in non-overlap regions")
        print("   Positivity assumption violated")

    if plot:
        import matplotlib.pyplot as plt
        plt.figure(figsize=(10, 6))
        plt.hist(treated, bins=30, alpha=0.5, label='Treated', density=True)
        plt.hist(control, bins=30, alpha=0.5, label='Control', density=True)
        plt.axvline(overlap_min, color='red', linestyle='--',
                   label='Overlap range')
        plt.axvline(overlap_max, color='red', linestyle='--')
        plt.xlabel('Propensity Score')
        plt.ylabel('Density')
        plt.legend()
        plt.title('Positivity Check: Propensity Score Overlap')
        plt.savefig('figures/positivity_check.png', dpi=300)
        plt.close()

    return diagnostics
```

#### 3. Consistency

**Assumption**: If A_i = a, then Y_i = Y_i(a)

"The treatment you received is the treatment you were assigned"

**Why it matters**:

```python
# WRONG: Treatment definition too vague
treatment = 'exercise'  # What kind? How much? When?

# If 'exercise' means different things to different people,
# consistency is violated

# CORRECT: Well-defined intervention
TREATMENT_DEFINITION = """
Treatment A = 1 if:
  - Aerobic exercise ≥150 minutes/week
  - Measured via accelerometer (objective)
  - Sustained for ≥6 months
  - Moderate-vigorous intensity (≥3 METs)

Treatment A = 0 if:
  - <150 minutes/week aerobic exercise
  - Or no accelerometer data (excluded)
"""
```

---

## Step 4: Estimate Causal Effects

### Method 1: Regression Adjustment

```python
import numpy as np
from sklearn.linear_model import LinearRegression

def estimate_ate_regression(y, treatment, confounders):
    """
    Estimate ATE via outcome regression.

    Args:
        y: Outcome (n,)
        treatment: Binary treatment (n,)
        confounders: Confounding variables (n, p)

    Returns:
        ate_estimate, standard_error
    """
    # Combine treatment + confounders
    X = np.column_stack([treatment, confounders])

    # Fit outcome model
    model = LinearRegression().fit(X, y)

    # ATE = coefficient on treatment
    ate = model.coef_[0]

    # Compute standard error (simplified)
    predictions = model.predict(X)
    residuals = y - predictions
    n = len(y)
    p = X.shape[1]

    # Variance estimate
    sigma2 = np.sum(residuals**2) / (n - p)
    var_beta = sigma2 * np.linalg.inv(X.T @ X)
    se_ate = np.sqrt(var_beta[0, 0])

    # 95% CI
    ci_lower = ate - 1.96 * se_ate
    ci_upper = ate + 1.96 * se_ate

    print(f"ATE (Regression): {ate:.3f} (95% CI: [{ci_lower:.3f}, {ci_upper:.3f}])")

    return ate, se_ate
```

### Method 2: Inverse Probability Weighting (IPW)

```python
from sklearn.linear_model import LogisticRegression

def estimate_ate_ipw(y, treatment, confounders):
    """
    Estimate ATE via inverse probability weighting.

    Args:
        y: Outcome (n,)
        treatment: Binary treatment (n,)
        confounders: Confounding variables (n, p)

    Returns:
        ate_estimate, standard_error
    """
    # Step 1: Estimate propensity scores
    ps_model = LogisticRegression(max_iter=1000).fit(confounders, treatment)
    propensity = ps_model.predict_proba(confounders)[:, 1]

    # Step 2: Compute weights
    weights = np.where(treatment == 1,
                      1 / propensity,
                      1 / (1 - propensity))

    # Check weight diagnostics
    print(f"Weight range: [{weights.min():.2f}, {weights.max():.2f}]")
    print(f"Weight mean: {weights.mean():.2f}")

    if weights.max() > 100:
        print("⚠️  WARNING: Extreme weights detected")
        print("   Consider weight truncation or doubly robust method")

    # Step 3: Weighted means
    ate = np.average(y[treatment == 1], weights=weights[treatment == 1]) - \
          np.average(y[treatment == 0], weights=weights[treatment == 0])

    # Standard error (simplified - use bootstrap in practice)
    n1 = np.sum(treatment == 1)
    n0 = np.sum(treatment == 0)
    var1 = np.var(y[treatment == 1])
    var0 = np.var(y[treatment == 0])
    se_ate = np.sqrt(var1/n1 + var0/n0)

    ci_lower = ate - 1.96 * se_ate
    ci_upper = ate + 1.96 * se_ate

    print(f"ATE (IPW): {ate:.3f} (95% CI: [{ci_lower:.3f}, {ci_upper:.3f}])")

    return ate, se_ate, propensity
```

### Method 3: Doubly Robust Estimation (Recommended)

```python
def estimate_ate_dr(y, treatment, confounders):
    """
    Doubly robust ATE estimation.

    Combines outcome regression + IPW.
    Robust to misspecification of EITHER model (not both).

    Args:
        y: Outcome (n,)
        treatment: Binary treatment (n,)
        confounders: Confounding variables (n, p)

    Returns:
        ate_estimate, standard_error
    """
    n = len(y)

    # Step 1: Fit propensity score model
    ps_model = LogisticRegression(max_iter=1000).fit(confounders, treatment)
    propensity = ps_model.predict_proba(confounders)[:, 1]

    # Step 2: Fit outcome models (separate for treated/control)
    treated_idx = (treatment == 1)
    control_idx = (treatment == 0)

    # Outcome model for treated
    model_1 = LinearRegression().fit(confounders[treated_idx], y[treated_idx])
    mu1 = model_1.predict(confounders)

    # Outcome model for control
    model_0 = LinearRegression().fit(confounders[control_idx], y[control_idx])
    mu0 = model_0.predict(confounders)

    # Step 3: Doubly robust estimator
    weights_1 = treatment / propensity
    weights_0 = (1 - treatment) / (1 - propensity)

    pseudo_outcome = weights_1 * (y - mu1) + mu1 - \
                     weights_0 * (y - mu0) - mu0

    ate = np.mean(pseudo_outcome)

    # Standard error via bootstrap (simplified)
    se_ate = np.std(pseudo_outcome) / np.sqrt(n)

    ci_lower = ate - 1.96 * se_ate
    ci_upper = ate + 1.96 * se_ate

    print(f"ATE (Doubly Robust): {ate:.3f} (95% CI: [{ci_lower:.3f}, {ci_upper:.3f}])")

    return ate, se_ate
```

---

## Step 5: Sensitivity Analysis (MANDATORY)

### Test Robustness to Unmeasured Confounding

```python
def sensitivity_analysis_evalue(ate, ci_lower):
    """
    Compute E-value: Minimum strength of unmeasured confounding
    needed to explain away the observed effect.

    Reference: VanderWeele & Ding (2017)

    Args:
        ate: Point estimate (on risk ratio scale)
        ci_lower: Lower bound of 95% CI

    Returns:
        E-value for point estimate and CI
    """
    # Convert to risk ratio if needed
    # Assuming ate is already RR or can be approximated

    # E-value formula
    rr = ate
    e_value_point = rr + np.sqrt(rr * (rr - 1))

    rr_lower = ci_lower
    e_value_ci = rr_lower + np.sqrt(rr_lower * (rr_lower - 1))

    print("E-value Sensitivity Analysis:")
    print("-" * 60)
    print(f"Point estimate: {e_value_point:.2f}")
    print(f"Lower CI bound: {e_value_ci:.2f}")
    print()
    print("Interpretation:")
    print(f"An unmeasured confounder would need to be associated with")
    print(f"BOTH treatment and outcome by a risk ratio of {e_value_ci:.2f}")
    print(f"to fully explain away the observed effect.")
    print()
    print("Example: If E-value = 2.0")
    print("  → Unmeasured confounder would need to double risk of")
    print("    treatment AND double risk of outcome")

    return e_value_point, e_value_ci


def sensitivity_analysis_rosenbaum(ate, se, gamma_range=np.array([1.0, 1.5, 2.0, 3.0])):
    """
    Rosenbaum bounds sensitivity analysis.

    Tests robustness of inference to hidden bias of strength Γ (gamma).

    Args:
        ate: Point estimate
        se: Standard error
        gamma_range: Range of sensitivity parameters

    Returns:
        DataFrame with sensitivity results
    """
    import pandas as pd

    results = []
    for gamma in gamma_range:
        # Adjust bounds for hidden bias of strength gamma
        # (simplified version - full Rosenbaum bounds more complex)
        ci_lower = ate - gamma * 1.96 * se
        ci_upper = ate + gamma * 1.96 * se
        includes_zero = (ci_lower <= 0 <= ci_upper)

        results.append({
            'gamma': gamma,
            'ci_lower': ci_lower,
            'ci_upper': ci_upper,
            'includes_null': includes_zero
        })

    df = pd.DataFrame(results)

    print("Rosenbaum Bounds Sensitivity Analysis:")
    print("-" * 60)
    print(df.to_string(index=False))
    print()
    print("Interpretation:")
    print("Γ = hidden bias strength (odds ratio scale)")
    print("If CI includes null at Γ=2.0:")
    print("  → A hidden confounder twice as likely to cause treatment")
    print("    would make effect not significant")

    return df
```

---

## Common Mistakes and How to Avoid Them

### Mistake 1: Controlling for Mediators

```python
# WRONG: Control for mediator
"""
DAG: Treatment → Mediator → Outcome
     Treatment → Outcome

If you control for Mediator, you BLOCK the causal path!
"""

# Example: Effect of education on income
# WRONG:
X = np.column_stack([education, occupation])  # Occupation is mediator!
model = LinearRegression().fit(X, income)
effect = model.coef_[0]  # WRONG: This is NOT total effect

# CORRECT: Do NOT control for mediators
X = np.column_stack([education, baseline_confounders])  # No occupation
model = LinearRegression().fit(X, income)
effect = model.coef_[0]  # Total effect of education

# If you want to understand mechanism, use mediation analysis separately
```

### Mistake 2: Controlling for Colliders

```python
# WRONG: Control for collider
"""
DAG:  Treatment → Collider ← Outcome
      Treatment → Outcome

If you control for Collider, you CREATE spurious association!
"""

# Example: Effect of exercise on mortality
# WRONG:
X = np.column_stack([exercise, hospital_visits])  # Hospital visits is collider!
# Hospital visits caused by BOTH poor health AND exercise injuries
model = LinearRegression().fit(X, mortality)
effect = model.coef_[0]  # BIASED by collider stratification bias

# CORRECT: Do NOT control for colliders
X = np.column_stack([exercise, baseline_confounders])  # No hospital visits
model = LinearRegression().fit(X, mortality)
effect = model.coef_[0]  # Unbiased (assuming no unmeasured confounding)
```

### Mistake 3: Table 2 Fallacy

```python
# WRONG: Interpreting coefficients causally without proper adjustment
"""
Table 2: Multivariable regression
Variable      | Coefficient | p-value
Treatment     | 0.45        | <0.001  # Is this causal? NO!
Confounder 1  | 0.23        | 0.03    # Is this causal? NO!
Confounder 2  | -0.15       | 0.12    # Is this causal? NO!

Problem: Coefficients on confounders have NO causal interpretation
         Only coefficient on treatment (maybe) has causal meaning
         And only if ALL confounders adjusted
"""

# CORRECT: Report only treatment effect as causal
print("Causal effect of treatment: 0.45 (95% CI: [0.30, 0.60])")
print("Note: Coefficients on confounders are not causally interpretable")
```

---

## Causal Inference Workflow Template

```python
"""
Template for causal analysis.
Copy this and fill in for each analysis.
"""

# ============================================================================
# 1. DEFINE CAUSAL QUESTION
# ============================================================================

CAUSAL_QUESTION = """
Treatment: [Clear definition]
Outcome: [Clear definition]
Population: [Clear definition]
Comparison: [Clear counterfactual]
"""

# ============================================================================
# 2. DRAW DAG
# ============================================================================

def draw_causal_dag():
    """Draw and save causal DAG."""
    # ... code here ...
    pass

# ============================================================================
# 3. STATE ASSUMPTIONS
# ============================================================================

ASSUMPTIONS = """
1. Exchangeability:
   [Justification for no unmeasured confounding]

2. Positivity:
   [Check covariate overlap - see figures/positivity_check.png]

3. Consistency:
   [Justify well-defined intervention]
"""

# ============================================================================
# 4. ESTIMATE CAUSAL EFFECT
# ============================================================================

# Load data
treatment, outcome, confounders = load_data()

# Estimate using multiple methods
ate_reg, se_reg = estimate_ate_regression(outcome, treatment, confounders)
ate_ipw, se_ipw, ps = estimate_ate_ipw(outcome, treatment, confounders)
ate_dr, se_dr = estimate_ate_dr(outcome, treatment, confounders)

# Check consistency across methods
print("Method comparison:")
print(f"Regression:      {ate_reg:.3f} ± {se_reg:.3f}")
print(f"IPW:             {ate_ipw:.3f} ± {se_ipw:.3f}")
print(f"Doubly Robust:   {ate_dr:.3f} ± {se_dr:.3f}")

# ============================================================================
# 5. SENSITIVITY ANALYSIS
# ============================================================================

# E-value
e_value_point, e_value_ci = sensitivity_analysis_evalue(ate_dr, ate_dr - 1.96*se_dr)

# Rosenbaum bounds
rosenbaum_results = sensitivity_analysis_rosenbaum(ate_dr, se_dr)

# ============================================================================
# 6. REPORT RESULTS
# ============================================================================

RESULTS_SUMMARY = f"""
Causal Effect Estimate:
- ATE: {ate_dr:.3f} (95% CI: [{ate_dr-1.96*se_dr:.3f}, {ate_dr+1.96*se_dr:.3f}])
- Method: Doubly robust estimation
- Standard error: {se_dr:.3f}

Sensitivity Analysis:
- E-value: {e_value_ci:.2f}
- Interpretation: [Explain what unmeasured confounding would be needed]

Caveats:
- [List limitations]
- [Residual confounding concerns]
- [External validity concerns]
"""

print(RESULTS_SUMMARY)
```

---

## Further Reading

### Essential Papers
1. Hernán & Robins (2020). "Causal Inference: What If" [Free online]
2. Pearl (2009). "Causality: Models, Reasoning, and Inference"
3. Imbens & Rubin (2015). "Causal Inference for Statistics, Social, and Biomedical Sciences"
4. VanderWeele (2015). "Explanation in Causal Inference"

### Python Libraries
- **DoWhy**: Microsoft's causal inference library (DAG-based)
- **CausalML**: Uber's library for heterogeneous treatment effects
- **EconML**: Microsoft's library for causal ML
- **CausalImpact**: Google's library for time series causal inference

### When in Doubt
- Consult with causal inference expert
- Pre-register your analysis plan
- Report ALL assumptions explicitly
- Conduct sensitivity analyses
- Interpret results cautiously

Remember: **Correlation ≠ Causation**. Causal claims require causal assumptions.

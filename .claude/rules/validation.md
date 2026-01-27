# Validation Requirements

## Minimum Validation Standards

ALL analyses must include:
1. **Data Validation** - Check data quality and integrity
2. **Assumption Validation** - Verify statistical assumptions
3. **Robustness Checks** - Sensitivity analyses
4. **Reproducibility Validation** - Results can be reproduced

## Validation-Driven Research

MANDATORY workflow:
1. Define validation criteria FIRST (RED)
2. Run analysis without validation - should have warnings
3. Add validation checks (GREEN)
4. Run with validation - should pass
5. Document validation results (VERIFY)
6. Check pass@k metrics (80%+)

## Data Validation Checklist

Before any analysis:

```r
# ALWAYS check these
stopifnot(nrow(data) > 0)                    # Non-empty
stopifnot(all(!is.na(data$outcome)))         # No missing outcomes
stopifnot(all(is.finite(data$predictor)))    # No infinite values
stopifnot(nrow(data) == length(unique(data$id)))  # No duplicates

# Document data properties
cat("Sample size:", nrow(data), "\n")
cat("Outcome range:", range(data$outcome), "\n")
cat("Missing data:", colSums(is.na(data)), "\n")
```

```python
# Python equivalent
assert len(data) > 0, "Empty dataset"
assert not data['outcome'].isna().any(), "Missing outcomes"
assert not np.isinf(data['predictor']).any(), "Infinite values"
assert len(data) == len(data['id'].unique()), "Duplicate IDs"

print(f"Sample size: {len(data)}")
print(f"Outcome range: {data['outcome'].min()}, {data['outcome'].max()}")
print(f"Missing data:\n{data.isna().sum()}")
```

## Assumption Validation

### Regression Models

```r
# ALWAYS check regression assumptions
model <- lm(y ~ x, data = data)

# 1. Linearity
plot(model, which = 1)  # Residuals vs Fitted

# 2. Normality
shapiro.test(residuals(model))
qqnorm(residuals(model))

# 3. Homoscedasticity
library(lmtest)
bptest(model)

# 4. Independence
durbinWatsonTest(model)

# Document violations
cat("Assumption checks:\n")
cat("  Shapiro-Wilk p-value:", shapiro.test(residuals(model))$p.value, "\n")
cat("  Breusch-Pagan p-value:", bptest(model)$p.value, "\n")
```

### Causal Inference

```r
# Check covariate balance
library(cobalt)
bal.tab(treatment ~ confounders, data = data)

# Check positivity
prop.table(table(data$treatment, data$high_risk_strata))

# Check SUTVA violations
# [Your domain-specific checks]
```

## Robustness Validation

ALWAYS run sensitivity analyses:

```r
# Main analysis
main_result <- lm(y ~ x + controls, data = data)

# Sensitivity: Different specifications
sensitivity_1 <- lm(y ~ x + controls + additional, data = data)
sensitivity_2 <- lm(y ~ x + controls, data = data[data$subsample, ])
sensitivity_3 <- rlm(y ~ x + controls, data = data)  # Robust regression

# Compare results
stargazer(main_result, sensitivity_1, sensitivity_2, sensitivity_3,
          type = "text",
          title = "Main and Sensitivity Analyses")
```

## Reproducibility Validation

Before marking analysis complete:

```bash
# 1. Clean environment test
Rscript --vanilla analysis/run_all.R

# 2. Fresh conda environment
conda create -n test python=3.10
conda activate test
pip install -r requirements.txt
python analysis/run_all.py

# 3. Check outputs match
diff results/main_results.csv results/main_results_check.csv
```

## Validation Documentation

ALWAYS document in analysis script:

```r
# === VALIDATION REPORT ===
# Date: 2024-01-27
# Analyst: [Name]
#
# Data Validation:
#   - Sample size: 1000
#   - Missing data: 0%
#   - Outliers: 3 observations (>3 SD), retained
#
# Assumption Checks:
#   - Normality: Shapiro p = 0.12 (pass)
#   - Homoscedasticity: BP p = 0.08 (pass)
#   - Independence: DW = 1.95 (pass)
#
# Robustness:
#   - Alternative specifications: 3 tested, results consistent
#   - Subsample analysis: Effect maintained (β = 0.52, p < 0.01)
#   - Robust regression: Similar effect (β = 0.49, p < 0.01)
#
# Reproducibility:
#   - Seed: 42
#   - R version: 4.3.1
#   - Package versions: See renv.lock
#   - Reproduced on: 2024-01-27 (passed)
# =======================
```

## Validation Criteria: pass@k

Use `/eval` framework:

```bash
# Define criteria
/eval define H-001

# Check validation metrics
/eval check H-001
```

Criteria for pass@k:
- **pass@1**: Single run passes all checks (≥90%)
- **pass@3**: 3 independent runs all pass (≥90%)
- **pass^3**: Replication, robustness, reproduction all pass (≥80%)

## Agent Support

- **Experimentalist** - Designs validation strategy
- **Methodologist** - Reviews validation rigor, runs eval-harness
- **Lab Manager** - Tracks validation status

## Common Validation Failures

### CRITICAL: Stop immediately
- Empty dataset after filtering
- All outcomes identical (no variation)
- Perfect multicollinearity
- Assumption violations with p < 0.001
- Completely non-reproducible results

### HIGH: Must address before proceeding
- >10% missing data without justification
- Significant assumption violations (p < 0.05)
- Results not robust to specifications
- Minor reproducibility issues

### MEDIUM: Document and consider
- Small assumption violations (0.05 < p < 0.10)
- Moderate sensitivity to specifications
- Minor data quality issues

## Pre-Publication Validation

Final checklist before writeup:

- [ ] All data validation checks passed
- [ ] All assumptions checked and documented
- [ ] ≥3 sensitivity analyses conducted
- [ ] Results reproduced on clean environment
- [ ] pass@3 ≥ 90%
- [ ] Validation report written
- [ ] Code runs without warnings/errors
- [ ] Random seeds documented
- [ ] Software versions recorded

## Validation Report Template

```markdown
# Validation Report: H-XXX

Date: YYYY-MM-DD
Analyst: [Name]

## Data Validation
- Sample size: [N]
- Missing data: [%]
- Outliers: [description and handling]
- Data quality issues: [list]

## Assumption Validation
- [Assumption 1]: [test result and interpretation]
- [Assumption 2]: [test result and interpretation]
- Violations: [describe and justify proceeding]

## Robustness Validation
- Specification 1: [result]
- Specification 2: [result]
- Consistency: [assessment]

## Reproducibility Validation
- Random seed: [value]
- Software: [versions]
- Reproduction date: [date]
- Status: [passed/failed]

## pass@k Metrics
- pass@1: [%]
- pass@3: [%]
- pass^3: [%]

## Conclusion
[Ready for publication / Needs revision / Failed]
```

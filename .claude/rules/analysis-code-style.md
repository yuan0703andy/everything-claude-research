# Analysis Code Style

## Reproducibility (CRITICAL)

ALWAYS ensure analyses can be reproduced:

```r
# WRONG: No seed, no documentation
results <- sample(data, 100)
model <- lm(y ~ x, data = results)

# CORRECT: Reproducible
set.seed(42)  # Fixed seed
# Sample 100 observations for pilot analysis
results <- sample(data, 100)
model <- lm(y ~ x, data = results)
```

```python
# WRONG: No seed
import random
sample = random.sample(data, 100)

# CORRECT: Reproducible
import random
random.seed(42)
sample = random.sample(data, 100)
```

## File Organization

MANY SMALL SCRIPTS > ONE LARGE SCRIPT:
- `01_load_data.R` - Data loading only
- `02_clean_data.R` - Data cleaning
- `03_analysis.R` - Primary analysis
- `04_sensitivity.R` - Sensitivity analyses
- `05_figures.R` - Figure generation
- `functions/` - Reusable functions

Typical limits:
- 200-300 lines for analysis scripts
- 500 max for main analysis
- Extract functions to `functions/` directory

## Error Handling

ALWAYS check data quality:

```r
# WRONG: Assume data is clean
model <- lm(y ~ x, data = data)

# CORRECT: Validate first
stopifnot(all(!is.na(data$y)))
stopifnot(all(!is.na(data$x)))
stopifnot(nrow(data) > 0)
model <- lm(y ~ x, data = data)
```

```python
# WRONG: No validation
model.fit(X, y)

# CORRECT: Check assumptions
assert not X.isna().any().any(), "X contains NA"
assert not y.isna().any(), "y contains NA"
assert len(X) > 0, "Empty dataset"
model.fit(X, y)
```

## Data Immutability

NEVER modify raw data files:
- Keep raw data in `data/raw/` (read-only)
- Save cleaned data to `data/processed/`
- Document all transformations

```r
# WRONG: Overwrite raw data
data <- read.csv("data/raw/study.csv")
data <- data[!is.na(data$outcome), ]
write.csv(data, "data/raw/study.csv")  # BAD!

# CORRECT: Save to processed
data_raw <- read.csv("data/raw/study.csv")
data_clean <- data_raw[!is.na(data_raw$outcome), ]
write.csv(data_clean, "data/processed/study_clean.csv")
```

## Code Quality Checklist

Before marking analysis complete:
- [ ] Random seed set and documented
- [ ] Data validation checks included
- [ ] No magic numbers (use named constants)
- [ ] Functions have clear names
- [ ] Complex logic has comments
- [ ] Results saved to `results/`
- [ ] Figures saved to `figures/`
- [ ] No absolute paths (use relative paths)
- [ ] All dependencies documented

## Documentation

ALWAYS document analysis steps:

```r
# WRONG: No context
fit <- lm(y ~ x1 + x2)

# CORRECT: Clear documentation
# Main hypothesis test: Effect of x1 on y, controlling for x2
# Based on theoretical framework in Smith et al. (2020)
fit <- lm(y ~ x1 + x2, data = data_clean)
```

## Function Design

Prefer functional style:

```r
# WRONG: Side effects
process_data <- function(data) {
  data$new_col <- data$x * 2  # Mutation
  data
}

# CORRECT: Pure function
add_transformed_var <- function(data, var_name, factor = 2) {
  data %>%
    mutate(!!var_name := x * factor)
}
```

## Magic Numbers

NO MAGIC NUMBERS:

```r
# WRONG
model <- lm(y ~ x, data = data[data$age > 18, ])

# CORRECT
MIN_AGE <- 18  # Minimum age for inclusion (IRB protocol #2024-001)
model <- lm(y ~ x, data = data[data$age > MIN_AGE, ])
```

## Version Control

- Commit analysis scripts to git
- Never commit raw data (add to .gitignore)
- Never commit large result files (>10MB)
- Commit key results as CSVs
- Document data sources in README

# Analysis Code Style

**Language**: English only for all code, comments, and documentation

**Technology Stack**: Python, JAX, and related packages (NumPy, SciPy, Pandas, etc.)

---

## Reproducibility (CRITICAL)

ALWAYS ensure analyses can be reproduced:

```python
# WRONG: No seed, no documentation
import random
import numpy as np

sample = random.sample(data, 100)
result = np.mean(sample)

# CORRECT: Reproducible with JAX
import jax
import jax.numpy as jnp

# Fixed seed with JAX's PRNG system
key = jax.random.PRNGKey(42)
# Sample 100 observations for pilot analysis (using proper subkey)
key, subkey = jax.random.split(key)
indices = jax.random.choice(subkey, len(data), shape=(100,), replace=False)
sample = data[indices]
result = jnp.mean(sample)
```

```python
# CORRECT: Reproducible with NumPy
import numpy as np

# Set global seed for reproducibility
np.random.seed(42)
# Document purpose: Sample for cross-validation split
indices = np.random.choice(len(data), size=100, replace=False)
sample = data[indices]
```

### JAX PRNG Best Practices

```python
# CRITICAL: Never reuse keys in JAX
key = jax.random.PRNGKey(0)

# WRONG: Key reuse (defeats randomness)
sample1 = jax.random.normal(key, (100,))
sample2 = jax.random.normal(key, (100,))  # Same as sample1!

# CORRECT: Always split keys
key, subkey1 = jax.random.split(key)
sample1 = jax.random.normal(subkey1, (100,))

key, subkey2 = jax.random.split(key)
sample2 = jax.random.normal(subkey2, (100,))

# BEST: Split multiple keys at once
key, *subkeys = jax.random.split(key, num=3)  # Generate 3 subkeys
sample1 = jax.random.normal(subkeys[0], (100,))
sample2 = jax.random.normal(subkeys[1], (100,))
```

---

## File Organization

MANY SMALL SCRIPTS > ONE LARGE SCRIPT:
- `01_load_data.py` - Data loading only
- `02_clean_data.py` - Data cleaning
- `03_analysis.py` - Primary analysis
- `04_sensitivity.py` - Sensitivity analyses
- `05_figures.py` - Figure generation
- `utils/` - Reusable functions

Typical limits:
- 200-300 lines for analysis scripts
- 500 max for main analysis
- Extract functions to `utils/` directory

### Directory Structure

```
project/
├── data/
│   ├── raw/           # Original data (read-only, not in git)
│   └── processed/     # Cleaned data (can be in git if de-identified)
├── notebooks/         # Exploratory Jupyter notebooks
├── scripts/           # Analysis scripts (01_*, 02_*, ...)
├── utils/             # Reusable functions
│   ├── data.py        # Data processing utilities
│   ├── models.py      # Model definitions
│   └── plotting.py    # Visualization functions
├── results/           # Analysis outputs (CSVs, tables)
├── figures/           # Plots and visualizations
├── tests/             # Unit tests
├── requirements.txt   # Python dependencies
└── README.md          # Project documentation
```

---

## Error Handling

ALWAYS check data quality:

```python
# WRONG: Assume data is clean
import pandas as pd

df = pd.read_csv('data.csv')
X = df[['feature1', 'feature2']].values
y = df['outcome'].values
model.fit(X, y)

# CORRECT: Validate first
import pandas as pd
import numpy as np

df = pd.read_csv('data.csv')

# Data quality checks
assert not df['outcome'].isna().any(), "Outcome contains NaN"
assert not df[['feature1', 'feature2']].isna().any().any(), "Features contain NaN"
assert len(df) > 0, "Empty dataset"
assert len(df) >= 100, f"Insufficient data: {len(df)} rows (need ≥100)"

X = df[['feature1', 'feature2']].values
y = df['outcome'].values
model.fit(X, y)
```

### JAX-Specific Validation

```python
import jax.numpy as jnp

# CORRECT: Check array properties
def validate_data(X, y):
    """Validate input data for JAX models."""
    # Check shapes
    assert X.ndim == 2, f"X must be 2D, got shape {X.shape}"
    assert y.ndim == 1, f"y must be 1D, got shape {y.shape}"
    assert X.shape[0] == y.shape[0], "X and y must have same number of samples"

    # Check for NaN/Inf (JAX doesn't always error on these)
    assert jnp.all(jnp.isfinite(X)), "X contains NaN or Inf"
    assert jnp.all(jnp.isfinite(y)), "y contains NaN or Inf"

    # Check data types (JAX prefers float32)
    assert X.dtype == jnp.float32, f"X should be float32, got {X.dtype}"
    assert y.dtype == jnp.float32, f"y should be float32, got {y.dtype}"

    return True

# Use in analysis
X_jax = jnp.array(X, dtype=jnp.float32)
y_jax = jnp.array(y, dtype=jnp.float32)
validate_data(X_jax, y_jax)
```

---

## Data Immutability

NEVER modify raw data files:
- Keep raw data in `data/raw/` (read-only, never in git)
- Save cleaned data to `data/processed/`
- Document all transformations

```python
# WRONG: Overwrite raw data
import pandas as pd

data = pd.read_csv("data/raw/study.csv")
data = data.dropna(subset=['outcome'])
data.to_csv("data/raw/study.csv")  # BAD! Overwrites original

# CORRECT: Save to processed
data_raw = pd.read_csv("data/raw/study.csv")
# Document transformation
data_clean = data_raw.dropna(subset=['outcome'])  # Remove missing outcomes
data_clean.to_csv("data/processed/study_clean.csv", index=False)

# Log transformation details
with open("data/processed/cleaning_log.txt", "w") as f:
    f.write(f"Original rows: {len(data_raw)}\n")
    f.write(f"After removing missing outcomes: {len(data_clean)}\n")
    f.write(f"Rows dropped: {len(data_raw) - len(data_clean)}\n")
```

### JAX Functional Immutability

```python
# CORRECT: JAX encourages functional style (no mutations)
import jax.numpy as jnp

def normalize(X):
    """Normalize features (functional style - no mutation)."""
    mean = jnp.mean(X, axis=0)
    std = jnp.std(X, axis=0)
    # Return NEW array, don't modify X
    return (X - mean) / (std + 1e-8), mean, std

# Use
X_norm, mean, std = normalize(X)  # X is unchanged
```

---

## Code Quality Checklist

Before marking analysis complete:
- [ ] Random seed set and documented (JAX key or NumPy seed)
- [ ] Data validation checks included
- [ ] No magic numbers (use named constants)
- [ ] Functions have clear names and type hints
- [ ] Complex logic has comments
- [ ] Results saved to `results/`
- [ ] Figures saved to `figures/`
- [ ] No absolute paths (use relative paths or `pathlib`)
- [ ] All dependencies in `requirements.txt`
- [ ] JAX device placement specified if using GPU/TPU

---

## Documentation

ALWAYS document analysis steps in English:

```python
# WRONG: No context
from sklearn.linear_model import LinearRegression

fit = LinearRegression().fit(X, y)

# CORRECT: Clear documentation in English
from sklearn.linear_model import LinearRegression

# Main hypothesis test: Effect of treatment (X[:, 0]) on outcome,
# controlling for age (X[:, 1]) and baseline health (X[:, 2])
# Based on theoretical framework in Smith et al. (2020)
fit = LinearRegression().fit(X, y)
treatment_effect = fit.coef_[0]
```

---

## Function Design

Prefer functional style (especially with JAX):

```python
# WRONG: Side effects and mutations
def process_data(data):
    data['new_col'] = data['x'] * 2  # Mutation
    return data

# CORRECT: Pure function
def add_transformed_feature(data: pd.DataFrame,
                           col_name: str,
                           factor: float = 2.0) -> pd.DataFrame:
    """
    Add transformed feature to dataframe.

    Args:
        data: Input dataframe
        col_name: Name of new column
        factor: Multiplication factor

    Returns:
        New dataframe with added column (original unchanged)
    """
    return data.assign(**{col_name: data['x'] * factor})
```

### Type Hints (Mandatory)

```python
# WRONG: No type hints
def compute_ate(y, treatment):
    return y[treatment == 1].mean() - y[treatment == 0].mean()

# CORRECT: Type hints + docstring
import numpy as np
from numpy.typing import NDArray

def compute_ate(y: NDArray[np.float64],
                treatment: NDArray[np.int32]) -> float:
    """
    Compute Average Treatment Effect (ATE).

    Args:
        y: Outcome variable (n,)
        treatment: Binary treatment indicator (n,), values in {0, 1}

    Returns:
        ATE estimate: E[Y|T=1] - E[Y|T=0]

    Raises:
        AssertionError: If treatment is not binary
    """
    assert set(treatment) == {0, 1}, "Treatment must be binary {0, 1}"
    return float(y[treatment == 1].mean() - y[treatment == 0].mean())
```

---

## Magic Numbers

NO MAGIC NUMBERS:

```python
# WRONG
model = train_model(data[data['age'] > 18])

# CORRECT
MIN_AGE = 18  # Minimum age for inclusion (IRB protocol #2024-001)
model = train_model(data[data['age'] > MIN_AGE])
```

```python
# WRONG: Hyperparameters as magic numbers
model = Model(hidden_dim=256, lr=0.001, dropout=0.1)

# CORRECT: Named configuration
from dataclasses import dataclass

@dataclass
class ModelConfig:
    """Model hyperparameters."""
    hidden_dim: int = 256      # Hidden layer dimension
    learning_rate: float = 1e-3  # Adam learning rate
    dropout_rate: float = 0.1   # Dropout probability

config = ModelConfig()
model = Model(config.hidden_dim, config.learning_rate, config.dropout_rate)
```

---

## Version Control

- Commit analysis scripts to git
- **Never commit raw data** (add to .gitignore)
- Never commit large result files (>10MB)
- Commit key results as CSVs
- Document data sources in README

```.gitignore
# Data files
data/raw/
*.csv
*.xlsx
*.dta
*.sav

# Large result files
results/*.pkl
results/*.joblib
models/*.h5

# Jupyter checkpoints
.ipynb_checkpoints/

# Python cache
__pycache__/
*.pyc
*.pyo

# JAX compilation cache
.jax_cache/
```

---

## Causal Inference Specific Rules

### 1. Identify Confounders

```python
# WRONG: Naive correlation
from scipy.stats import pearsonr

correlation = pearsonr(treatment, outcome)[0]
print(f"Treatment effect: {correlation}")  # Confounded!

# CORRECT: Control for confounders (regression adjustment)
from sklearn.linear_model import LinearRegression

# Regress outcome on treatment + confounders
X = np.column_stack([treatment, age, sex, baseline_health])
model = LinearRegression().fit(X, outcome)
treatment_effect = model.coef_[0]  # Adjusted for confounders
```

### 2. Check Overlap/Positivity

```python
# CRITICAL: Check covariate balance and overlap
def check_overlap(treatment: NDArray, propensity: NDArray) -> None:
    """
    Check positivity assumption: 0 < P(T=1|X) < 1 for all X.

    Args:
        treatment: Binary treatment (n,)
        propensity: Estimated propensity scores (n,)
    """
    treated = propensity[treatment == 1]
    control = propensity[treatment == 0]

    # Check for extreme propensity scores
    assert (propensity > 0.01).all(), "Propensity scores too close to 0"
    assert (propensity < 0.99).all(), "Propensity scores too close to 1"

    # Visualize overlap
    import matplotlib.pyplot as plt
    plt.hist(treated, alpha=0.5, label='Treated', bins=20)
    plt.hist(control, alpha=0.5, label='Control', bins=20)
    plt.xlabel('Propensity Score')
    plt.ylabel('Frequency')
    plt.legend()
    plt.title('Covariate Overlap Check')
    plt.savefig('figures/propensity_overlap.png')
    plt.close()
```

### 3. Sensitivity Analysis

```python
# MANDATORY: Test robustness to unmeasured confounding
def sensitivity_analysis_rosenbaum(ate_estimate: float,
                                   se: float,
                                   gamma_range: NDArray) -> None:
    """
    Rosenbaum bounds sensitivity analysis.

    Args:
        ate_estimate: Point estimate of ATE
        se: Standard error
        gamma_range: Range of sensitivity parameters (e.g., [1.0, 1.5, 2.0])
    """
    from scipy.stats import norm

    results = []
    for gamma in gamma_range:
        # Adjust confidence interval for unmeasured confounding
        ci_lower = ate_estimate - gamma * 1.96 * se
        ci_upper = ate_estimate + gamma * 1.96 * se
        results.append({
            'gamma': gamma,
            'ci_lower': ci_lower,
            'ci_upper': ci_upper,
            'includes_zero': (ci_lower <= 0 <= ci_upper)
        })

    # Report
    print("Sensitivity Analysis (Rosenbaum Bounds):")
    print("-" * 60)
    for r in results:
        status = "INCONCLUSIVE" if r['includes_zero'] else "ROBUST"
        print(f"Γ={r['gamma']:.1f}: CI=[{r['ci_lower']:.3f}, {r['ci_upper']:.3f}] {status}")
```

---

## Deep Learning Specific Rules (JAX)

### 1. Device Management

```python
# CORRECT: Explicit device placement
import jax

# Check available devices
print("Available devices:", jax.devices())

# Use GPU if available, fallback to CPU
device = jax.devices('gpu')[0] if jax.devices('gpu') else jax.devices('cpu')[0]

# Move data to device
X_device = jax.device_put(X, device)
y_device = jax.device_put(y, device)
```

### 2. Gradient Checks

```python
# MANDATORY: Verify gradients numerically (at least once)
import jax
import jax.numpy as jnp

def loss_fn(params, X, y):
    """Example loss function."""
    predictions = jnp.dot(X, params)
    return jnp.mean((predictions - y) ** 2)

def numerical_gradient(fn, params, X, y, eps=1e-5):
    """Compute gradients numerically for verification."""
    grad_num = jnp.zeros_like(params)
    for i in range(len(params)):
        params_plus = params.at[i].set(params[i] + eps)
        params_minus = params.at[i].set(params[i] - eps)
        grad_num = grad_num.at[i].set(
            (fn(params_plus, X, y) - fn(params_minus, X, y)) / (2 * eps)
        )
    return grad_num

# Verify gradients
params_init = jax.random.normal(jax.random.PRNGKey(0), (X.shape[1],))
grad_auto = jax.grad(loss_fn)(params_init, X, y)
grad_num = numerical_gradient(loss_fn, params_init, X, y)

# Should be very close
diff = jnp.abs(grad_auto - grad_num).max()
assert diff < 1e-4, f"Gradient error: {diff}"
print(f"✓ Gradient check passed (max diff: {diff:.2e})")
```

### 3. Training Loop Best Practices

```python
# CORRECT: Proper JAX training loop with JIT compilation
from typing import Tuple, Dict
import optax  # JAX optimization library

@jax.jit  # JIT compile for speed
def train_step(params: Dict,
               opt_state: optax.OptState,
               X: jnp.ndarray,
               y: jnp.ndarray,
               optimizer: optax.GradientTransformation) -> Tuple:
    """
    Single training step (JIT compiled).

    Returns:
        Updated params, optimizer state, and loss value
    """
    loss, grads = jax.value_and_grad(loss_fn)(params, X, y)
    updates, opt_state = optimizer.update(grads, opt_state)
    params = optax.apply_updates(params, updates)
    return params, opt_state, loss

# Training loop
def train_model(X, y, num_epochs=100, learning_rate=1e-3):
    """Train model with proper logging and checkpointing."""
    # Initialize
    key = jax.random.PRNGKey(42)
    params = initialize_params(key, X.shape[1])
    optimizer = optax.adam(learning_rate)
    opt_state = optimizer.init(params)

    # Track metrics
    losses = []

    for epoch in range(num_epochs):
        params, opt_state, loss = train_step(params, opt_state, X, y, optimizer)
        losses.append(float(loss))

        # Log progress
        if epoch % 10 == 0:
            print(f"Epoch {epoch:3d} | Loss: {loss:.4f}")

        # Early stopping check
        if epoch > 20 and losses[-1] > losses[-20]:
            print(f"Early stopping at epoch {epoch} (loss increasing)")
            break

    return params, losses
```

### 4. Model Checkpointing

```python
# CORRECT: Save model checkpoints
import pickle
from pathlib import Path

def save_checkpoint(params: Dict,
                   losses: list,
                   epoch: int,
                   checkpoint_dir: str = "checkpoints/") -> None:
    """Save model checkpoint."""
    Path(checkpoint_dir).mkdir(exist_ok=True)

    checkpoint = {
        'params': params,
        'losses': losses,
        'epoch': epoch
    }

    filepath = f"{checkpoint_dir}/checkpoint_epoch{epoch:04d}.pkl"
    with open(filepath, 'wb') as f:
        pickle.dump(checkpoint, f)

    print(f"✓ Checkpoint saved: {filepath}")
```

### 5. Numerical Stability

```python
# CRITICAL: Avoid numerical instability
import jax.numpy as jnp

# WRONG: Direct softmax (can overflow)
def softmax_unstable(logits):
    return jnp.exp(logits) / jnp.sum(jnp.exp(logits))

# CORRECT: Log-sum-exp trick
def softmax_stable(logits):
    """Numerically stable softmax."""
    logits_max = jnp.max(logits, axis=-1, keepdims=True)
    exp_logits = jnp.exp(logits - logits_max)
    return exp_logits / jnp.sum(exp_logits, axis=-1, keepdims=True)

# BEST: Use JAX built-in (already stable)
from jax.nn import softmax
probs = softmax(logits)
```

---

## Performance Best Practices

### JAX-Specific Optimizations

```python
# 1. Use JIT compilation
@jax.jit
def compute_loss(params, X, y):
    """JIT-compiled loss computation (first call slow, then fast)."""
    predictions = model(params, X)
    return jnp.mean((predictions - y) ** 2)

# 2. Use vectorization (vmap) instead of loops
# SLOW: Loop over batch
def predict_loop(params, X_batch):
    predictions = []
    for x in X_batch:
        predictions.append(model(params, x))
    return jnp.array(predictions)

# FAST: Vectorized
predict_vectorized = jax.vmap(lambda x: model(params, x))
predictions = predict_vectorized(X_batch)

# 3. Use float32 by default (faster than float64)
X = jnp.array(X, dtype=jnp.float32)

# 4. Batch operations
# WRONG: Process one sample at a time
for i in range(len(X)):
    result = process_sample(X[i])

# CORRECT: Batch processing
batch_size = 32
for i in range(0, len(X), batch_size):
    batch = X[i:i+batch_size]
    results = process_batch(batch)
```

---

## Testing

### Unit Tests (Mandatory for Utils)

```python
# tests/test_utils.py
import pytest
import numpy as np
from numpy.testing import assert_array_almost_equal
from utils.data import normalize, compute_ate

def test_normalize():
    """Test normalization function."""
    X = np.array([[1, 2], [3, 4], [5, 6]])
    X_norm, mean, std = normalize(X)

    # Check mean is 0, std is 1
    assert_array_almost_equal(X_norm.mean(axis=0), [0, 0], decimal=6)
    assert_array_almost_equal(X_norm.std(axis=0), [1, 1], decimal=6)

def test_compute_ate():
    """Test ATE computation."""
    y = np.array([1.0, 2.0, 3.0, 4.0])
    treatment = np.array([0, 1, 0, 1])

    ate = compute_ate(y, treatment)
    expected = (2.0 + 4.0) / 2 - (1.0 + 3.0) / 2
    assert_array_almost_equal(ate, expected)

def test_compute_ate_binary_check():
    """Test ATE rejects non-binary treatment."""
    y = np.array([1.0, 2.0, 3.0])
    treatment = np.array([0, 1, 2])  # Not binary!

    with pytest.raises(AssertionError):
        compute_ate(y, treatment)
```

---

## Summary

**Critical principles**:
1. **English only** - Code, comments, documentation
2. **JAX + Python** - No R code
3. **Reproducibility** - Seeds, documentation, version control
4. **Type hints** - Mandatory for all functions
5. **Validation** - Check data quality, gradients, assumptions
6. **Functional style** - Immutability, pure functions (especially for JAX)
7. **Testing** - Unit tests for utilities

**For causal inference**:
- Check overlap/positivity
- Run sensitivity analysis
- Document identification assumptions

**For deep learning**:
- Use JAX best practices (JIT, vmap, device management)
- Verify gradients numerically
- Monitor training stability
- Save checkpoints

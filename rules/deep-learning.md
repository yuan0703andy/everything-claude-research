# Deep Learning Rules (JAX-Focused)

**Primary Framework**: JAX + Flax/Haiku + Optax

**Philosophy**: Functional, composable, and debuggable neural networks

---

## Core JAX Principles

### 1. Functional Programming

JAX enforces functional programming - NO side effects:

```python
# WRONG: Stateful training (PyTorch style)
class Model(nn.Module):
    def __init__(self):
        self.weights = nn.Parameter(torch.randn(10, 5))

    def forward(self, x):
        self.weights += 0.01  # MUTATION - doesn't work in JAX!
        return x @ self.weights

# CORRECT: Pure functions in JAX
import jax
import jax.numpy as jnp

def forward(params, x):
    """Pure function: params and x are NOT modified."""
    return jnp.dot(x, params['weights'])

def update_params(params, grads, learning_rate):
    """Return NEW params, don't modify old ones."""
    return jax.tree_map(lambda p, g: p - learning_rate * g, params, grads)
```

### 2. Explicit Randomness (PRNG Keys)

```python
# CRITICAL: Never reuse random keys
key = jax.random.PRNGKey(0)

# WRONG: Key reuse
dropout_mask1 = jax.random.bernoulli(key, 0.5, (100,))
dropout_mask2 = jax.random.bernoulli(key, 0.5, (100,))  # Same as mask1!

# CORRECT: Always split keys
key, subkey1, subkey2 = jax.random.split(key, 3)
dropout_mask1 = jax.random.bernoulli(subkey1, 0.5, (100,))
dropout_mask2 = jax.random.bernoulli(subkey2, 0.5, (100,))

# BEST PRACTICE: Split keys in training loop
def train_step(key, params, x, y):
    key, dropout_key = jax.random.split(key)
    # Use dropout_key for this step
    loss, grads = jax.value_and_grad(loss_fn)(params, x, y, dropout_key)
    params = update_params(params, grads)
    return key, params, loss  # Return updated key!
```

### 3. JIT Compilation

```python
import jax

# Compile functions for speed (10-100x faster)
@jax.jit
def fast_function(x, y):
    """First call: compile. Subsequent calls: fast."""
    return jnp.dot(x, y) + jnp.sum(x)

# When to use JIT:
# ✅ Training steps (called many times)
# ✅ Forward pass
# ✅ Loss computation
# ❌ Data loading (not differentiable)
# ❌ Plotting (side effects)

# JIT with static arguments
@jax.jit
def forward(params, x, training=False):
    # training flag changes computation
    # Mark as static to avoid recompilation
    pass

@jax.partial(jax.jit, static_argnames=['training'])
def forward_static(params, x, training):
    """Compile separately for training=True and training=False."""
    if training:
        return apply_dropout(params, x)
    else:
        return apply_no_dropout(params, x)
```

---

## Model Architecture Design

### Flax (Recommended for Beginners)

```python
from flax import linen as nn
import jax.numpy as jnp

class MLP(nn.Module):
    """Simple multi-layer perceptron."""
    features: list  # [hidden_dim1, hidden_dim2, output_dim]
    dropout_rate: float = 0.1

    @nn.compact
    def __call__(self, x, training: bool = False):
        """
        Forward pass.

        Args:
            x: Input (batch_size, input_dim)
            training: Whether in training mode (affects dropout)

        Returns:
            Output (batch_size, output_dim)
        """
        for i, feat in enumerate(self.features[:-1]):
            x = nn.Dense(feat, name=f'hidden_{i}')(x)
            x = nn.relu(x)
            x = nn.Dropout(rate=self.dropout_rate,
                          deterministic=not training)(x)

        # Output layer (no activation)
        x = nn.Dense(self.features[-1], name='output')(x)
        return x

# Initialize model
model = MLP(features=[128, 64, 10])

# Initialize parameters
key = jax.random.PRNGKey(0)
dummy_input = jnp.ones((1, 784))  # Batch of 1, input dim 784
params = model.init(key, dummy_input, training=False)

# Forward pass
output = model.apply(params, dummy_input, training=False)
```

### Custom Initialization

```python
import jax
from flax import linen as nn

class CustomMLP(nn.Module):
    features: list

    def setup(self):
        """Alternative to @nn.compact - define layers in setup."""
        self.layers = [nn.Dense(feat) for feat in self.features]

    def __call__(self, x):
        for layer in self.layers[:-1]:
            x = layer(x)
            x = nn.relu(x)
        x = self.layers[-1](x)  # Output layer
        return x

# Custom weight initialization
from jax.nn.initializers import he_normal, zeros

class CustomInitMLP(nn.Module):
    features: list

    @nn.compact
    def __call__(self, x):
        for feat in self.features:
            # He initialization for ReLU networks
            x = nn.Dense(feat,
                        kernel_init=he_normal(),
                        bias_init=zeros)(x)
            x = nn.relu(x)
        return x
```

---

## Training Loop Best Practices

### Complete Training Pipeline

```python
import jax
import jax.numpy as jnp
from flax import linen as nn
from flax.training import train_state
import optax
from typing import Tuple, Dict, Any

# ============================================================================
# 1. Define Loss Function
# ============================================================================

def cross_entropy_loss(logits, labels):
    """
    Cross-entropy loss for classification.

    Args:
        logits: Model outputs (batch_size, num_classes)
        labels: True labels (batch_size,) as integers

    Returns:
        Scalar loss
    """
    # One-hot encode labels
    num_classes = logits.shape[-1]
    labels_onehot = jax.nn.one_hot(labels, num_classes)

    # Softmax cross-entropy
    log_probs = jax.nn.log_softmax(logits)
    loss = -jnp.sum(labels_onehot * log_probs) / len(labels)

    return loss

# ============================================================================
# 2. Training State (Flax Pattern)
# ============================================================================

def create_train_state(rng, model, learning_rate, input_shape):
    """
    Create training state with model, optimizer, and parameters.

    Args:
        rng: Random key
        model: Flax model
        learning_rate: Optimizer learning rate
        input_shape: Shape of input for initialization

    Returns:
        TrainState object
    """
    # Initialize parameters
    dummy_input = jnp.ones(input_shape)
    params = model.init(rng, dummy_input, training=False)['params']

    # Create optimizer
    tx = optax.adam(learning_rate)

    # Create training state
    state = train_state.TrainState.create(
        apply_fn=model.apply,
        params=params,
        tx=tx
    )

    return state

# ============================================================================
# 3. Training Step (JIT-compiled)
# ============================================================================

@jax.jit
def train_step(state: train_state.TrainState,
               batch: Dict[str, jnp.ndarray],
               dropout_rng: jax.random.PRNGKey) -> Tuple[train_state.TrainState, float]:
    """
    Single training step.

    Args:
        state: Current training state
        batch: Dictionary with 'image' and 'label'
        dropout_rng: Random key for dropout

    Returns:
        Updated state and loss value
    """
    def loss_fn(params):
        """Compute loss for this batch."""
        logits = state.apply_fn(
            {'params': params},
            batch['image'],
            training=True,
            rngs={'dropout': dropout_rng}
        )
        loss = cross_entropy_loss(logits, batch['label'])
        return loss

    # Compute loss and gradients
    loss, grads = jax.value_and_grad(loss_fn)(state.params)

    # Update parameters
    state = state.apply_gradients(grads=grads)

    return state, loss

# ============================================================================
# 4. Evaluation Step (JIT-compiled)
# ============================================================================

@jax.jit
def eval_step(state: train_state.TrainState,
              batch: Dict[str, jnp.ndarray]) -> Tuple[float, float]:
    """
    Single evaluation step.

    Args:
        state: Current training state
        batch: Dictionary with 'image' and 'label'

    Returns:
        Loss and accuracy
    """
    logits = state.apply_fn(
        {'params': state.params},
        batch['image'],
        training=False  # No dropout during eval
    )

    loss = cross_entropy_loss(logits, batch['label'])

    # Compute accuracy
    predictions = jnp.argmax(logits, axis=-1)
    accuracy = jnp.mean(predictions == batch['label'])

    return loss, accuracy

# ============================================================================
# 5. Main Training Loop
# ============================================================================

def train_model(model, train_ds, val_ds,
                num_epochs=10,
                learning_rate=1e-3,
                batch_size=32,
                seed=0):
    """
    Complete training loop.

    Args:
        model: Flax model
        train_ds: Training dataset
        val_ds: Validation dataset
        num_epochs: Number of training epochs
        learning_rate: Optimizer learning rate
        batch_size: Batch size
        seed: Random seed

    Returns:
        Trained state and training history
    """
    # Initialize
    rng = jax.random.PRNGKey(seed)
    rng, init_rng = jax.random.split(rng)

    input_shape = (1, 28, 28, 1)  # Example for MNIST
    state = create_train_state(init_rng, model, learning_rate, input_shape)

    # Training history
    history = {
        'train_loss': [],
        'val_loss': [],
        'val_accuracy': []
    }

    # Training loop
    for epoch in range(num_epochs):
        # Training
        train_losses = []
        for batch in train_ds:  # Iterate over training batches
            rng, dropout_rng = jax.random.split(rng)
            state, loss = train_step(state, batch, dropout_rng)
            train_losses.append(loss)

        # Validation
        val_losses = []
        val_accuracies = []
        for batch in val_ds:  # Iterate over validation batches
            loss, accuracy = eval_step(state, batch)
            val_losses.append(loss)
            val_accuracies.append(accuracy)

        # Compute epoch metrics
        epoch_train_loss = jnp.mean(jnp.array(train_losses))
        epoch_val_loss = jnp.mean(jnp.array(val_losses))
        epoch_val_accuracy = jnp.mean(jnp.array(val_accuracies))

        # Log
        print(f"Epoch {epoch+1}/{num_epochs}")
        print(f"  Train Loss: {epoch_train_loss:.4f}")
        print(f"  Val Loss: {epoch_val_loss:.4f}")
        print(f"  Val Accuracy: {epoch_val_accuracy:.4f}")

        # Save history
        history['train_loss'].append(float(epoch_train_loss))
        history['val_loss'].append(float(epoch_val_loss))
        history['val_accuracy'].append(float(epoch_val_accuracy))

        # Early stopping check
        if epoch > 5 and history['val_loss'][-1] > history['val_loss'][-5]:
            print(f"Early stopping at epoch {epoch+1} (validation loss increasing)")
            break

    return state, history
```

---

## Gradient Checking (CRITICAL)

```python
def numerical_gradient(f, params, eps=1e-5):
    """
    Compute gradients numerically for verification.

    Args:
        f: Function to differentiate (takes params, returns scalar)
        params: Parameters (pytree)
        eps: Finite difference step size

    Returns:
        Numerical gradients (same structure as params)
    """
    def grad_for_param(param_path, param_value):
        """Compute numerical gradient for single parameter."""
        # Helper to set parameter at path
        def set_param(params, path, value):
            if len(path) == 1:
                params[path[0]] = value
            else:
                set_param(params[path[0]], path[1:], value)
            return params

        # f(param + eps)
        params_plus = jax.tree_map(lambda x: x, params)  # Deep copy
        set_param(params_plus, param_path, param_value + eps)
        loss_plus = f(params_plus)

        # f(param - eps)
        params_minus = jax.tree_map(lambda x: x, params)  # Deep copy
        set_param(params_minus, param_path, param_value - eps)
        loss_minus = f(params_minus)

        # Numerical gradient
        return (loss_plus - loss_minus) / (2 * eps)

    # Compute for all parameters
    # (Simplified - full implementation would use jax.tree_util.tree_flatten)
    grad_numerical = jax.tree_map(
        lambda p: numerical_gradient_single(f, p, eps),
        params
    )

    return grad_numerical

def check_gradients(loss_fn, params, x, y, eps=1e-5, threshold=1e-4):
    """
    Verify automatic gradients against numerical gradients.

    Args:
        loss_fn: Loss function(params, x, y) -> scalar
        params: Model parameters
        x, y: Example input and target
        eps: Finite difference step size
        threshold: Maximum allowed difference

    Raises:
        AssertionError if gradients differ by more than threshold
    """
    # Automatic gradients
    grad_auto = jax.grad(loss_fn)(params, x, y)

    # Numerical gradients
    grad_num = numerical_gradient(lambda p: loss_fn(p, x, y), params, eps)

    # Compare
    diff = jax.tree_map(lambda a, n: jnp.abs(a - n), grad_auto, grad_num)
    max_diff = jax.tree_util.tree_reduce(
        lambda a, b: max(a, jnp.max(b)),
        diff,
        initializer=0.0
    )

    if max_diff > threshold:
        print(f"❌ Gradient check FAILED: max difference = {max_diff:.2e}")
        print("Automatic gradients may be incorrect!")
        raise AssertionError(f"Gradient error: {max_diff:.2e} > {threshold:.2e}")
    else:
        print(f"✅ Gradient check PASSED: max difference = {max_diff:.2e}")

# Usage
model = MLP(features=[64, 10])
params = model.init(jax.random.PRNGKey(0), jnp.ones((1, 784)), training=False)
x_test = jax.random.normal(jax.random.PRNGKey(1), (10, 784))
y_test = jax.random.randint(jax.random.PRNGKey(2), (10,), 0, 10)

check_gradients(
    lambda p, x, y: cross_entropy_loss(model.apply(p, x, training=False), y),
    params,
    x_test,
    y_test
)
```

---

## Debugging Strategies

### 1. Check Shapes

```python
def print_shapes(pytree, name="params"):
    """Print shapes of all arrays in pytree."""
    print(f"\n{name} shapes:")
    print("-" * 60)
    for key, value in jax.tree_util.tree_flatten_with_path(pytree)[0]:
        path = '/'.join(str(k) for k in key)
        print(f"  {path}: {value.shape} ({value.dtype})")

# Usage
print_shapes(params, "Model parameters")
```

### 2. Monitor Gradients

```python
def check_gradient_health(grads):
    """
    Check for gradient problems.

    Args:
        grads: Gradients pytree

    Prints warnings for:
    - NaN or Inf gradients
    - Very large gradients (>100)
    - Very small gradients (<1e-6)
    - Zero gradients
    """
    grad_stats = jax.tree_map(
        lambda g: {
            'mean': jnp.mean(jnp.abs(g)),
            'max': jnp.max(jnp.abs(g)),
            'has_nan': jnp.any(jnp.isnan(g)),
            'has_inf': jnp.any(jnp.isinf(g))
        },
        grads
    )

    for key, stats in jax.tree_util.tree_flatten_with_path(grad_stats)[0]:
        path = '/'.join(str(k) for k in key)

        if stats['has_nan']:
            print(f"⚠️  NaN gradients in {path}")
        if stats['has_inf']:
            print(f"⚠️  Inf gradients in {path}")
        if stats['max'] > 100:
            print(f"⚠️  Very large gradients in {path}: max={stats['max']:.2e}")
        if stats['mean'] < 1e-6:
            print(f"⚠️  Very small gradients in {path}: mean={stats['mean']:.2e}")

# Usage in training loop
loss, grads = jax.value_and_grad(loss_fn)(params, x, y)
check_gradient_health(grads)
```

### 3. Overfit Single Batch

```python
def overfit_single_batch(model, batch, num_steps=1000):
    """
    Overfit on single batch - sanity check.

    If model can't overfit one batch, something is wrong:
    - Model too small
    - Learning rate wrong
    - Bug in forward pass or loss

    Args:
        model: Flax model
        batch: Single batch of data
        num_steps: Number of training steps

    Returns:
        Final loss (should be near 0)
    """
    # Initialize
    rng = jax.random.PRNGKey(0)
    params = model.init(rng, batch['image'], training=False)['params']
    optimizer = optax.adam(1e-3)
    opt_state = optimizer.init(params)

    # Training loop
    for step in range(num_steps):
        rng, dropout_rng = jax.random.split(rng)

        # Compute loss and gradients
        def loss_fn(p):
            logits = model.apply({'params': p}, batch['image'],
                                training=True, rngs={'dropout': dropout_rng})
            return cross_entropy_loss(logits, batch['label'])

        loss, grads = jax.value_and_grad(loss_fn)(params)

        # Update
        updates, opt_state = optimizer.update(grads, opt_state)
        params = optax.apply_updates(params, updates)

        if step % 100 == 0:
            print(f"Step {step}: Loss = {loss:.4f}")

    if loss > 0.1:
        print(f"⚠️  WARNING: Cannot overfit single batch (final loss={loss:.4f})")
        print("   Check: model capacity, learning rate, loss function, forward pass")
    else:
        print(f"✅ Single batch overfit successful (final loss={loss:.6f})")

    return loss
```

---

## Regularization Techniques

### 1. Weight Decay (L2 Regularization)

```python
# Correct way to add weight decay in JAX
import optax

# Create optimizer with weight decay
optimizer = optax.adamw(learning_rate=1e-3, weight_decay=1e-4)

# Alternatively, add L2 penalty to loss
def loss_with_l2(params, x, y, weight_decay=1e-4):
    """Loss with L2 regularization."""
    # Standard loss
    logits = model.apply({'params': params}, x)
    ce_loss = cross_entropy_loss(logits, y)

    # L2 penalty
    l2_loss = sum(jnp.sum(jnp.square(p)) for p in jax.tree_util.tree_leaves(params))

    return ce_loss + weight_decay * l2_loss
```

### 2. Dropout (Built into Flax)

```python
class MLPWithDropout(nn.Module):
    features: list
    dropout_rate: float = 0.5

    @nn.compact
    def __call__(self, x, training: bool):
        for feat in self.features[:-1]:
            x = nn.Dense(feat)(x)
            x = nn.relu(x)
            # Dropout only during training
            x = nn.Dropout(rate=self.dropout_rate,
                          deterministic=not training)(x)
        x = nn.Dense(self.features[-1])(x)
        return x

# Usage
model = MLPWithDropout(features=[128, 64, 10], dropout_rate=0.5)

# Training: dropout active
output = model.apply(params, x, training=True, rngs={'dropout': dropout_key})

# Inference: dropout inactive
output = model.apply(params, x, training=False)
```

### 3. Batch Normalization

```python
class MLPWithBatchNorm(nn.Module):
    features: list

    @nn.compact
    def __call__(self, x, training: bool):
        for feat in self.features[:-1]:
            x = nn.Dense(feat)(x)
            # Batch norm before activation
            x = nn.BatchNorm(use_running_average=not training)(x)
            x = nn.relu(x)
        x = nn.Dense(self.features[-1])(x)
        return x

# Note: BatchNorm adds running mean/var to params
# Need to handle batch_stats separately
params = model.init(rng, x, training=True)
# params now contains: {'params': ..., 'batch_stats': ...}
```

---

## Model Evaluation

### 1. Compute Metrics

```python
from sklearn.metrics import classification_report, confusion_matrix
import numpy as np

def evaluate_model(state, test_ds):
    """
    Comprehensive model evaluation.

    Args:
        state: Trained model state
        test_ds: Test dataset

    Returns:
        Dictionary with metrics
    """
    all_predictions = []
    all_labels = []
    all_logits = []

    # Collect predictions
    for batch in test_ds:
        logits = state.apply_fn(
            {'params': state.params},
            batch['image'],
            training=False
        )
        predictions = jnp.argmax(logits, axis=-1)

        all_predictions.append(np.array(predictions))
        all_labels.append(np.array(batch['label']))
        all_logits.append(np.array(logits))

    # Concatenate
    all_predictions = np.concatenate(all_predictions)
    all_labels = np.concatenate(all_labels)
    all_logits = np.concatenate(all_logits)

    # Compute metrics
    accuracy = np.mean(all_predictions == all_labels)

    # Classification report
    print("\nClassification Report:")
    print(classification_report(all_labels, all_predictions))

    # Confusion matrix
    cm = confusion_matrix(all_labels, all_predictions)
    print("\nConfusion Matrix:")
    print(cm)

    return {
        'accuracy': accuracy,
        'predictions': all_predictions,
        'labels': all_labels,
        'logits': all_logits
    }
```

### 2. Calibration Check

```python
def plot_calibration(logits, labels, num_bins=10):
    """
    Plot calibration curve.

    Well-calibrated model: predicted probability matches empirical frequency.

    Args:
        logits: Model outputs (n, num_classes)
        labels: True labels (n,)
        num_bins: Number of bins for calibration curve
    """
    import matplotlib.pyplot as plt

    # Get predicted probabilities
    probs = jax.nn.softmax(logits, axis=-1)
    predicted_probs, predicted_classes = jnp.max(probs, axis=-1), jnp.argmax(probs, axis=-1)

    # Compute calibration
    correct = (predicted_classes == labels)

    bin_edges = np.linspace(0, 1, num_bins + 1)
    bin_centers = (bin_edges[:-1] + bin_edges[1:]) / 2

    empirical_freqs = []
    for i in range(num_bins):
        mask = (predicted_probs >= bin_edges[i]) & (predicted_probs < bin_edges[i+1])
        if np.sum(mask) > 0:
            empirical_freqs.append(np.mean(correct[mask]))
        else:
            empirical_freqs.append(np.nan)

    # Plot
    plt.figure(figsize=(8, 8))
    plt.plot([0, 1], [0, 1], 'k--', label='Perfect calibration')
    plt.plot(bin_centers, empirical_freqs, 'o-', label='Model calibration')
    plt.xlabel('Predicted Probability')
    plt.ylabel('Empirical Frequency')
    plt.title('Calibration Plot')
    plt.legend()
    plt.grid(True)
    plt.savefig('figures/calibration.png', dpi=300)
    plt.close()

    # Expected Calibration Error (ECE)
    bin_counts = []
    for i in range(num_bins):
        mask = (predicted_probs >= bin_edges[i]) & (predicted_probs < bin_edges[i+1])
        bin_counts.append(np.sum(mask))

    ece = 0
    for i in range(num_bins):
        if not np.isnan(empirical_freqs[i]):
            ece += (bin_counts[i] / len(labels)) * \
                   abs(empirical_freqs[i] - bin_centers[i])

    print(f"Expected Calibration Error (ECE): {ece:.4f}")
    return ece
```

---

## Hyperparameter Tuning

### Grid Search (Simple)

```python
from itertools import product

def hyperparameter_search(model_fn, train_ds, val_ds,
                         param_grid, num_epochs=10):
    """
    Simple grid search over hyperparameters.

    Args:
        model_fn: Function that creates model given hyperparameters
        train_ds, val_ds: Datasets
        param_grid: Dictionary of hyperparameter lists
        num_epochs: Training epochs per configuration

    Returns:
        Best hyperparameters and validation accuracy
    """
    # Generate all combinations
    keys = param_grid.keys()
    values = param_grid.values()
    combinations = list(product(*values))

    best_val_acc = 0
    best_params = None
    results = []

    for combo in combinations:
        hyperparams = dict(zip(keys, combo))
        print(f"\nTrying: {hyperparams}")

        # Create and train model
        model = model_fn(**hyperparams)
        state, history = train_model(model, train_ds, val_ds, num_epochs=num_epochs)

        val_acc = max(history['val_accuracy'])

        results.append({
            'hyperparams': hyperparams,
            'val_accuracy': val_acc
        })

        if val_acc > best_val_acc:
            best_val_acc = val_acc
            best_params = hyperparams

    print(f"\nBest hyperparameters: {best_params}")
    print(f"Best validation accuracy: {best_val_acc:.4f}")

    return best_params, results

# Usage
param_grid = {
    'hidden_dim': [64, 128, 256],
    'num_layers': [2, 3, 4],
    'dropout_rate': [0.1, 0.3, 0.5],
    'learning_rate': [1e-4, 1e-3, 1e-2]
}

best_params, results = hyperparameter_search(
    model_fn=lambda **kwargs: MLP(features=[kwargs['hidden_dim']] * kwargs['num_layers'] + [10],
                                  dropout_rate=kwargs['dropout_rate']),
    train_ds=train_ds,
    val_ds=val_ds,
    param_grid=param_grid
)
```

---

## Advanced: Custom Training Loop with Metrics

```python
import optax
from flax.training import train_state
from typing import Any, Callable

class TrainStateWithMetrics(train_state.TrainState):
    """Extended TrainState with metrics tracking."""
    batch_stats: Any = None
    metrics: dict = None

def create_train_state_with_metrics(rng, model, learning_rate, input_shape):
    """Create training state with metrics tracking."""
    dummy_input = jnp.ones(input_shape)

    # Initialize with batch_stats if model uses batch norm
    variables = model.init(rng, dummy_input, training=True)
    params = variables['params']
    batch_stats = variables.get('batch_stats')

    # Create optimizer schedule
    schedule = optax.warmup_cosine_decay_schedule(
        init_value=0.0,
        peak_value=learning_rate,
        warmup_steps=1000,
        decay_steps=10000,
        end_value=learning_rate * 0.01
    )

    optimizer = optax.chain(
        optax.clip_by_global_norm(1.0),  # Gradient clipping
        optax.adamw(learning_rate=schedule, weight_decay=1e-4)
    )

    state = TrainStateWithMetrics.create(
        apply_fn=model.apply,
        params=params,
        tx=optimizer,
        batch_stats=batch_stats,
        metrics={'train_loss': [], 'val_loss': [], 'val_accuracy': []}
    )

    return state
```

---

## Common Pitfalls and Solutions

### 1. Vanishing/Exploding Gradients

```python
# Solution 1: Gradient clipping
optimizer = optax.chain(
    optax.clip_by_global_norm(1.0),  # Clip gradients
    optax.adam(learning_rate)
)

# Solution 2: Proper initialization (He for ReLU, Xavier for tanh)
nn.Dense(feat, kernel_init=he_normal())

# Solution 3: Residual connections
class ResidualBlock(nn.Module):
    features: int

    @nn.compact
    def __call__(self, x):
        residual = x
        x = nn.Dense(self.features)(x)
        x = nn.relu(x)
        x = nn.Dense(self.features)(x)
        return x + residual  # Skip connection
```

### 2. Overfitting

```python
# Solutions:
# 1. Dropout
x = nn.Dropout(rate=0.5)(x, deterministic=not training)

# 2. Weight decay
optimizer = optax.adamw(learning_rate, weight_decay=1e-4)

# 3. Data augmentation (implement in data loading)

# 4. Early stopping (monitor validation loss)

# 5. Reduce model capacity
```

### 3. Slow Training

```python
# Solutions:
# 1. Use JIT compilation
@jax.jit
def train_step(...):
    pass

# 2. Use mixed precision (float16)
from jax import config
config.update("jax_enable_x64", False)

# 3. Batch data properly (use larger batches if memory allows)

# 4. Profile code
import jax.profiler
jax.profiler.start_trace("/tmp/tensorboard")
# ... run code ...
jax.profiler.stop_trace()
```

---

## Checklist Before Deployment

- [ ] Model converges on training set (can overfit single batch)
- [ ] Gradients checked numerically (at least once)
- [ ] No NaN/Inf in gradients during training
- [ ] Validation accuracy reasonable (>random chance)
- [ ] Model calibrated (calibration plot looks good)
- [ ] Tested on held-out test set
- [ ] Hyperparameters tuned (grid search or similar)
- [ ] Model saved and can be reloaded
- [ ] Inference time acceptable
- [ ] Memory usage acceptable

---

## References

- [JAX Documentation](https://jax.readthedocs.io/)
- [Flax Documentation](https://flax.readthedocs.io/)
- [Optax Documentation](https://optax.readthedocs.io/)
- Goodfellow et al. (2016). "Deep Learning" (textbook)
- He et al. (2015). "Delving Deep into Rectifiers" (initialization)
- Ioffe & Szegedy (2015). "Batch Normalization"

# t-SNE - Theory

## What is t-SNE?

**t-Distributed Stochastic Neighbor Embedding** - a non-linear dimensionality reduction technique primarily used for visualizing high-dimensional data in 2D or 3D space.

**Core idea**: Preserve local structure by making similar points in high-dimensional space remain close in low-dimensional space, while allowing dissimilar points to spread apart.

---

## Problem t-SNE Solves

**Challenge**: Visualizing high-dimensional data (e.g., 50+ dimensions) in 2D/3D

**Traditional approaches fail**:
- PCA: Linear, loses non-linear structure
- Random projection: No structure preservation
- Naive selection of 2 features: Loses information

**t-SNE solution**: Non-linear embedding that preserves neighborhood relationships

---

## Algorithm Overview

**Two-step process**:

1. **High-dimensional space**: Compute pairwise similarities using Gaussian kernel
2. **Low-dimensional space**: Compute pairwise similarities using Student t-distribution
3. **Optimization**: Minimize divergence between the two probability distributions

---

## Mathematical Foundation

### Step 1: High-Dimensional Similarities

For each point $x_i$, compute conditional probability that $x_j$ is its neighbor:

$$p_{j|i} = \frac{\exp(-||x_i - x_j||^2 / 2\sigma_i^2)}{\sum_{k \neq i}\exp(-||x_i - x_k||^2 / 2\sigma_i^2)}$$

**Symmetrize**:
$$p_{ij} = \frac{p_{j|i} + p_{i|j}}{2n}$$

**Interpretation**: $p_{ij}$ represents similarity between $x_i$ and $x_j$ in original space

### Step 2: Low-Dimensional Similarities

In embedded space (2D/3D), use Student t-distribution (heavy tails):

$$q_{ij} = \frac{(1 + ||y_i - y_j||^2)^{-1}}{\sum_{k \neq l}(1 + ||y_k - y_l||^2)^{-1}}$$

**Why t-distribution?**
- Heavy tails allow moderate distances in low-dim
- Prevents "crowding problem"
- Makes optimization easier

### Step 3: Optimization

**Minimize Kullback-Leibler divergence**:

$$C = KL(P||Q) = \sum_i\sum_j p_{ij}\log\frac{p_{ij}}{q_{ij}}$$

**Gradient**:
$$\frac{\delta C}{\delta y_i} = 4\sum_j(p_{ij} - q_{ij})(y_i - y_j)(1 + ||y_i - y_j||^2)^{-1}$$

**Optimization**: Gradient descent with momentum

---

## Perplexity

**Most important hyperparameter**

**Definition**: Measure of effective number of neighbors

**Formula**: $Perp(P_i) = 2^{H(P_i)}$ where $H(P_i) = -\sum_j p_{j|i}\log_2 p_{j|i}$

**Effect**:
- Balances attention between local and global structure
- Determines $\sigma_i$ for each point via binary search
- Typical values: 5-50 (common: 30)

**Practical guidance**:
- Small perplexity (5-10): Focuses on very local structure
- Medium perplexity (30-50): Balanced view (default)
- Large perplexity (100+): More global structure

**Rule of thumb**: Perplexity should be smaller than number of points

---

## Key Properties

### Preserves Local Structure
- Similar points in original space → close in embedding
- Dissimilar points → can be far apart
- Local neighborhoods maintained

### Non-Convex Optimization
- Multiple local minima exist
- Different runs produce different results
- Random initialization matters

### Not Preserving Distances
- Doesn't preserve global structure or distances
- Only relative similarities matter
- Clusters can appear at arbitrary distances

### Computational Complexity
- O(n²) in time and space (naive)
- O(n log n) with Barnes-Hut approximation
- Memory intensive for large n

---

## t-SNE vs PCA

| Aspect | t-SNE | PCA |
|--------|-------|-----|
| **Type** | Non-linear | Linear |
| **Preserves** | Local structure | Global variance |
| **Speed** | Slow O(n²) | Fast O(nd²) |
| **Deterministic** | No (random init) | Yes |
| **Interpretable axes** | No | Yes (principal components) |
| **Out-of-sample** | Difficult | Easy (transform) |
| **Best for** | Visualization, exploration | Preprocessing, compression |

---

## Hyperparameters

### 1. Perplexity (5-50)
**Effect**: Controls local vs global structure balance
- Too low: Overfits to local noise
- Too high: Misses local structure
- Default: 30

### 2. Learning Rate (10-1000)
**Effect**: Step size in gradient descent
- Too low: Very slow convergence
- Too high: Unstable, poor results
- Default: 200

### 3. Number of Iterations (250-1000)
**Effect**: Optimization duration
- Too few: Incomplete optimization
- Too many: Unnecessary computation
- Default: 1000

### 4. Early Exaggeration (4-12)
**Effect**: Multiplies $p_{ij}$ in early iterations
- Creates tight clusters early
- Prevents bad local minima
- Default: 12 for first 250 iterations

---

## Optimization Tricks

### 1. Early Exaggeration
Multiply $p_{ij}$ by 4-12 in first iterations:
- Encourages tight clusters early
- Natural spacing later
- Better final embedding

### 2. Momentum
Use momentum term in gradient descent:
- Faster convergence
- Escapes shallow local minima
- Typical: 0.5 initially, 0.8 after 250 iterations

### 3. Barnes-Hut Approximation
- Reduces O(n²) to O(n log n)
- Uses space partitioning trees
- Approximates far-field forces
- Enabled by default in sklearn for n > 1000

---

## Advantages

1. **Excellent visualization**: Best for exploring high-dim data
2. **Reveals structure**: Finds clusters and patterns
3. **Non-linear**: Captures complex manifolds
4. **Flexible**: Works on various data types
5. **Interpretable results**: Clear 2D/3D plots

---

## Disadvantages

1. **Slow**: O(n²) complexity, hours for large datasets
2. **No global structure**: Distances between clusters meaningless
3. **Hyperparameter sensitive**: Results vary with perplexity
4. **Non-deterministic**: Different runs give different results
5. **No out-of-sample**: Cannot project new points easily
6. **Not for reduction**: Only for visualization, not preprocessing

---

## When to Use t-SNE

### Use When:
- Visualizing high-dimensional data (exploration)
- Finding clusters visually
- Understanding data structure
- Presenting results to stakeholders
- Dataset size: 100 - 10,000 points

### Don't Use When:
- Need deterministic results
- Need to project new data
- Global distances matter
- Very large datasets (n > 50,000)
- Need interpretable dimensions
- Preprocessing for supervised learning (use PCA)

---

## Practical Tips

### Preprocessing:
1. **Scale features**: Critical for distance-based method
2. **Remove outliers**: Can distort embedding
3. **PCA first**: Reduce to 50 dimensions if starting > 50
4. **Handle missing**: Impute before t-SNE

### Parameter Tuning:
1. **Try multiple perplexities**: Plot 5, 30, 50
2. **Run multiple times**: Check consistency
3. **Sufficient iterations**: At least 1000
4. **Early exaggeration**: Keep at 12

### Interpretation:
1. **Cluster sizes meaningless**: Only proximity matters
2. **Distances between clusters meaningless**: Only within-cluster structure
3. **Run multiple times**: Ensure stability
4. **Compare with other methods**: Validate findings

### Common Mistakes:
1. **Over-interpreting**: Don't read too much into distances
2. **Single run**: Always run multiple times
3. **Wrong perplexity**: Too low or too high
4. **No preprocessing**: Forgetting to scale
5. **Too few iterations**: Stopping early

---

## Advanced Variants

### UMAP (Uniform Manifold Approximation and Projection)
- Faster than t-SNE
- Preserves global structure better
- More consistent across runs
- Better for large datasets

### Parametric t-SNE
- Uses neural network
- Can project new points
- More computationally expensive

---

## Key Questions

**Q: Why does t-SNE use different distributions for high-dim and low-dim?**
A: Gaussian in high-dim captures local similarities. Student t in low-dim has heavier tails, solving the "crowding problem" - allows moderate distances in 2D to represent larger distances in high-dim.

**Q: Can I use t-SNE for dimensionality reduction before machine learning?**
A: No. t-SNE is for visualization only. It doesn't preserve global structure, is non-deterministic, and can't project new data. Use PCA for preprocessing.

**Q: Why do different runs give different results?**
A: t-SNE optimization is non-convex with many local minima. Random initialization leads to different solutions. Run multiple times and check consistency.

**Q: What if I see one big cluster?**
A: Try higher perplexity, more iterations, or different initialization. Could also indicate data genuinely doesn't have clear clusters.

---

**Next**: See implementation for t-SNE visualization on MNIST and other datasets.

# UMAP - Theory

## What is UMAP?

**Uniform Manifold Approximation and Projection** - a dimension reduction technique that preserves both local and global structure, faster than t-SNE with more consistent results.

**Core idea**: Build high-dimensional graph representation of data, then optimize low-dimensional graph to be as similar as possible using fuzzy topology and Riemannian geometry.

---

## Key Advantages Over t-SNE

| Aspect | UMAP | t-SNE |
|--------|------|-------|
| **Speed** | Much faster | Slow |
| **Global structure** | Preserves better | Loses |
| **Scalability** | Handles millions | Struggles >10k |
| **Consistency** | More reproducible | High variance |
| **New data** | Can transform | Cannot |
| **Parameters** | n_neighbors, min_dist | Perplexity |

---

## Algorithm Overview

**Three main steps:**

1. **Construct graph** in high-dimensional space
   - Find k-nearest neighbors for each point
   - Weight edges by distance
   - Build fuzzy simplicial complex

2. **Initialize** low-dimensional embedding
   - Spectral embedding or random

3. **Optimize** low-dimensional layout
   - Minimize cross-entropy between graphs
   - Stochastic gradient descent

---

## Key Parameters

### 1. n_neighbors (5-50)
**Controls local vs global tradeoff**
- Small (5-15): Focuses on local structure, fine detail
- Medium (15-30): Balanced (default: 15)
- Large (50+): More global structure, broader view

**Similar to**: t-SNE's perplexity

### 2. min_dist (0.0-0.99)
**Controls embedding tightness**
- Small (0.0-0.1): Tight clusters, preserves topology
- Medium (0.1-0.5): Balanced (default: 0.1)
- Large (0.5+): Looser, more spread out

**Effect**: How tightly points pack together

### 3. metric
**Distance measure**
- Euclidean (default)
- Cosine (for high-dim sparse)
- Manhattan, Hamming, etc.

### 4. n_components
**Output dimensions**
- 2D: Visualization
- 3D: Interactive exploration
- Higher: Dimensionality reduction for ML

---

## UMAP vs t-SNE vs PCA

| Feature | UMAP | t-SNE | PCA |
|---------|------|-------|-----|
| **Preserves** | Local + Global | Local | Global variance |
| **Speed** | Fast | Slow | Fastest |
| **Deterministic** | More stable | Variable | Yes |
| **Scales** | Large data | Small data | Large data |
| **Transform new** | Yes | No | Yes |
| **Interpretable** | No | No | Yes |

---

## Manifold Learning

**Assumption**: High-dimensional data lies on lower-dimensional manifold

**UMAP approach**:
1. Assumes data uniformly distributed on manifold locally
2. Uses Riemannian geometry for distances on manifold
3. Builds fuzzy topological representation
4. Optimizes low-dim to match topology

---

## Mathematical Foundation

### High-Dimensional Graph

For each point, find k-nearest neighbors. Edge weight:

$$w_{ij} = \exp\left(-\frac{d(x_i, x_j) - \rho_i}{\sigma_i}\right)$$

Where:
- $\rho_i$: Distance to nearest neighbor
- $\sigma_i$: Normalization factor

### Low-Dimensional Graph

Similar but with different distribution:

$$v_{ij} = \frac{1}{1 + ad_{ij}^{2b}}$$

Where $a, b$ learned from min_dist

### Optimization

Minimize cross-entropy:

$$CE = \sum_{ij}w_{ij}\log\frac{w_{ij}}{v_{ij}} + (1-w_{ij})\log\frac{1-w_{ij}}{1-v_{ij}}$$

Use stochastic gradient descent

---

## Advantages

1. **Faster**: 10-100x faster than t-SNE
2. **Scalable**: Millions of points
3. **Global structure**: Preserves meaningful distances between clusters
4. **Consistent**: Less sensitive to parameters
5. **Transform**: Can project new data
6. **General purpose**: Both visualization and preprocessing
7. **Flexible metrics**: Many distance functions

---

## Disadvantages

1. **Newer**: Less established than t-SNE (2018)
2. **Parameter tuning**: Still requires experimentation
3. **Not deterministic**: Different runs vary (but less than t-SNE)
4. **Theoretical**: Complex mathematical foundation
5. **Interpretation**: No intuitive parameter meanings like perplexity

---

## When to Use UMAP

### Use When:
- Large datasets (>10k points)
- Need both local and global structure
- Want faster results than t-SNE
- Need to transform new data
- Preprocessing for supervised learning
- Consistent results important

### Don't Use When:
- Very small datasets (<100 points)
- Need deterministic results
- Linear methods suffice (PCA faster)
- Only local structure matters

---

## Practical Applications

**Visualization**:
- Single-cell RNA sequencing
- Large image datasets
- Document embeddings
- High-dimensional sensor data

**Preprocessing**:
- Feature reduction before classification
- Anomaly detection
- Semi-supervised learning
- Clustering high-dimensional data

---

## Practical Tips

### Parameter Selection:

**n_neighbors**:
- Start: 15 (default)
- Small datasets: 5-10
- Large datasets: 30-50
- Local focus: 5-10
- Global focus: 50-100

**min_dist**:
- Tight clusters: 0.0
- Balanced: 0.1 (default)
- Spread out: 0.5-0.99

**metric**:
- Dense numerical: Euclidean
- Sparse/text: Cosine
- Categorical: Hamming

### Preprocessing:
1. **Scale features**: Important for distance metrics
2. **Handle missing**: UMAP doesn't handle NaN
3. **High dimensions**: Can apply directly (no PCA needed)

### Optimization:
1. **Multiple runs**: Check consistency
2. **Compare parameters**: Grid search on n_neighbors
3. **Validate**: Check if structure makes sense

---

## Implementation Notes

**Libraries**:
- **umap-learn**: Official Python implementation
- Not in sklearn, separate package
- Install: `pip install umap-learn`

**Compatibility**:
- Follows sklearn API
- fit(), transform(), fit_transform()
- Works with sklearn pipelines

---

## Key Questions

**Q: When should I use UMAP instead of t-SNE?**
A: Use UMAP for larger datasets (>5k points), when you need global structure preservation, faster computation, or ability to transform new data. Use t-SNE only for small datasets where local structure is critical.

**Q: Can UMAP replace PCA for preprocessing?**
A: Sometimes. UMAP preserves non-linear structure better than PCA and can reduce to any dimension. But PCA is faster, deterministic, and interpretable. For preprocessing: PCA first, UMAP if needed.

**Q: Why does UMAP preserve global structure better than t-SNE?**
A: UMAP uses a different mathematical framework (fuzzy topology) that doesn't force all large distances to be equally large. t-SNE's heavy tails in low-dim make all between-cluster distances similar.

**Q: Is UMAP deterministic?**
A: No, but more consistent than t-SNE. Set random_state for reproducibility. Results vary less between runs than t-SNE due to better optimization.

---

**Next**: See demonstration comparing UMAP with t-SNE and PCA on real datasets.

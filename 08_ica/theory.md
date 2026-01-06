# Independent Component Analysis - Theory

## What is ICA?

**Independent Component Analysis** - a computational method for separating a multivariate signal into additive, independent non-Gaussian components.

**Core idea**: Find a linear transformation that makes components as statistically independent as possible.

---

## Problem: Blind Source Separation

**Cocktail Party Problem**: Multiple people speaking simultaneously, multiple microphones recording. Can we recover individual voices?

**Model**:
$$X = AS$$

Where:
- $X$: Observed mixed signals (m × n matrix)
- $A$: Unknown mixing matrix (m × m)
- $S$: Unknown source signals (m × n matrix)

**Goal**: Find unmixing matrix $W = A^{-1}$ such that:
$$Y = WX = S$$

---

## ICA vs PCA

| Aspect | ICA | PCA |
|--------|-----|-----|
| **Objective** | Maximize independence | Maximize variance |
| **Components** | Non-Gaussian, independent | Gaussian, uncorrelated |
| **Orthogonality** | Not required | Required |
| **Order** | Arbitrary | Ordered by variance |
| **Uniqueness** | Up to scaling/permutation | Unique |
| **Use case** | Signal separation | Dimensionality reduction |

**Key difference**: PCA finds uncorrelated components (2nd order), ICA finds independent components (higher order statistics)

---

## Statistical Independence

**Uncorrelated (PCA)**:
$$E[xy] = E[x]E[y]$$

**Independent (ICA)**: 
$$p(x,y) = p(x)p(y)$$

**Independence is stronger**: Independent implies uncorrelated, but not vice versa.

---

## Non-Gaussianity

**Central Limit Theorem**: Sum of independent variables → Gaussian

**ICA principle**: Mixtures are more Gaussian than sources

**Strategy**: Find directions of maximum non-Gaussianity

**Measures of non-Gaussianity**:
1. **Kurtosis**: $kurt(y) = E[y^4] - 3(E[y^2])^2$
2. **Negentropy**: $J(y) = H(y_{gauss}) - H(y)$

---

## FastICA Algorithm

**Most popular ICA algorithm** (Hyvärinen, 1999)

**Steps**:

1. **Center data**: $X \leftarrow X - E[X]$
2. **Whiten data**: Decorrelate and scale to unit variance
   - Use PCA or eigenvalue decomposition
   - $X \leftarrow EDX$ where $D$ is diagonal
3. **Iterative optimization**:
   - Initialize random weight vector $w$
   - Repeat:
     - $w \leftarrow E[Xg(w^TX)] - E[g'(w^TX)]w$
     - $w \leftarrow w / ||w||$
   - Until convergence
4. **Extract components**: $y = w^TX$

Where $g$ is non-linear function (e.g., $g(x) = \tanh(x)$)

---

## Assumptions

1. **Components are independent**: $p(s) = \prod_i p(s_i)$
2. **Components are non-Gaussian**: At most one can be Gaussian
3. **Mixing matrix is square and invertible**: m sources, m observations
4. **Sources are stationary**: Statistics don't change over time

---

## Limitations

1. **Cannot determine order**: Components returned in arbitrary order
2. **Cannot determine scale**: $AS = (αA)(S/α)$ for any α
3. **Cannot determine sign**: $AS = (-A)(-S)$
4. **Requires non-Gaussianity**: Fails if all sources Gaussian
5. **Assumes linear mixing**: Non-linear mixing requires non-linear ICA

---

## Applications

### Audio Processing
- **Cocktail party problem**: Separate mixed voices
- **Music source separation**: Isolate instruments
- **Echo cancellation**: Remove reverberations

### Biomedical
- **EEG/MEG analysis**: Separate brain signals
- **fMRI analysis**: Identify brain networks
- **ECG artifact removal**: Clean heart signals

### Image Processing
- **Feature extraction**: Find independent features
- **Denoising**: Separate signal from noise

### Finance
- **Factor analysis**: Identify independent market factors
- **Portfolio optimization**: Uncorrelated risk sources

---

## Practical Tips

### Preprocessing:
1. **Center data**: Remove mean (required)
2. **Whiten data**: Decorrelate (required for FastICA)
3. **Remove outliers**: Can affect non-Gaussianity measures

### Number of Components:
- **Equal to sources**: If known
- **Use PCA**: Reduce to significant components first
- **Try multiple**: Validate with domain knowledge

### Algorithm Selection:
- **FastICA**: Fast, robust, most popular
- **Infomax**: Maximum likelihood approach
- **JADE**: Uses 4th-order cumulants

### Validation:
1. **Visual inspection**: Plot recovered signals
2. **Independence test**: Check correlation matrix
3. **Domain knowledge**: Do components make sense?

---

## ICA Variants

### Temporal ICA
- Exploits temporal structure
- Better for time series

### Non-negative ICA
- Constrains components to be positive
- Useful for image/text data

### Non-linear ICA
- Handles non-linear mixing
- More complex, less stable

---

## Key Questions

**Q: When to use ICA vs PCA?**
A: Use ICA when you want to separate independent sources (e.g., mixed audio signals). Use PCA when you want to reduce dimensions while preserving variance.

**Q: Why does ICA need non-Gaussian sources?**
A: Gaussian variables have all higher-order statistics zero. Only mean and covariance matter. ICA uses higher-order statistics to find independence, so Gaussian sources are unidentifiable.

**Q: Can ICA handle more observations than sources?**
A: Yes (overcomplete ICA), but standard algorithms assume square mixing matrix. Use dimensionality reduction first or specialized algorithms.

**Q: How to choose number of components?**
A: Use PCA to find effective dimensionality (explained variance), then set ICA components equal to significant PCA components.

---

**Next**: See implementation for blind source separation demonstrations.

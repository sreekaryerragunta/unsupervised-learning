# Principal Component Analysis - Theory

## What is PCA?

**Principal Component Analysis** - a dimensionality reduction technique that transforms data into a new coordinate system where the axes (principal components) capture maximum variance.

**Core idea**: Find orthogonal directions in high-dimensional space that capture the most variance in the data, allowing us to reduce dimensions while preserving information.

---

## Why PCA?

**The Curse of Dimensionality**: As dimensions increase, data becomes sparse and distances become less meaningful.

**PCA Solutions**:
- Reduce computational cost
- Remove noise and redundancy
- Visualize high-dimensional data
- Improve algorithm performance

---

## Mathematical Foundation

### Variance and Covariance

**Variance**: Spread of data along one dimension
$$Var(X) = \frac{1}{n}\sum_{i=1}^{n}(x_i - \bar{x})^2$$

**Covariance**: How two variables change together
$$Cov(X,Y) = \frac{1}{n}\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})$$

**Covariance Matrix**: $C_{ij} = Cov(X_i, X_j)$

---

## PCA Algorithm

**Step 1**: Center the data
$$X_{centered} = X - \bar{X}$$

**Step 2**: Compute covariance matrix
$$C = \frac{1}{n}X_{centered}^T X_{centered}$$

**Step 3**: Find eigenvalues and eigenvectors
$$C v = \lambda v$$

**Step 4**: Sort by eigenvalue (largest first)

**Step 5**: Select top k eigenvectors

**Step 6**: Transform data
$$X_{pca} = X_{centered} \cdot V_k$$

---

## Eigenvectors and Eigenvalues

**Eigenvector**: Direction that doesn't change under transformation
**Eigenvalue**: How much variance is captured in that direction

**Larger eigenvalue** = More important principal component

---

## Variance Explained

**Total variance**: Sum of all eigenvalues
$$\text{Total Variance} = \sum_{i=1}^{d}\lambda_i$$

**Variance explained by component k**:
$$\frac{\lambda_k}{\sum_{i=1}^{d}\lambda_i}$$

**Cumulative variance**: How much information retained with k components

---

## Choosing Number of Components

**Methods**:

1. **Variance Threshold**: Keep components until X% variance explained (e.g., 95%)
2. **Elbow Method**: Plot explained variance, look for "elbow"
3. **Kaiser Rule**: Keep components with eigenvalue > 1
4. **Cross-Validation**: Test on downstream task

---

## PCA vs Other Methods

| Method | Linear | Supervised | Preserves | Use Case |
|--------|--------|------------|-----------|----------|
| **PCA** | Yes | No | Global variance | Dimensionality reduction |
| **LDA** | Yes | Yes | Class separation | Classification |
| **t-SNE** | No | No | Local structure | Visualization |
| **UMAP** | No | No | Local + Global | Visualization |
| **Autoencoder** | No | No | Non-linear patterns | Complex data |

---

## Advantages

1. **Removes correlation**: Principal components are orthogonal
2. **Reduces noise**: Minor components often contain noise
3. **Speeds up algorithms**: Fewer features = faster training
4. **Visualization**: Project to 2D/3D
5. **Interpretable**: Based on variance
6. **No parameters**: Data-driven

---

## Disadvantages

1. **Linear method**: Can't capture non-linear relationships
2. **Loses meaning**: Components are combinations of original features
3. **Sensitive to scale**: Must standardize features
4. **Assumes linearity**: May miss complex patterns
5. **Outliers affect**: Extreme values influence components

---

## Practical Tips

### Preprocessing:
1. **Standardize**: Critical! Scale features to mean=0, std=1
2. **Handle missing**: Impute before PCA
3. **Remove outliers**: Can dominate components

### Component Selection:
- **Visualization**: 2-3 components
- **ML preprocessing**: 95% variance threshold
- **Noise reduction**: Remove last few components

### Validation:
- Check variance explained
- Visualize first few components
- Test downstream task performance

---

## Applications

**Data Compression**:
- Image compression
- Feature extraction
- Storage optimization

**Visualization**:
- Exploratory data analysis
- Cluster visualization
- Anomaly detection

**Preprocessing**:
- Before classification
- Remove multicollinearity
- Speed up training

**Noise Reduction**:
- Signal processing
- Image denoising
- Feature cleaning

---

## Key Questions

**Q: When should I use PCA?**
A: Use PCA when you have high-dimensional data with correlated features and want to reduce dimensions while preserving variance. Best for linear relationships.

**Q: How many components should I keep?**
A: Common rule: keep components that explain 95% of variance. For visualization, use 2-3. Validate with downstream task.

**Q: Should I always standardize?**
A: Yes! PCA is sensitive to scale. Always standardize unless features are already on same scale and you want to preserve that information.

**Q: Can PCA handle non-linear data?**
A: No, standard PCA is linear. For non-linear data, use kernel PCA, t-SNE, UMAP, or autoencoders.

---

**Next**: See implementation for eigendecomposition and practical demonstrations.

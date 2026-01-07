# Anomaly Detection - Theory

## What is Anomaly Detection?

**Anomaly Detection** - the task of identifying rare observations that differ significantly from the majority of data (also called outlier detection).

**Core idea**: Normal data follows patterns; anomalies deviate from these patterns in measurable ways.

---

## Types of Anomalies

### Point Anomalies
Individual data points that are anomalous with respect to the rest of the data.
*Example*: A single fraudulent transaction among normal purchases.

### Contextual Anomalies
Data points that are anomalous in a specific context but not globally.
*Example*: High temperature is normal in summer but anomalous in winter.

### Collective Anomalies
A collection of related data points that together are anomalous.
*Example*: A sequence of normal transactions that together indicate fraud.

---

## Methods

### 1. Statistical Methods

**Z-Score**: Points with |z| > 3 are often anomalies.
$$z = \frac{x - \mu}{\sigma}$$

**IQR Method**: Points outside [Q1 - 1.5*IQR, Q3 + 1.5*IQR] are outliers.

**Pros**: Simple, interpretable
**Cons**: Assumes normal distribution, fails with multivariate data

---

### 2. Isolation Forest

**Key Insight**: Anomalies are easier to isolate (require fewer splits).

**Algorithm**:
1. Build random trees by randomly selecting features and split points
2. Anomalies have shorter average path lengths
3. Anomaly score based on average path length across trees

**Advantages**:
- Scales well to high dimensions
- Handles large datasets efficiently
- No distance calculations needed

---

### 3. One-Class SVM

**Approach**: Learn a decision boundary around normal data.

**Algorithm**:
1. Map data to high-dimensional space via kernel
2. Find hyperplane that separates data from origin with maximum margin
3. Points on wrong side of boundary are anomalies

**Advantages**:
- Works with non-linear boundaries
- Effective for novelty detection

**Disadvantages**:
- Sensitive to hyperparameters (nu, gamma)
- Computationally expensive for large datasets

---

### 4. Local Outlier Factor (LOF)

**Approach**: Compare local density of a point to its neighbors.

**Algorithm**:
1. Calculate k-distance for each point
2. Compute local reachability density
3. LOF = ratio of average neighbor density to point's density

**LOF Interpretation**:
- LOF ≈ 1: Similar density to neighbors (normal)
- LOF > 1: Lower density than neighbors (outlier)
- LOF < 1: Higher density than neighbors (core point)

---

### 5. DBSCAN-based Detection

Points labeled as noise (-1) by DBSCAN are considered anomalies.

**Advantages**: Natural byproduct of clustering
**Disadvantages**: Requires careful epsilon/minPts tuning

---

## Evaluation Metrics

### When labels are available:
- Precision, Recall, F1-Score
- ROC-AUC
- Precision-Recall Curve (important for imbalanced data)

### When labels are unavailable:
- Visual inspection
- Domain expert validation
- Silhouette-like scores for anomaly methods

---

## Choosing a Method

| Method | Best For | Scalability | Interpretability |
|--------|----------|-------------|------------------|
| **Statistical** | Simple, univariate data | High | High |
| **Isolation Forest** | High-dimensional, large datasets | High | Medium |
| **One-Class SVM** | Small datasets, complex boundaries | Low | Low |
| **LOF** | Density-based, local anomalies | Medium | Medium |
| **DBSCAN** | When clustering is also needed | Medium | Medium |

---

## Applications

- **Fraud Detection**: Credit card, insurance claims
- **Network Security**: Intrusion detection, DDoS attacks
- **Manufacturing**: Defect detection, predictive maintenance
- **Healthcare**: Disease outbreak, unusual patient vitals
- **Finance**: Market manipulation, unusual trading patterns

---

## Key Questions

**Q: How is anomaly detection different from classification?**
A: Classification requires labeled examples of all classes. Anomaly detection typically only has normal samples for training (one-class learning) or no labels at all (unsupervised).

**Q: How do you handle class imbalance in anomaly detection?**
A: Use metrics like precision-recall instead of accuracy. Anomalies are rare by definition, so ROC-AUC or PR-AUC are better measures.

**Q: When should you use Isolation Forest vs One-Class SVM?**
A: Use Isolation Forest for large, high-dimensional datasets. Use One-Class SVM for smaller datasets where you need precise boundary control.

---

**Next**: See demonstration notebook for practical implementations using scikit-learn.

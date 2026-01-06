# K-Means Clustering - Theory

## What is K-Means?

An **unsupervised learning** algorithm that partitions data into K distinct clusters based on feature similarity.

**Goal**: Minimize within-cluster variance (make points in same cluster close to each other).

---

## Algorithm

1. **Initialize**: Randomly select K points as initial centroids
2. **Assign**: Assign each point to nearest centroid
3. **Update**: Recalculate centroids as mean of assigned points
4. **Repeat**: Steps 2-3 until convergence (centroids don't change)

---

## Objective Function

Minimize **within-cluster sum of squares (WCSS)**:
$$J = \sum_{i=1}^{K}\sum_{x \in C_i} ||x - \mu_i||^2$$

- $C_i$: Cluster $i$
- $\mu_i$: Centroid of cluster $i$
- $||x - \mu_i||^2$: Squared Euclidean distance

---

## Key Hyperparameter: K

**Elbow Method**: Plot WCSS vs K, look for "elbow" where decrease slows

**Silhouette Score**: Measures how similar points are to own cluster vs other clusters

---

## Advantages

1. **Simple and fast**: O(nKd) complexity
2. **Scales well**: Works on large datasets
3. **Guaranteed convergence**: Always reaches local minimum

---

## Disadvantages

1. **Must specify K**: Need to know number of clusters
2. **Sensitive to initialization**: Different starts lead to different results
3. **Assumes spherical clusters**: Struggles with elongated shapes
4. **Sensitive to outliers**: Outliers affect centroids

---

**Key Point:** "K-Means iteratively assigns points to nearest centroids and updates centroids until convergence. It's fast and effective for spherical clusters but sensitive to initialization and outlier values."

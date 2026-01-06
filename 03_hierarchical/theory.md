# Hierarchical Clustering - Theory

## What is Hierarchical Clustering?

A clustering method that builds a hierarchy of clusters, creating a tree-like structure (dendrogram) showing relationships between data points at different levels of granularity.

**Core idea**: Iteratively merge (agglomerative) or split (divisive) clusters based on proximity, creating a nested hierarchy.

---

## Two Approaches

### Agglomerative (Bottom-Up)
**Process**:
1. Start: Each point is its own cluster (n clusters)
2. Repeat: Merge two closest clusters
3. End: All points in one cluster (1 cluster)

**Most common approach** - we'll focus on this.

### Divisive (Top-Down)
**Process**:
1. Start: All points in one cluster
2. Repeat: Split a cluster into two
3. End: Each point is its own cluster

**Less common** - computationally more expensive.

---

## Algorithm (Agglomerative)

```
1. Initialize: Each point as its own cluster
2. Compute: Distance matrix between all clusters
3. While more than 1 cluster:
   a. Find two closest clusters
   b. Merge them into single cluster
   c. Update distance matrix
4. Output: Dendrogram showing merge history
```

**Time Complexity**: O(n³) naive, O(n² log n) optimized

---

## Linkage Criteria

**How to measure distance between clusters?**

### Single Linkage (MIN)
$$d(C_1, C_2) = \min_{x \in C_1, y \in C_2} d(x, y)$$

**Definition**: Distance between closest points in clusters

**Properties**:
- Creates chain-like clusters
- Sensitive to noise (chaining effect)
- Good for non-spherical clusters
- Can produce elongated clusters

**Use when**: Clusters have irregular shapes

### Complete Linkage (MAX)
$$d(C_1, C_2) = \max_{x \in C_1, y \in C_2} d(x, y)$$

**Definition**: Distance between farthest points in clusters

**Properties**:
- Creates compact, spherical clusters
- Less sensitive to outliers than single
- Prefers equally-sized clusters
- Breaks large clusters early

**Use when**: Want compact, well-separated clusters

### Average Linkage (MEAN)
$$d(C_1, C_2) = \frac{1}{|C_1||C_2|} \sum_{x \in C_1}\sum_{y \in C_2} d(x, y)$$

**Definition**: Average distance between all pairs

**Properties**:
- Balance between single and complete
- Less extreme than both
- Most commonly used in practice
- Produces moderate compactness

**Use when**: General-purpose clustering

### Ward's Linkage
$$d(C_1, C_2) = \frac{|C_1||C_2|}{|C_1|+|C_2|} ||m_1 - m_2||^2$$

**Definition**: Increase in sum of squared errors (SSE) when merging

**Properties**:
- Minimizes within-cluster variance
- Similar to K-Means objective
- Creates compact, equal-sized clusters
- Most popular for general use

**Use when**: Want to minimize variance (default choice)

---

## Dendrogram

**Visualization**: Tree diagram showing cluster merges

**Components**:
- **Leaves**: Individual data points
- **Branches**: Cluster merges
- **Height**: Distance at which merge occurs
- **Horizontal cut**: Defines number of clusters

**Interpretation**:
- Height of merge = dissimilarity between clusters
- Longer vertical lines = more distinct clusters
- Cut dendrogram at desired height to get k clusters

---

## Choosing Number of Clusters

### 1. Dendrogram Inspection
- Look for large gaps in merge heights
- Cut before a large merge (long vertical line)
- Visual, subjective method

### 2. Elbow Method
- Plot within-cluster sum of squares vs k
- Look for elbow (diminishing returns)

### 3. Silhouette Score
- Measure cluster quality for different k
- Higher score = better clustering
- Range: [-1, 1], aim for > 0.5

### 4. Domain Knowledge
- Use expected number of categories
- Business requirements

---

## Distance Metrics

**Between individual points:**

**Euclidean**:
$$d(x, y) = \sqrt{\sum_{i=1}^{n}(x_i - y_i)^2}$$
- Most common
- Assumes features on similar scales

**Manhattan**:
$$d(x, y) = \sum_{i=1}^{n}|x_i - y_i|$$
- Less sensitive to outliers
- Grid-based distances

**Cosine**:
$$d(x, y) = 1 - \frac{x \cdot y}{||x|| ||y||}$$
- Measures angle, not magnitude
- Good for high-dimensional sparse data

---

## Advantages

1. **No K required**: Dendrogram shows all possible clusterings
2. **Hierarchical structure**: Captures nested relationships
3. **Deterministic**: Same result every time
4. **Any distance metric**: Flexible
5. **Interpretable**: Dendrogram provides insights
6. **Works with small n**: Effective on small datasets

---

## Disadvantages

1. **Computationally expensive**: O(n²) memory, O(n² log n) time
2. **Doesn't scale**: Impractical for large datasets (n > 10,000)
3. **Sensitive to noise**: Outliers can distort hierarchy
4. **Cannot undo merges**: Greedy algorithm, no backtracking
5. **Space complexity**: Needs distance matrix in memory

---

## Hierarchical vs K-Means

| Aspect | Hierarchical | K-Means |
|--------|--------------|---------|
| **K required** | No (choose from dendrogram) | Yes |
| **Scalability** | Poor (O(n² log n)) | Better (O(nkd)) |
| **Deterministic** | Yes | No (random init) |
| **Cluster shape** | Flexible (linkage-dependent) | Spherical |
| **Output** | Full hierarchy | Flat partitions |
| **Best for** | Small datasets, hierarchy needed | Large datasets, efficiency |

---

## Practical Applications

### Biology
- Phylogenetic trees (evolutionary relationships)
- Gene expression analysis
- Species taxonomy

### Document Analysis
- Document organization
- Topic hierarchies
- Webpage categorization

### Image Analysis
- Image segmentation
- Object recognition
- Feature hierarchies

### Social Networks
- Community detection
- User segmentation
- Influence hierarchies

---

## Practical Tips

### Preprocessing:
1. **Scale features**: Critical as with all distance-based methods
2. **Handle outliers**: Remove or cap extreme values
3. **Dimension reduction**: PCA before hierarchical if high-dim

### Linkage Selection:
1. **Default**: Start with Ward's linkage
2. **Non-spherical**: Try single linkage
3. **Outlier-prone**: Use complete or average linkage
4. **Compare**: Try multiple, use silhouette score

### Efficiency:
1. **Sample first**: Cluster sample, then assign remaining points
2. **Use optimized libraries**: scipy.cluster.hierarchy is fast
3. **Consider alternatives**: If n > 10,000, use K-Means or DBSCAN

### Dendrogram Reading:
1. **Height matters**: Larger gaps = more distinct clusters
2. **Color coding**: Can help identify clusters visually
3. **Truncate**: For large datasets, show only top merges

---

## Advanced Variants

### BIRCH
**Balanced Iterative Reducing and Clustering using Hierarchies**
- Handles large datasets
- Uses CF-tree (Clustering Feature tree)
- O(n) time complexity
- Good for preprocessing before hierarchical

### CURE
**Clustering Using Representatives**
- Uses representative points instead of centroids
- Better for non-spherical clusters
- More robust to outliers

---

## Key Questions

**Q: How to choose linkage criterion?**
A: Ward's is best default (minimizes variance). Use single for non-spherical clusters, complete for compact clusters, average for balance. Try multiple and compare with silhouette score.

**Q: When does hierarchical fail?**
A: Large datasets (slow), high noise (distorts hierarchy), or when you only need flat partitioning. Use K-Means or DBSCAN instead.

**Q: How to interpret dendrogram height?**
A: Height of horizontal line represents distance/dissimilarity at which clusters merged. Larger heights = clusters more different. Cut dendrogram at height to get desired number of clusters.

**Q: Can hierarchical clustering handle non-convex shapes?**
A: Yes with single linkage (follows chain), but creates chaining effect. For arbitrary shapes without chaining, DBSCAN is better choice.

---

**Next**: See implementation for agglomerative clustering with dendrogram visualization.

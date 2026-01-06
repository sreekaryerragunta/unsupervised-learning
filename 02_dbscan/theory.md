# DBSCAN - Theory

## What is DBSCAN?

**Density-Based Spatial Clustering of Applications with Noise** - a clustering algorithm that groups together points that are closely packed, marking points in low-density regions as outliers.

**Core idea**: Clusters are dense regions of points separated by regions of low density. Points in sparse areas are noise/outliers.

---

## Key Concepts

### 1. Epsilon (ε)
**Definition**: Maximum distance between two points to be considered neighbors.

**Effect**:
- Small ε: More clusters, more noise
- Large ε: Fewer clusters, less noise
- Too small: Everything becomes noise
- Too large: Everything becomes one cluster

### 2. MinPts
**Definition**: Minimum number of points required to form a dense region (cluster).

**Effect**:
- Small MinPts: More clusters, less noise
- Large MinPts: Fewer clusters, more noise
- Rule of thumb: MinPts ≥ dimensionality + 1

### 3. Point Classifications

**Core Point**:
- Has at least MinPts points within ε distance (including itself)
- Forms the interior of clusters

**Border Point**:
- Has fewer than MinPts neighbors
- But is within ε distance of a core point
- On the edge of clusters

**Noise Point**:
- Neither core nor border
- Isolated in low-density regions
- Outliers

---

## Algorithm

```
1. For each point P:
   a. Find all points within ε distance (ε-neighborhood)
   b. If |ε-neighborhood| ≥ MinPts:
      - Mark P as core point
   
2. For each core point P:
   a. Create new cluster if not assigned
   b. Add all density-reachable points to cluster:
      - Direct neighbors in ε-neighborhood
      - Recursively add neighbors of core points
   
3. Assign border points to nearest clusters

4. Mark remaining points as noise
```

**Time Complexity**: O(n²) naive, O(n log n) with spatial indexing

---

## Density Reachability

**Directly Density-Reachable**:
- Point q is directly density-reachable from p if:
  - p is core point
  - q is in ε-neighborhood of p

**Density-Reachable**:
- Point q is density-reachable from p if there's a chain of directly density-reachable points from p to q

**Density-Connected**:
- Points p and q are density-connected if both are density-reachable from some point o

**Cluster**: Maximal set of density-connected points

---

## DBSCAN vs K-Means

| Aspect | DBSCAN | K-Means |
|--------|--------|---------|
| **K required** | No | Yes |
| **Cluster shape** | Arbitrary | Spherical |
| **Outlier detection** | Yes (noise) | No |
| **Density** | Finds dense regions | Assumes equal density |
| **Scalability** | O(n²) or O(n log n) | O(knd) - faster |
| **Reproducibility** | Deterministic* | Non-deterministic |
| **Parameter tuning** | ε and MinPts | K and init method |

*Border points may be assigned to different clusters, but core structure is deterministic

---

## Advantages

1. **No K required**: Automatically determines number of clusters
2. **Arbitrary shapes**: Can find non-spherical clusters
3. **Outlier detection**: Identifies noise points explicitly
4. **Robust**: Doesn't assume cluster shapes or sizes
5. **One scan**: Single pass through data with neighbors
6. **Intuitive parameters**: ε and MinPts have clear meaning

---

## Disadvantages

1. **Parameter sensitivity**: Results vary significantly with ε and MinPts
2. **Varying densities**: Struggles when clusters have different densities
3. **High dimensions**: Distance becomes less meaningful (curse of dimensionality)
4. **Memory**: Needs to store distance matrix or spatial index
5. **Border points**: Can belong to multiple clusters (ambiguity)

---

## Parameter Selection

### Epsilon (ε)

**K-distance plot method**:
1. For each point, compute distance to kth nearest neighbor (k = MinPts)
2. Sort distances in descending order
3. Plot sorted distances
4. Choose ε at "elbow" (sharp increase in distance)

**Domain knowledge**:
- Geographic data: Use meaningful distance (e.g., 500 meters)
- Normalized data: Start with ε = 0.5

### MinPts

**Rule of thumb**:
- MinPts ≥ dimensionality + 1
- For 2D data: MinPts = 4
- For noisy data: Higher MinPts (filter noise)
- For low-noise data: Lower MinPts (find small clusters)

**Common values**: 4, 5, 10

---

## Spatial Indexing

**Problem**: Computing all pairwise distances is O(n²)

**Solutions**:
1. **KD-Tree**: O(n log n) for low dimensions (d < 20)
2. **Ball Tree**: Better for high dimensions
3. **R-Tree**: Good for geographic data
4. **Grid-based**: Discretize space into cells

**sklearn uses**: Ball Tree by default

---

## Handling Varying Densities

**Problem**: Single ε assumes uniform density across dataset

**Solutions**:

**OPTICS** (Ordering Points To Identify Clustering Structure):
- Generalization of DBSCAN
- Produces reachability plot
- Can extract clusters at multiple densities

**HDBSCAN** (Hierarchical DBSCAN):
- Builds hierarchy of clusters
- Extracts stable clusters
- Automatic parameter selection

**Variable ε**:
- Use different ε for different regions
- Adaptive/local density estimation

---

## Applications

### Geographic Data
- Finding hotspots in crime data
- Customer location clustering
- Disease outbreak detection

### Anomaly Detection
- Network intrusion detection
- Fraud detection
- Manufacturing defect detection

### Image Segmentation
- Separating objects from background
- Color-based segmentation

### Astronomy
- Galaxy formation detection
- Star cluster identification

---

## Practical Tips

### Preprocessing:
1. **Scale features**: DBSCAN uses distance, scaling is critical
2. **Remove duplicates**: Can cause issues with MinPts
3. **Handle missing values**: Impute or remove

### Parameter Tuning:
1. **Start with k-distance plot**: Find ε systematically
2. **MinPts = 2 × dimensions**: Good default
3. **Try multiple values**: Use silhouette score to evaluate
4. **Visual inspection**: Plot results, check if makes sense

### When DBSCAN Fails:
1. **Varying densities**: Use HDBSCAN instead
2. **Too many noise points**: Decrease MinPts or increase ε
3. **Too few clusters**: Increase ε or decrease MinPts
4. **High dimensions**: Apply PCA first

---

## Evaluation Metrics

**Cannot use standard metrics** (no ground truth often):

1. **Silhouette Score**: Measures cluster cohesion and separation
2. **Davies-Bouldin Index**: Lower is better
3. **Visual inspection**: Most important for DBSCAN
4. **Domain validation**: Ask domain experts

**If ground truth available**:
- Adjusted Rand Index (ARI)
- Normalized Mutual Information (NMI)

---

## Key Questions

**Q: How to choose ε without ground truth?**
A: Use k-distance plot - compute distance to MinPts-th nearest neighbor for all points, sort, plot. Choose ε at the "elbow" where distances sharply increase.

**Q: What if clusters have different densities?**
A: Standard DBSCAN struggles. Use HDBSCAN which handles varying densities through hierarchical approach, or adjust ε locally.

**Q: Why is scaling critical for DBSCAN?**
A: DBSCAN uses Euclidean distance. If one feature ranges 0-100 and another 0-1, distance is dominated by the larger scale, making ε meaningless.

**Q: Can DBSCAN find nested clusters?**
A: No, DBSCAN produces flat partitioning. For nested structures, use hierarchical clustering or OPTICS/HDBSCAN which provide hierarchical information.

---

**Next**: See implementation for hands-on DBSCAN with k-distance plots and parameter tuning.

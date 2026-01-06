# Unsupervised Learning - From Scratch

A comprehensive collection of **unsupervised learning algorithms** implemented from scratch in Python with NumPy, demonstrating deep understanding of clustering, dimensionality reduction, and pattern discovery techniques.

**Repository Focus**: Building intuition for unsupervised learning through clear implementations, mathematical foundations, and visual demonstrations.

---

## 📚 Repository Structure

```
unsupervised-learning/
├── 01_kmeans/                    # K-Means Clustering
├── 02_dbscan/                    # Density-Based Clustering
├── 03_hierarchical/              # Hierarchical Clustering
├── 04_gmm/                       # Gaussian Mixture Models
├── 05_pca/                       # Principal Component Analysis
├── 06_tsne/                      # t-SNE Visualization
├── 07_umap/                      # UMAP Dimensionality Reduction
├── 08_ica/                       # Independent Component Analysis
├── requirements.txt
└── README.md
```

---

## 🎯 Algorithms Implemented

### Clustering Algorithms (4)

**1. K-Means**
- Centroid-based partitioning
- Elbow method for K selection
- K-Means++ initialization
- **Use case**: Customer segmentation, image compression

**2. DBSCAN** (Density-Based Spatial Clustering)
- Density-based clustering
- Automatic outlier detection
- No need to specify K
- **Use case**: Anomaly detection, geographic clustering

**3. Hierarchical Clustering**
- Agglomerative (bottom-up)
- Dendrogram visualization
- Linkage methods: single, complete, average
- **Use case**: Taxonomy creation, document organization

**4. Gaussian Mixture Models (GMM)**
- Probabilistic clustering
- EM algorithm
- Soft cluster assignments
- **Use case**: Image segmentation, speaker recognition

---

### Dimensionality Reduction (4)

**5. PCA** (Principal Component Analysis)
- Linear dimensionality reduction
- Variance preservation
- Eigenvalue decomposition
- **Use case**: Feature reduction, visualization, noise filtering

**6. t-SNE** (t-Distributed Stochastic Neighbor Embedding)
- Non-linear visualization
- Preserves local structure
- Perplexity tuning
- **Use case**: High-dimensional data visualization, exploratory analysis

**7. UMAP** (Uniform Manifold Approximation and Projection)
- Faster than t-SNE
- Preserves both local and global structure
- Manifold learning
- **Use case**: Large dataset visualization, preprocessing for ML

**8. ICA** (Independent Component Analysis)
- Signal separation
- Non-Gaussian components
- Blind source separation
- **Use case**: Audio separation, fMRI analysis

---

## 🔬 What's Included

Each algorithm component contains:

### 📖 Theory (`theory.md`)
- Mathematical foundations
- Algorithm intuition
- When to use vs when not to
- Hyperparameter guidance
- Comparison with alternatives
- Key questions and answers

### 💻 Implementation (`*_from_scratch.ipynb`)
- From-scratch NumPy implementation
- Step-by-step walkthrough
- sklearn comparison (validation)
- Code matches theory

### 📊 Demonstrations (`*.ipynb`)
- Visual explanations
- Real-world datasets
- Hyperparameter effects
- Practical examples
- Interactive visualizations

### 🛠️ Utilities (`utils.py`)
- Reusable helper functions
- Distance metrics
- Evaluation metrics
- Visualization tools

---

## 🚀 Quick Start

```bash
# Clone repository
git clone https://github.com/sreekaryerragunta/unsupervised-learning.git
cd unsupervised-learning

# Install dependencies
pip install -r requirements.txt

# Launch Jupyter
jupyter notebook
```

### Run an Algorithm

```python
# Example: K-Means
from sklearn.datasets import make_blobs
import sys
sys.path.append('01_kmeans')
from kmeans_from_scratch import KMeans

# Generate data
X, _ = make_blobs(n_samples=300, centers=3, random_state=42)

# Fit K-Means
kmeans = KMeans(n_clusters=3)
kmeans.fit(X)
labels = kmeans.predict(X)
```

---

## 📊 Key Concepts

### Clustering
**Goal**: Group similar data points together without labels

**Applications**:
- Customer segmentation (marketing)
- Image segmentation (computer vision)
- Anomaly detection (fraud, network intrusion)
- Document organization (NLP)
- Gene expression analysis (bioinformatics)

### Dimensionality Reduction
**Goal**: Reduce number of features while preserving information

**Applications**:
- Visualization (3D/2D projection)
- Noise reduction
- Feature engineering
- Compression
- Speeding up supervised learning

---

## 🎓 Learning Path

### Beginner
1. **Start**: K-Means (simplest clustering)
2. **Then**: PCA (linear dimensionality reduction)
3. **Practice**: Apply to real datasets

### Intermediate
4. **Density-based**: DBSCAN (handles arbitrary shapes)
5. **Hierarchical**: Dendrogram interpretation
6. **Visualization**: t-SNE for exploration

### Advanced
7. **Probabilistic**: GMM with EM algorithm
8. **Manifold learning**: UMAP for large datasets
9. **Signal processing**: ICA for separation

---

## 📈 Comparison Guide

### When to use which clustering?

| Algorithm | Best For | Cluster Shape | K Required? | Handles Noise? |
|-----------|----------|---------------|-------------|----------------|
| **K-Means** | Spherical clusters, fast | Spherical | Yes | No |
| **DBSCAN** | Arbitrary shapes, outliers | Any | No | Yes |
| **Hierarchical** | Taxonomy, small datasets | Any | No | Moderate |
| **GMM** | Soft assignments, probability | Elliptical | Yes | Moderate |

### When to use which dimensionality reduction?

| Algorithm | Best For | Linear? | Speed | Preserves |
|-----------|----------|---------|-------|-----------|
| **PCA** | Feature reduction, compression | Yes | Fast | Global variance |
| **t-SNE** | Visualization, exploration | No | Slow | Local structure |
| **UMAP** | Large datasets, preprocessing | No | Fast | Local + Global |
| **ICA** | Signal separation | Yes | Moderate | Independence |

---

## 🛠️ Tech Stack

- **Python 3.8+**
- **NumPy**: Core computations
- **Matplotlib**: Visualizations
- **Seaborn**: Statistical plots
- **scikit-learn**: Validation & comparison
- **Jupyter**: Interactive notebooks

---

## 📝 Design Philosophy

### 1. **Clarity Over Brevity**
- Explicit implementations over one-liners
- Detailed comments explaining each step
- Mathematical notation linked to code

### 2. **Theory-Code Connection**
- Theory document explains math
- Implementation follows theory exactly
- Visual proof of correctness

### 3. **Production Ready**
- Match sklearn performance
- Handle edge cases
- Optimized where critical
- Well-tested implementations

### 4. **Visual Learning**
- Plots for every major concept
- Step-by-step algorithm animation
- Interactive parameter exploration

---

## 🎯 Repository Goals

1. **Deep Understanding**: Not just "how" but "why"
2. **Practical Intuition**: When to use each algorithm
3. **Complete Coverage**: Clustering + dimensionality reduction
4. **Signal Quality**: Expert-level implementations
5. **Portfolio Piece**: Demonstrates ML expertise

---

## 📚 Resources & References

### Books
- "Pattern Recognition and Machine Learning" - Bishop
- "The Elements of Statistical Learning" - Hastie et al.
- "Introduction to Data Mining" - Tan et al.

### Papers
- K-Means: Lloyd (1982)
- DBSCAN: Ester et al. (1996)
- t-SNE: van der Maaten & Hinton (2008)
- UMAP: McInnes et al. (2018)

---

## 🤝 Contributing

This is a personal learning repository, but suggestions and corrections are welcome!

---

## 📄 License

MIT License - feel free to learn from and build upon this work.

---

## 👤 Author

**Sreekar Yerragunta**

Building expertise in machine learning through comprehensive from-scratch implementations.

- GitHub: [@sreekaryerragunta](https://github.com/sreekaryerragunta)
- Portfolio: [core-machine-learning](https://github.com/sreekaryerragunta/core-machine-learning) (supervised learning)
- Foundation: [ml-math-from-scratch](https://github.com/sreekaryerragunta/ml-math-from-scratch) (mathematical foundations)

---

**Note**: This repository focuses on **understanding** unsupervised learning through implementation. For production use, sklearn is recommended. For learning and interviews, implement from scratch!

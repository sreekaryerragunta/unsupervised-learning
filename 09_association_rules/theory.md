# Association Rule Mining - Theory

## What is Association Rule Mining?

**Association Rule Mining** - a technique for discovering interesting relationships (associations) between variables in large datasets, commonly used in market basket analysis.

**Core idea**: Find rules that describe how items are frequently purchased together, enabling recommendations and inventory management.

---

## Key Concepts

### Support
The proportion of transactions containing an itemset.

$$\text{Support}(X) = \frac{\text{Number of transactions containing } X}{\text{Total transactions}}$$

### Confidence
The likelihood of buying item Y given that item X was bought.

$$\text{Confidence}(X \rightarrow Y) = \frac{\text{Support}(X \cup Y)}{\text{Support}(X)}$$

### Lift
How much more likely Y is bought when X is bought, compared to Y being bought independently.

$$\text{Lift}(X \rightarrow Y) = \frac{\text{Support}(X \cup Y)}{\text{Support}(X) \times \text{Support}(Y)}$$

- **Lift > 1**: Positive correlation (X increases likelihood of Y)
- **Lift = 1**: No correlation
- **Lift < 1**: Negative correlation

---

## Apriori Algorithm

**Purpose**: Efficiently find frequent itemsets and generate association rules.

### Algorithm Steps:
1. **Find frequent 1-itemsets**: Items with support >= min_support
2. **Generate candidates**: Combine frequent k-itemsets to form (k+1)-itemsets
3. **Prune**: Remove candidates with infrequent subsets (Apriori principle)
4. **Count support**: Calculate support for remaining candidates
5. **Repeat**: Until no more frequent itemsets found
6. **Generate rules**: From frequent itemsets, create rules meeting min_confidence

### Apriori Principle
If an itemset is infrequent, all its supersets are also infrequent.

This allows aggressive pruning of the search space.

---

## FP-Growth Algorithm

**Purpose**: More efficient alternative to Apriori using a compressed data structure.

### Key Features:
- Builds an FP-tree (Frequent Pattern tree)
- No candidate generation required
- Compressed representation of transactions
- Two database scans instead of multiple passes

---

## Advantages

1. **Interpretable**: Rules are easy to understand
2. **No labels needed**: Unsupervised discovery
3. **Scalable**: Efficient with proper pruning
4. **Actionable**: Direct business applications

---

## Disadvantages

1. **Spurious correlations**: May find meaningless patterns
2. **Threshold sensitivity**: Results depend heavily on min_support/confidence
3. **Computational cost**: Large itemsets can be expensive
4. **Rare items**: Important but infrequent items may be missed

---

## Applications

- **Market Basket Analysis**: "Customers who bought X also bought Y"
- **Recommendation Systems**: Product suggestions
- **Medical Diagnosis**: Symptom-disease associations
- **Web Usage Mining**: Page navigation patterns
- **Fraud Detection**: Unusual transaction patterns

---

## Key Questions

**Q: What is the difference between Support and Confidence?**
A: Support measures how often an itemset appears in all transactions (frequency). Confidence measures how often Y appears in transactions containing X (conditional probability).

**Q: Why is Lift important?**
A: Lift accounts for the base popularity of items. High confidence rules can be misleading if Y is very popular anyway. Lift shows true correlation strength.

**Q: When should you use FP-Growth over Apriori?**
A: For large datasets with many items. FP-Growth is more memory-efficient and faster as it avoids candidate generation.

---

**Next**: See demonstration notebook for practical implementation using the mlxtend library.

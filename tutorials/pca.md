---
layout: default
title: PCA
parent: ML Basics
nav_order: 12
---

# Principal Component Analysis (PCA)

## Simple Explanation

Imagine a 3D sculpture. Taking a photo of it gives you a 2D image — you lose some depth information, but you still capture the most important shapes.

PCA does the same for data. If your dataset has 100 features, PCA compresses it down to 2 or 3 features that capture most of the important information. This makes data easier to visualize and speeds up other ML models.

---

## How It Works

1. Find the directions (components) in the data that have the most variation
2. Rank these directions from most to least important
3. Keep only the top K components — discarding the rest
4. Project all data points onto these K components

The result is a smaller dataset that retains most of the original information.

**Note:** Like K-Means, PCA is unsupervised — no labels required.

---

## When to Use It

**Good for:**
- Visualizing high-dimensional data in 2D or 3D
- Reducing data size before feeding it into another ML model
- Removing redundant or correlated features

**Not ideal for:**
- When interpretability matters (PCA components are combinations of original features — hard to explain)
- Non-linear structure in the data (use t-SNE or UMAP instead)

---

## Hands-On Code

Install:

```bash
pip install scikit-learn matplotlib
```

```python
import matplotlib.pyplot as plt
from sklearn.datasets import load_iris
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA

# Load Iris dataset (4 features)
data = load_iris()
X = data.data
y = data.target

# Scale features before PCA — important step
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Reduce from 4 features to 2 for easy visualization
pca = PCA(n_components=2)
X_reduced = pca.fit_transform(X_scaled)

# How much information did we keep?
variance_kept = pca.explained_variance_ratio_.sum()
print(f"Variance retained: {variance_kept * 100:.1f}%")

# Visualize the 2D result
plt.figure(figsize=(8, 5))
colors = ['red', 'green', 'blue']
for i, species in enumerate(data.target_names):
    mask = y == i
    plt.scatter(X_reduced[mask, 0], X_reduced[mask, 1],
                label=species, color=colors[i], alpha=0.7)
plt.xlabel('Principal Component 1')
plt.ylabel('Principal Component 2')
plt.title('Iris Dataset — 4 Features Compressed to 2')
plt.legend()
plt.show()
```

**Expected output:**
```
Variance retained: 95.8%
```

A 2D scatter plot showing three clearly separated flower species appears.

---

## Key Takeaways

- PCA compresses high-dimensional data while keeping most of the important information
- Always scale your features before applying PCA
- The "explained variance ratio" tells you how much information you kept
- Useful for visualization and as a preprocessing step for other models
- PCA components are mathematical combinations of original features — not directly interpretable

---

[← K-Means Clustering](kmeans){: .btn } [Next → Gradient Boosting](gradient-boosting){: .btn .btn-primary }

---
layout: default
title: K-Nearest Neighbors
parent: ML Basics
nav_order: 10
---

# K-Nearest Neighbors (KNN)

## Simple Explanation

You move to a new city and want to know if a neighborhood is good for families. You ask your 5 nearest neighbors. If 4 out of 5 say "yes, great for families," you conclude the neighborhood is family-friendly.

That's KNN. To classify a new data point, find the K nearest examples in your training data and take a vote.

---

## How It Works

1. Store all training examples (no actual "training" happens — just memorize the data)
2. For a new data point, calculate its distance to every training example
3. Find the K closest examples (the "neighbors")
4. Take a majority vote: whichever class appears most among the K neighbors wins

**K** is a number you choose. K=3 means "look at the 3 closest examples."

---

## When to Use It

**Good for:**
- Small to medium datasets
- When you want a simple, easy-to-understand model
- Problems where similar examples should have similar labels

**Not ideal for:**
- Very large datasets (slow — must compare to every training point)
- High-dimensional data (distances become meaningless in many dimensions)

---

## Hands-On Code

Install:

```bash
pip install scikit-learn
```

```python
from sklearn.datasets import load_iris
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Load the Iris dataset: 150 flowers, 3 species
data = load_iris()
X = data.data    # 4 measurements per flower
y = data.target  # Species label

# Split into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Create model with K=5 (look at the 5 nearest neighbors)
model = KNeighborsClassifier(n_neighbors=5)
model.fit(X_train, y_train)  # Just memorizes the training data

# Predict on the test set
predictions = model.predict(X_test)

# Check accuracy
accuracy = accuracy_score(y_test, predictions)
print(f"Accuracy: {accuracy * 100:.1f}%")

# Predict a single new flower
new_flower = [[6.0, 3.0, 4.8, 1.8]]
pred = model.predict(new_flower)
print(f"Predicted species: {data.target_names[pred[0]]}")
```

**Expected output:**
```
Accuracy: 100.0%
Predicted species: virginica
```

---

## Key Takeaways

- KNN classifies by finding the K most similar training examples and voting
- There is no real "training" — the model just stores the data
- Choosing K matters: too small = noisy predictions, too large = blurry boundaries
- Simple and intuitive, but slow on large datasets
- Works best when similar inputs really do have similar outputs

---

[← Logistic Regression](logistic-regression){: .btn } [Next → Naive Bayes](naive-bayes){: .btn .btn-primary }

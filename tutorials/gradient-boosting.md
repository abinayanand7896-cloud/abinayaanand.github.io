---
layout: default
title: Gradient Boosting
parent: ML Basics
nav_order: 7
---

# Gradient Boosting

## Simple Explanation

Imagine you're a student taking multiple practice tests. After each test, you study the questions you got wrong. Over time, you focus more and more on your weak spots. Each study session makes you a little better.

Gradient Boosting works the same way. It trains a sequence of small Decision Trees, where each new tree focuses on correcting the mistakes made by the previous ones. The final prediction is the sum of all these small trees working together.

---

## How It Works

1. Start with a simple prediction (e.g., the average of all values)
2. Calculate the errors (how far off the predictions are)
3. Train a small tree to predict those errors
4. Add this tree's predictions to the current predictions (reducing the errors)
5. Repeat steps 2–4 for many rounds

Each round reduces the remaining errors a little more. The "gradient" part refers to the mathematical technique used to measure and reduce these errors efficiently.

---

## When to Use It

**Good for:**
- Tabular data competitions (Gradient Boosting wins many Kaggle competitions)
- When you want the best possible accuracy on structured data
- Regression and classification tasks

**Not ideal for:**
- Images, audio, or text (use neural networks)
- When training speed is critical and data is very large (XGBoost is faster)

---

## Hands-On Code

Install:

```bash
pip install scikit-learn
```

```python
from sklearn.datasets import load_breast_cancer
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

# Load dataset
data = load_breast_cancer()
X = data.data
y = data.target

# Split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train Gradient Boosting with 100 rounds
model = GradientBoostingClassifier(n_estimators=100, learning_rate=0.1, max_depth=3, random_state=42)
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")

# Feature importance
importances = model.feature_importances_
top_features = sorted(zip(data.feature_names, importances), key=lambda x: x[1], reverse=True)[:5]
print("\nTop 5 most important features:")
for name, score in top_features:
    print(f"  {name}: {score:.3f}")
```

**Expected output:**
```
Accuracy: 96.5%

Top 5 most important features:
  worst concave points: 0.318
  worst perimeter: 0.141
  mean concave points: 0.098
  worst radius: 0.096
  worst area: 0.079
```

---

## Key Takeaways

- Gradient Boosting trains trees sequentially, each fixing the previous trees' mistakes
- It's one of the most powerful algorithms for tabular (spreadsheet) data
- Key parameters: `n_estimators` (number of trees), `learning_rate` (how much each tree contributes), `max_depth` (tree complexity)
- Slower to train than Random Forests but often more accurate
- XGBoost and LightGBM are faster, optimized versions of this same idea

---

[← PCA](pca){: .btn } [Next → XGBoost](xgboost){: .btn .btn-primary }

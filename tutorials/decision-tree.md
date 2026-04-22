---
layout: default
title: Decision Trees
parent: ML Foundations
nav_order: 6
---

# Decision Trees

## Plain English Intro

Think of the game 20 Questions. You ask: "Is it a mammal?" → Yes. "Is it bigger than a dog?" → No. "Does it live indoors?" → Yes. → Cat!

A Decision Tree works exactly like this. It learns a series of yes/no questions from your data and uses them to classify new examples. Each question splits the data further until a final answer is reached.

---

## How It Works

1. The model looks at all features and finds the question that best splits the data
2. It asks that question and splits into two branches: Yes / No
3. It repeats for each branch until every leaf contains mostly one class
4. To predict, start at the top, answer questions, follow branches, arrive at a leaf

---

## When to Use It

**Good for:**
- Problems where decisions can naturally be expressed as rules
- When you need to explain your model's reasoning to non-technical people
- Mixed data types (numbers and categories together)

**Not ideal for:**
- Large datasets where the tree grows too complex and overfits
- Smooth, continuous prediction tasks (use Linear Regression)

---

## Hands-On Code

Install:

```bash
pip install scikit-learn matplotlib
```

```python
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
import matplotlib.pyplot as plt

# Load Iris dataset
data = load_iris()
X = data.data
y = data.target

# Split into train/test
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Create model — max_depth=3 keeps the tree readable
model = DecisionTreeClassifier(max_depth=3, random_state=42)
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
print(f"Accuracy: {accuracy_score(y_test, predictions) * 100:.1f}%")

# Visualize the tree
plt.figure(figsize=(12, 6))
plot_tree(model, feature_names=data.feature_names,
          class_names=data.target_names, filled=True)
plt.title("Decision Tree for Iris Classification")
plt.show()
```

**Expected output:**
```
Accuracy: 100.0%
```

A diagram showing the decision tree with colored boxes will appear.

---

## Key Takeaways

- Decision Trees make predictions by asking a series of yes/no questions
- They are easy to visualize and explain to anyone
- They can overfit if grown too deep — use `max_depth` to limit this
- They are the building block for more powerful models like Random Forests
- Great for mixed data types and rule-based problems

---

[← Naive Bayes](naive-bayes){: .btn } [Next → Random Forests](random-forest){: .btn .btn-primary }

---
layout: default
title: Naive Bayes
parent: ML Basics
nav_order: 11
---

# Naive Bayes

## Simple Explanation

A doctor sees a patient with a fever and a cough. Based on past patients, they know: 80% of flu patients had fever, 70% had cough. Using these probabilities together, they estimate the chance this patient has the flu.

Naive Bayes works the same way. It calculates the probability of each class given the observed features, and picks the most likely class. It's "naive" because it assumes all features are independent of each other — which is rarely true, but works surprisingly well in practice.

---

## How It Works

1. During training, calculate how often each feature appears in each class
2. For a new example, multiply the probabilities of each feature given each class
3. Pick the class with the highest probability

---

## When to Use It

**Good for:**
- Text classification (spam filtering, sentiment analysis, news categorization)
- Real-time classification — it's extremely fast
- Small datasets where other models overfit

**Not ideal for:**
- When features are strongly correlated (the "naive" assumption breaks down)
- Continuous features with complex distributions (use Gaussian Naive Bayes as a workaround)

---

## Hands-On Code

Install:

```bash
pip install scikit-learn
```

```python
from sklearn.datasets import fetch_20newsgroups
from sklearn.naive_bayes import MultinomialNB
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics import accuracy_score

# Load two categories of news articles
categories = ['rec.sport.baseball', 'sci.space']
train_data = fetch_20newsgroups(subset='train', categories=categories)
test_data  = fetch_20newsgroups(subset='test',  categories=categories)

# Convert text to word counts (each word becomes a feature)
vectorizer = CountVectorizer()
X_train = vectorizer.fit_transform(train_data.data)  # Learn vocabulary + count words
X_test  = vectorizer.transform(test_data.data)        # Apply same vocabulary to test

# Train Naive Bayes classifier
model = MultinomialNB()
model.fit(X_train, train_data.target)

# Predict and check accuracy
predictions = model.predict(X_test)
accuracy = accuracy_score(test_data.target, predictions)
print(f"Accuracy: {accuracy * 100:.1f}%")

# Classify a new sentence
new_text = ["The rocket launched into orbit successfully"]
new_counts = vectorizer.transform(new_text)
pred = model.predict(new_counts)
print(f"Category: {train_data.target_names[pred[0]]}")
```

**Expected output:**
```
Accuracy: 97.3%
Category: sci.space
```

---

## Key Takeaways

- Naive Bayes uses probability to classify data
- It assumes features are independent — a simplification that works well in practice
- Extremely fast and works well with text data
- One of the best models for spam filtering and text classification
- Good choice when you have limited data and need quick results

---

[← K-Nearest Neighbors](knn){: .btn } [Next → Decision Trees](decision-tree){: .btn .btn-primary }

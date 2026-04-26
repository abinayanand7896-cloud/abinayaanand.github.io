---
layout: default
title: Convolutional Neural Networks
parent: Deep Learning
nav_order: 4
---

# Convolutional Neural Networks (CNN)

## Simple Explanation

When you look at a photo of a cat, your eyes don't process each pixel independently. They detect edges, then shapes, then features like ears and whiskers. CNNs work the same way — they scan images with small filters, detecting simple patterns first and combining them into complex features layer by layer.

This is why CNNs dramatically outperform MLPs on images. An MLP treats each pixel independently. A CNN understands that nearby pixels are related.

---

## How It Works

A CNN has two key types of layers:

**Convolutional Layer:**
- Slides a small filter (e.g., 3×3) across the image
- At each position, multiplies the filter values by the pixel values and sums the result
- Different filters detect different patterns (edges, curves, textures)

**Pooling Layer:**
- Shrinks the output by taking the maximum value in each small region
- Makes the network less sensitive to exact position of features

After several conv+pool layers, the output is flattened and passed to a regular MLP for the final classification.

---

## When to Use It

**Good for:**
- Image classification (cats vs. dogs, medical images, faces)
- Object detection and image segmentation
- Any task where spatial patterns in 2D data matter

**Not ideal for:**
- Sequences or time series (use RNN/LSTM)
- Tabular/spreadsheet data (use gradient boosting)

---

## Hands-On Code

Install:

```bash
pip install tensorflow
```

```python
import tensorflow as tf
import numpy as np

# Load MNIST: 70,000 handwritten digit images (0-9)
(X_train, y_train), (X_test, y_test) = tf.keras.datasets.mnist.load_data()

# Normalize pixel values from 0-255 to 0-1
X_train = X_train / 255.0
X_test  = X_test / 255.0

# Add channel dimension: (28, 28) → (28, 28, 1)
# CNNs expect images with a color channel (1=grayscale, 3=RGB)
X_train = X_train[..., np.newaxis]
X_test  = X_test[..., np.newaxis]

# Build CNN
model = tf.keras.Sequential([
    tf.keras.layers.Conv2D(32, (3, 3), activation='relu', input_shape=(28, 28, 1)),
    tf.keras.layers.MaxPooling2D((2, 2)),       # Shrink spatial dimensions by half
    tf.keras.layers.Conv2D(64, (3, 3), activation='relu'),
    tf.keras.layers.MaxPooling2D((2, 2)),
    tf.keras.layers.Flatten(),                  # Convert 2D feature maps to 1D
    tf.keras.layers.Dense(64, activation='relu'),
    tf.keras.layers.Dense(10, activation='softmax')  # 10 output classes (digits 0-9)
])

model.compile(optimizer='adam', loss='sparse_categorical_crossentropy', metrics=['accuracy'])

# Train for 5 epochs (passes through full dataset)
model.fit(X_train, y_train, epochs=5, batch_size=64, verbose=1)

# Evaluate
test_loss, test_accuracy = model.evaluate(X_test, y_test, verbose=0)
print(f"\nTest Accuracy: {test_accuracy * 100:.1f}%")
```

**Expected output:**
```
Epoch 1/5 - accuracy: 0.9758
Epoch 2/5 - accuracy: 0.9875
...
Test Accuracy: 99.1%
```

---

## Key Takeaways

- CNNs scan images with learned filters, detecting patterns from simple (edges) to complex (faces)
- Convolutional layers detect patterns; pooling layers reduce size while keeping key information
- Always normalize pixel values to 0–1 before training
- CNNs vastly outperform MLPs on image tasks because they understand spatial relationships
- The same architecture that classifies handwritten digits powers medical imaging and self-driving cars

---

[← MLP](mlp){: .btn } [Next → RNN](rnn){: .btn .btn-primary }

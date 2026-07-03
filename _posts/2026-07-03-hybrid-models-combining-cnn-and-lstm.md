---
layout: post
title: "Hybrid Models: Combining CNN and LSTM"
date: 2026-07-03
---



ICON: 🧬
# Hybrid Models
## Combining CNN and LSTM
Deep Dive Portfolio Project #4
What happens when you combine the "Eyes" of a CNN with the "Memory" of an LSTM? Magic.

***

ICON: 👁️
## Why use a CNN on Time Series Data?
CNNs aren't just for images! A 1D Convolutional Neural Network (`Conv1D`) acts as an incredibly powerful feature extractor.
Instead of feeding raw chemical readings (pH, Solids) directly to the memory, the CNN acts like an "eye" that scans the sequences and extracts hidden patterns (bites) first!

***

ICON: 🧠
## Passing to the Memory
Once the CNN extracts the dense, high-level features, we pass them to the LSTM.
The LSTM now has an easier job: instead of looking at noisy raw data, it looks at the clean "summary" the CNN provided over time.

***

ICON: 💻
## The Hybrid Architecture
```python
model = models.Sequential([
    # The Eye: Extracts chemical features
    layers.Conv1D(filters=64, kernel_size=2, activation='relu', input_shape=(5, 9)),
    layers.BatchNormalization(), # Speeds up learning
    
    # The Memory: Understands time
    layers.LSTM(64),
    layers.Dropout(0.3),
    
    # The Decision
    layers.Dense(1, activation='sigmoid')
])
```

***

ICON: 🏆
## The Ultimate Setup
This Hybrid Architecture is an industry standard for processing complex sequential data. 
By running this on the `water_potability.csv` dataset, we leverage the strengths of both architectures. 
Always experiment with Hybrid models when standard LSTMs hit a performance plateau!

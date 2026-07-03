---
layout: post
title: "Hybrid CNN-LSTM: The Best of Both Worlds"
date: 2026-07-03
---

# Hybrid Models:<br>CNN + LSTM

Combining Vision and Memory.

Portfolio Project Part 4: Water Potability.

## The Concept

CNNs are excellent at finding patterns and extracting features (like detecting an edge in an image).

LSTMs are excellent at remembering sequences over time.

If we use a 1D CNN to read our water quality features, it can filter out the noise and pass only the most critical signals to the LSTM!

## Building the Hybrid Architecture

First, we apply a `Conv1D` layer to extract spatial features from the 9 water metrics (pH, Hardness, etc).

Then, we pass those refined features into an `LSTM` layer to understand how they change over time.

```python
from tensorflow.keras.layers import Conv1D, LSTM, Dense, Dropout

model = Sequential([
    # The 'Eye': Extracts features from the 9 sensors
    Conv1D(filters=64, kernel_size=2, activation='relu', input_shape=(time_steps, 9)),
    
    # The 'Memory': Understands the timeline of those features
    LSTM(64),
    
    # Dropout prevents the model from memorizing the data (Overfitting)
    Dropout(0.3),
    
    Dense(1, activation='sigmoid')
])
```

## The Results

When run on our `water_potability.csv` dataset, the Hybrid model often outperforms standalone LSTMs.

The CNN acts as a perfect data-cleaner, allowing the LSTM to focus purely on the timeline of the anomalies.

This architecture is heavily used in stock market prediction and predictive maintenance!


---
layout: post
title: "Predicting Water Quality with RNNs"
date: 2026-07-03
---

# Predicting Water Quality<br>with RNNs

A deep dive into Sequence Models.

Portfolio Project: Analysing the Water Potability Dataset.

## Why RNNs for Water Data?

Standard Neural Networks treat every data point independently.

But what if we are tracking water quality **over time** (e.g., daily readings of pH, Sulfate, Chloramines)?

**Recurrent Neural Networks (RNNs)** have a 'memory' that remembers the previous time steps, allowing them to spot temporal anomalies in the water supply.

## Step 1: The Data

We use the **water_potability.csv** dataset.

It contains 9 critical features like `ph`, `Hardness`, and `Turbidity`.

First, we must normalize the data. RNNs are highly sensitive to unscaled inputs, which can cause the **Exploding Gradient Problem**.

```python
import pandas as pd
from sklearn.preprocessing import StandardScaler

df = pd.read_csv('DATASETS/water_potability.csv')
# Fill missing values
df.fillna(df.mean(), inplace=True)

# Scale the features
scaler = StandardScaler()
X_scaled = scaler.fit_transform(df.drop('Potability', axis=1))
y = df['Potability'].values
```

## Step 2: Building the RNN

In Keras, building an RNN is straightforward.

We use `SimpleRNN` layers. Since water data isn't incredibly complex, a single layer with 32 units is a good starting point.

```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import SimpleRNN, Dense

model = Sequential([
    # Reshape X_scaled to (samples, timesteps, features)
    SimpleRNN(32, input_shape=(time_steps, 9)),
    Dense(1, activation='sigmoid')
])

model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])
```

## The Limitation of SimpleRNN

While this model trains quickly, it suffers from **Short-Term Memory**.

If an anomaly in water quality depends on a reading from 30 days ago, the SimpleRNN will forget it.

To solve this, we need an upgrade. Check out the next project: **LSTM Networks**.


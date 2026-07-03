---
layout: post
title: "Understanding RNNs: Predicting Water Quality"
date: 2026-07-03
---



ICON: 💧
# Understanding RNNs
## Predicting Water Quality
Deep Dive Portfolio Project #1
We aren't using dummy data. Let's analyze the `water_potability.csv` dataset.

***

ICON: 📊
## The Real Dataset
Before building AI, you must understand your data. Our dataset has 3,276 records of water samples.
We track 9 chemicals: `pH`, `Hardness`, `Solids`, `Chloramines`, `Sulfate`, `Conductivity`, `Organic_carbon`, `Trihalomethanes`, and `Turbidity`.
Our goal: Predict if the water is safe to drink (`Potability = 1`).

***

ICON: 🧹
## Data Cleaning & Imputation
Real datasets are messy. Some sensors fail and leave missing values (`NaN`).
```python
import pandas as pd

# Load the real dataset
df = pd.read_csv('DATASETS/water_potability.csv')

# Impute missing values with the mean
df.fillna(df.mean(), inplace=True)
```
If we don't impute, the neural network will crash.

***

ICON: ⚖️
## Normalization is Critical
Look at the data: `pH` ranges from 0-14, but `Solids` can reach 40,000!
Neural networks are highly sensitive to scale. If we don't scale the data, the network will think `Solids` is 3,000x more important than `pH`.
```python
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

***

ICON: 🧠
## The RNN Architecture
Recurrent Neural Networks (RNNs) have a "memory" of the previous timestep. By grouping our water samples into sequences of 5, the RNN can learn how the chemical composition is changing over time.
```python
from tensorflow.keras import layers, models

model = models.Sequential([
    layers.SimpleRNN(32, input_shape=(5, 9)),
    layers.Dense(1, activation='sigmoid')
])
```
It's not just math; it's a dynamic system learning the flow of time!

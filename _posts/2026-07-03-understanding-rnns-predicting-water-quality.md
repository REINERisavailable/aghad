---
layout: post
title: "Predicting Water Quality with RNNs (PyTorch)"
date: 2026-07-03
---

# Predicting Water Quality<br>with RNNs

A deep dive into Sequence Models using PyTorch.

Portfolio Project: Data Storytelling with the Water Potability Dataset.

## The Business Problem

Access to safe drinking water is a critical global issue.

Our dataset contains 9 key metrics including pH, Hardness, and Sulfate levels.

**The Data Story:** We noticed that 39% of the water samples were unsafe. Standard models analyze each sample independently, but what if we need to monitor these sensors **over time** to predict a failure before it happens?

## Why RNNs?

Recurrent Neural Networks (RNNs) have an internal 'memory'.

They don't just look at today's pH level; they remember yesterday's level too. This allows them to spot dangerous trends in the water supply that a standard network would miss.

## Data Preparation & KPIs

![Media](../portfolio_site/assets/images/dashboards/water_dashboard.png)

Looking at our KPI Dashboard, we see a heavy imbalance. We must normalize the data using `StandardScaler` to ensure the neural network doesn't over-prioritize large numbers like 'Conductivity' over 'pH'.

```python
import pandas as pd
import torch
from torch.utils.data import DataLoader, TensorDataset
from sklearn.preprocessing import StandardScaler

df = pd.read_csv('DATASETS/water_potability.csv')
df.fillna(df.mean(), inplace=True)

# Scale and convert to PyTorch Tensors
scaler = StandardScaler()
X = torch.tensor(scaler.fit_transform(df.drop('Potability', axis=1)), dtype=torch.float32)
y = torch.tensor(df['Potability'].values, dtype=torch.float32).unsqueeze(1)

# Prepare for sequence modeling (Samples, Timesteps, Features)
X_seq = X.unsqueeze(1)
dataset = TensorDataset(X_seq, y)
loader = DataLoader(dataset, batch_size=32, shuffle=True)
```

## Building the RNN in PyTorch

In PyTorch, we define our architecture by subclassing `nn.Module`.

We use `nn.RNN` to process the sequence, and a linear layer to output the final potability prediction.

```python
import torch.nn as nn

class WaterRNN(nn.Module):
    def __init__(self):
        super().__init__()
        # input_size=9 features, hidden_size=32 memory cells
        self.rnn = nn.RNN(input_size=9, hidden_size=32, batch_first=True)
        self.fc = nn.Linear(32, 1)
        self.sigmoid = nn.Sigmoid()
        
    def forward(self, x):
        # out contains the sequence, h_n is the final memory state
        out, h_n = self.rnn(x)
        # Pass the final memory state to the decision layer
        return self.sigmoid(self.fc(h_n[-1]))

model = WaterRNN()
```

## The Limitation of SimpleRNN

While this PyTorch model trains quickly, it suffers from **Short-Term Memory** (The Vanishing Gradient).

If an anomaly in water quality depends on a reading from 30 days ago, the SimpleRNN will forget it.

To solve this, we need an upgrade. Check out the next project: **LSTM Networks**.


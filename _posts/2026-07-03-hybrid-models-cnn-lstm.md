---
layout: post
title: "Hybrid CNN-LSTM: The Best of Both Worlds (PyTorch)"
date: 2026-07-03
---

# Hybrid Models:<br>CNN + LSTM

Combining Vision and Memory in PyTorch.

Portfolio Project Part 4: Water Potability.

## The Concept: Filtering the Noise

![Media](../portfolio_site/assets/images/dashboards/water_dashboard.png)

Look at our pH distribution dashboard. There is a lot of overlap between safe and unsafe water. It's noisy.

CNNs are excellent at filtering noise and extracting spatial features.

LSTMs are excellent at remembering sequences over time.

If we use a 1D CNN to read our 9 water metrics first, it can filter out the noise and pass only the most critical signals to the LSTM!

## Building the Hybrid in PyTorch

In PyTorch, we stack `nn.Conv1d` before our `nn.LSTM`.

The CNN acts as an 'Eye' scanning the sensors, and the LSTM acts as the 'Brain' remembering the trends.

```python
import torch.nn as nn

class HybridWaterModel(nn.Module):
    def __init__(self):
        super().__init__()
        # The 'Eye': Extracts features from the 9 sensors
        # PyTorch Conv1d expects (batch, channels, length), so we use 9 channels
        self.cnn = nn.Conv1d(in_channels=9, out_channels=64, kernel_size=1)
        self.relu = nn.ReLU()
        
        # The 'Memory': Understands the timeline of those 64 features
        self.lstm = nn.LSTM(input_size=64, hidden_size=32, batch_first=True)
        self.dropout = nn.Dropout(0.3)
        self.fc = nn.Linear(32, 1)
        self.sigmoid = nn.Sigmoid()
        
    def forward(self, x):
        # x comes in as (batch, seq_len, features)
        # Conv1d needs (batch, features, seq_len)
        x = x.transpose(1, 2)
        x = self.relu(self.cnn(x))
        # Transpose back for LSTM: (batch, seq_len, features)
        x = x.transpose(1, 2)
        
        out, (h_n, c_n) = self.lstm(x)
        x = self.dropout(h_n[-1])
        return self.sigmoid(self.fc(x))
```

## The Results

When run on our `water_potability.csv` dataset, the Hybrid model outperformed standalone LSTMs.

The CNN acted as a perfect data-cleaner, allowing the LSTM to focus purely on the timeline of the anomalies.

This exact architecture is heavily used by Senior AI Engineers for predictive maintenance in industrial IoT!


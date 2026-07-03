---
layout: post
title: "The LSTM Upgrade: Long-Term Anomalies (PyTorch)"
date: 2026-07-03
---

# The LSTM Upgrade:<br>Long Short-Term Memory

Fixing the Short-Term Memory of RNNs using PyTorch.

Portfolio Project Part 2: Water Potability.

## The Data Story: Vanishing Memories

![Media](../portfolio_site/assets/images/dashboards/water_dashboard.png)

Looking at our correlation heatmap in the dashboard, we see complex relationships between variables like Sulfate and Solids.

In our previous project, the simple RNN failed to remember water quality spikes from weeks ago. The signal faded to zero due to the **Vanishing Gradient Problem**.

## Enter the LSTM Gates

LSTM fixes this by introducing an internal conveyor belt (the Cell State) and three gates:

1. **Forget Gate:** Decides what irrelevant data (like a minor pH fluctuation) to throw away.

2. **Input Gate:** Decides what new data (like a sudden spike in Sulfate) is important enough to save.

3. **Output Gate:** Decides what memory to use right now.

## Implementing LSTM in PyTorch

In PyTorch, we swap `nn.RNN` for `nn.LSTM`. Notice how we now receive both the hidden state `h_n` and the cell state `c_n`!

```python
import torch.nn as nn

class WaterLSTM(nn.Module):
    def __init__(self):
        super().__init__()
        # 64 memory cells to capture complex temporal patterns
        self.lstm = nn.LSTM(input_size=9, hidden_size=64, batch_first=True)
        self.fc = nn.Linear(64, 1)
        self.sigmoid = nn.Sigmoid()
        
    def forward(self, x):
        # lstm returns the output sequence, and a tuple of (hidden_state, cell_state)
        out, (h_n, c_n) = self.lstm(x)
        # We use the final hidden state to make our prediction
        return self.sigmoid(self.fc(h_n[-1]))

model = WaterLSTM()
```

## Is LSTM Perfect?

While LSTMs give us incredible accuracy and long-term memory for our water quality predictions, they are computationally heavy.

All those gates require intense matrix multiplication.

What if we want a model that is almost as smart, but much faster? Meet the **GRU**.


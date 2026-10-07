---
title: The gradient along the cell
---

::: card
Follow the gradient back along the cell state. One step back multiplies by

$$ \frac{\partial \mathbf{c}_t}{\partial \mathbf{c}_{t-1}} = \mathrm{diag}(\mathbf{f}_t) $$

a diagonal of forget gate values ([LSTM cell](reference:lstm-cell)). No weight matrix, and no
tanh slope.
:::

::: card
Over many steps the factors multiply:

$$ \frac{\partial \mathbf{c}_T}{\partial \mathbf{c}_1} = \prod_{t=2}^{T} \mathrm{diag}(\mathbf{f}_t) $$

If the forget gates stay near 1, this product stays near the identity, and the gradient reaches
step 1 almost intact. The RNN multiplied by $\mathbf{W}_h$ and a tanh slope at every step; the
LSTM multiplies by a gate value it can learn to hold open.
:::

::: card
Compare one entry of each. The RNN factor is an eigenvalue times a typical tanh slope; the LSTM
factor is the forget gate. With a gate of 0.99 the cell still passes $0.99^{100} \approx 0.37$ of
the gradient after 100 steps. An RNN factor of 0.9 passes about $2.7 \times 10^{-5}$.

```plot
x: { var: t, label: "steps back in time", from: 0, to: 100, ticks: 10, grid: true }
y: { label: "gradient that survives", from: 0.00001, to: 1.5, scale: log }

inputs:
  - { name: f, min: 0.8, max: 1, default: 0.99, step: 0.005, label: "forget gate f" }
  - { name: r, min: 0.5, max: 1, default: 0.9, step: 0.01, label: "RNN factor per step" }

draw:
  - curve: { is: r ^ t, dash: true, label: "RNN" }
  - curve: { is: f ^ t, accent: true, label: "LSTM cell" }
```
:::

::: card
The fix is not complete. If the forget gates sit below 1, the product still decays, only more
slowly. An LSTM learns to keep its gates open for the facts that matter, and so it remembers far
longer than a plain RNN.
:::

::: exercise q1
Every forget gate along 50 steps equals 0.98. What fraction of the gradient along the cell
survives?

::: answer
About 0.36. $0.98^{49} \approx 0.371$, or $0.98^{50} \approx 0.364$ if you count 50 factors.
:::
:::

::: exercise q2
Why does the cell path avoid the exploding gradient as well?

::: answer
Every factor is a forget gate value, a sigmoid output below 1. A product of numbers below 1
cannot grow.
:::
:::

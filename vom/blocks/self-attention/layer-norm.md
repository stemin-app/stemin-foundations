---
title: Layer normalization
---

::: card
Residuals fix the gradient, but the values themselves can still drift. If each layer adds a little
on average, values grow layer after layer; if it removes a little, they shrink. Huge values make
training unstable, and tiny ones drown the signal in noise. **Layer normalization** resets the
scale after each sublayer.
:::

::: card
It works on one token's vector $\mathbf{x} \in \mathbb{R}^d$ at a time. Compute the mean and the
standard deviation of its $d$ entries:

$$ \mu = \frac{1}{d}\sum_{i=1}^{d} x_i, \qquad \sigma = \sqrt{\frac{1}{d}\sum_{i=1}^{d}(x_i - \mu)^2} $$

Then subtract the mean and divide by the spread: $\hat{\mathbf{x}} = (\mathbf{x} - \mu)/\sqrt{\sigma^2 + \epsilon}$.
The small $\epsilon$, about $10^{-5}$, avoids a division by zero.
:::

::: card
Forcing mean 0 and variance 1 on every vector would limit the network. So two learned vectors,
the scale $\boldsymbol{\gamma}$ and the shift $\boldsymbol{\beta}$, act last:

$$ \mathrm{LayerNorm}(\mathbf{x}) = \boldsymbol{\gamma} \odot \hat{\mathbf{x}} + \boldsymbol{\beta} $$

They start at $\mathbf{1}$ and $\mathbf{0}$, and the network can learn any other scale and centre
([layer normalization](reference:layer-normalization)).
:::

::: card
Take $\mathbf{x} = [1.2, 0.6, -0.2, 0.1]$. The mean is $1.7 / 4 = 0.425$. The squared deviations
are $0.601, 0.031, 0.391, 0.106$, with mean $0.282$, so $\sigma \approx 0.531$. Normalize:

$$ \hat{\mathbf{x}} = \frac{[0.775, 0.175, -0.625, -0.325]}{0.531} \approx [1.46, 0.33, -1.18, -0.61] $$

These add to 0 and have variance 1.
:::

::: card
The four values of that vector are the points at $x = 1, 2, 3, 4$. Scale them by $c$ and shift them
by $s$, as a drifting layer would. The black points move. The blue ones, after layer
normalization, do not: the result ignores any common scale and shift.

```plot
x: { var: t, label: "entry", from: 0.5, to: 4.5, ticks: 1, grid: true }
y: { label: "value", from: -3, to: 4 }

inputs:
  - { name: c, min: 0.2, max: 3, default: 1, step: 0.1, label: "scale c" }
  - { name: s, min: -1, max: 2, default: 0, step: 0.1, label: "shift s" }

draw:
  - hline: { at: 0, dash: true }
  - point: { at: [1, 1.2 * c + s] }
  - point: { at: [2, 0.6 * c + s] }
  - point: { at: [3, -0.2 * c + s] }
  - point: { at: [4, 0.1 * c + s] }
  - point: { at: [1, 1.46], accent: true }
  - point: { at: [2, 0.33], accent: true }
  - point: { at: [3, -1.18], accent: true }
  - point: { at: [4, -0.61], accent: true }
```
:::

::: card
Layer normalization uses only the entries of one vector. Batch normalization, used in vision
networks, averages each feature across the examples of a batch. The layer version does not
depend on the batch size or on the other tokens, which suits sequences of any length.
:::

::: exercise q1
Normalize $\mathbf{x} = [2, 4, 6, 8]$, with $\epsilon$ ignored.

::: answer
About $[-1.34, -0.45, 0.45, 1.34]$. The mean is 5 and $\sigma = \sqrt{5} \approx 2.236$.
:::

::: solution
$\mu = 20 / 4 = 5$.

Deviations: $-3, -1, 1, 3$; squares: $9, 1, 1, 9$; mean $5$; $\sigma = \sqrt{5} \approx 2.236$.

$\hat{\mathbf{x}} = [-3, -1, 1, 3] / 2.236 \approx [-1.34, -0.45, 0.45, 1.34]$. ∎
:::
:::

::: exercise q2
Why does $\mathrm{LayerNorm}(10\,\mathbf{x})$ equal $\mathrm{LayerNorm}(\mathbf{x})$, with $\epsilon$
ignored?

::: answer
Scaling by 10 multiplies both $\mathbf{x} - \mu$ and $\sigma$ by 10, and the ratio cancels it.
:::
:::

::: reference layer-normalization
# Layer normalization

Layer normalization centres one vector on its own mean, divides it by its own standard deviation,
and then applies a learned scale and shift.

::: equation
\mathrm{LayerNorm}(\mathbf{x}) = \boldsymbol{\gamma} \odot \frac{\mathbf{x} - \mu}{\sqrt{\sigma^2 + \epsilon}} + \boldsymbol{\beta}, \qquad \mu = \frac{1}{d}\sum_i x_i, \quad \sigma^2 = \frac{1}{d}\sum_i (x_i - \mu)^2
:::

::: legend
$\mathbf{x}$: one token's vector, in $\mathbb{R}^d$
$\mu, \sigma^2$: the mean and the variance of its entries
$\epsilon$: a small constant, about $10^{-5}$
$\boldsymbol{\gamma}, \boldsymbol{\beta}$: the learned scale and shift, in $\mathbb{R}^d$
:::

::: derivation
Let $\hat{x}_i = (x_i - \mu)/\sigma$, with $\epsilon$ ignored.
Mean: $\frac{1}{d}\sum_i \hat{x}_i = \frac{1}{\sigma}\left(\frac{1}{d}\sum_i x_i - \mu\right) = 0$.
Variance: $\frac{1}{d}\sum_i \hat{x}_i^2 = \frac{1}{\sigma^2} \cdot \frac{1}{d}\sum_i (x_i - \mu)^2 = 1$.[variance](reference:variance)
For $c\mathbf{x} + s$ with $c > 0$: the mean becomes $c\mu + s$ and the deviation $c\sigma$, so $\hat{\mathbf{x}}$ is unchanged. ∎
:::
:::

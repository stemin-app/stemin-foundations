---
title: Gradients through many layers
---

::: card
Stack attention and FFN layers deep and training runs into trouble. The gradient that reaches the
first layer is a product of one factor per layer above it ([chain rule](reference:chain-rule)).
For a plain stack $\mathbf{y} = \mathbf{W}_L \cdots \mathbf{W}_2\mathbf{W}_1\mathbf{x}$, each step
back multiplies by one more matrix.
:::

::: card
If each factor is about 0.5, ten layers give $0.5^{10} \approx 0.001$ and twenty give
$0.5^{20} \approx 10^{-6}$. The first layer gets almost no signal and stops learning: the
gradient **vanishes**. If each factor is 1.5, ten layers give $1.5^{10} \approx 58$ and twenty give
$1.5^{20} \approx 3{,}300$: the gradient **explodes**, and the updates throw the weights away.
:::

::: card
Depth makes both worse, exponentially. A five-layer network may train fine and a fifty-layer one
not at all. Move the factor per layer and the depth.

```plot
x: { var: L, label: "number of layers", from: 0, to: 50, ticks: 5, grid: true }
y: { label: "gradient at the first layer", from: 0.000001, to: 1000000, scale: log }

inputs:
  - { name: a, min: 0.5, max: 1.5, default: 0.8, step: 0.05, label: "factor per layer" }

draw:
  - hline: { at: 1, dash: true }
  - curve: { is: a ^ L, accent: true }
```
:::

::: card
The fix, for a transformer, is a direct path that skips each sublayer. The next deck builds it:
the residual connection. Its gradient path multiplies by the identity, not by a weight matrix, so
depth no longer shrinks or grows the signal along it.
:::

::: exercise q1
Each of 12 layers multiplies the gradient by 0.7. What fraction reaches the first layer?

::: answer
About 0.014. $0.7^{12} \approx 0.0138$.
:::
:::

::: exercise q2
For which factor per layer does the gradient keep its size at any depth?

::: answer
Exactly 1. Any factor below 1 shrinks it and any factor above 1 grows it, exponentially in depth.
:::
:::

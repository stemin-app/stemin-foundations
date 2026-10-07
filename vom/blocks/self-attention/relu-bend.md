---
title: The ReLU bend
---

::: card
The most common nonlinearity is the **rectified linear unit**:

$$ \mathrm{ReLU}(x) = \max(0, x) $$

On a vector it acts entry by entry: $\mathrm{ReLU}([2, -1, 0, -3, 5]) = [2, 0, 0, 0, 5]$. A
positive entry passes unchanged, a negative one becomes zero ([ReLU](reference:relu)).
:::

::: card
Compare a line with a ReLU of the same line. Below, the dashed line is $f(x) = wx$, and the blue
curve is $g(x) = \mathrm{ReLU}(wx + b)$. For a positive weight, the curve follows a line on one
side of its bend and lies flat at zero on the other. Move $b$ and the bend slides.

```plot
x: { var: x, label: "x", from: -3, to: 3, ticks: 1, grid: true }
y: { label: "output", from: -4, to: 6 }

inputs:
  - { name: w, min: -2, max: 2, default: 2, step: 0.25, label: "weight w" }
  - { name: b, min: -3, max: 3, default: 0, step: 0.25, label: "bias b" }

draw:
  - hline: { at: 0 }
  - curve: { is: w * x, dash: true, label: "w x" }
  - curve: { is: "max(0, w * x + b)", accent: true, label: "ReLU" }
```
:::

::: card
The bend breaks linearity. With $g(x) = \mathrm{ReLU}(2x)$:

$$ g(-1) + g(1) = 0 + 2 = 2, \qquad g(-1 + 1) = g(0) = 0 $$

A linear function would give the same number both ways. The line $f(x) = 2x$ does: $-2 + 2 = 0 = f(0)$.
:::

::: card
The bend splits the input into two regions. Below it the unit is **off**: the output is zero,
however far down the input goes. Above it the unit is **on** and passes its input along. One
ReLU is one threshold, the simplest possible "if".
:::

::: card
ReLU is cheap and trains well. It needs only a comparison, no exponential. Its slope is 0 or 1,
so the backward pass is trivial. And it does not saturate for positive inputs, unlike the sigmoid
and tanh, so the gradient through an active unit never shrinks.
:::

::: exercise q1
Compute $\mathrm{ReLU}([0.5, -2, 3, -0.1])$.

::: answer
$[0.5, 0, 3, 0]$.
:::
:::

::: exercise q2
With $g(x) = \mathrm{ReLU}(x - 1)$, compare $g(2) + g(0)$ with $2\,g(1)$. What does this show?

::: answer
$1 + 0 = 1$, but $2\,g(1) = 0$. A linear function would make $g(2) + g(0) = 2\,g(1)$; the bend
breaks it.
:::
:::

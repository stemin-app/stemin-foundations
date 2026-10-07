---
title: Many bends make any shape
---

::: card
With two inputs, one unit computes $h_i = \mathrm{ReLU}(\mathbf{w}_i^T\mathbf{x} + b_i)$. The
equation $\mathbf{w}_i^T\mathbf{x} + b_i = 0$ is a line in the plane. On one side the unit is
off, on the other it is on. With $n$ units you get $n$ lines, and the plane falls into regions,
each with its own pattern of units on and off.
:::

::: card
Three units solve the light switches. Unit 1 fires when switch A is up, unit 2 when switch B is
up, unit 3 when both are up:

$$ h_1 = \mathrm{ReLU}(a - 0.5), \quad h_2 = \mathrm{ReLU}(b - 0.5), \quad h_3 = \mathrm{ReLU}(a + b - 1.5) $$

The output is $2h_1 + 2h_2 - 4h_3$, so that a fired unit counts 1.
:::

::: card
Check the four cases. $(0, 0)$: no unit fires, output 0. $(0, 1)$: $h_2 = 0.5$, output 1.
$(1, 0)$: $h_1 = 0.5$, output 1. $(1, 1)$: $h_1 = h_2 = h_3 = 0.5$, output $1 + 1 - 2 = 0$. The
light is on exactly when one switch is up: XOR, which no single line could do.
:::

::: card
A sum of ReLUs is **piecewise linear**: straight pieces joined at bends. One piece can do little.
Many pieces trace a curve. Below, $N$ straight pieces join points of the parabola $x^2$. Each
extra piece is one more ReLU. Raise $N$ and the pieces close in on the curve.

```plot
x: { var: x, label: "x", from: -1, to: 1, ticks: 0.25, grid: true }
y: { label: "y", from: -0.05, to: 1.05 }

inputs:
  - { name: n, min: 1, max: 16, default: 3, step: 1, label: "number of pieces N" }

let:
  h: 2 / n
  k: "min(floor((x + 1) / h), n - 1)"
  a: -1 + k * h

draw:
  - curve: { is: x ^ 2, dash: true, label: "x²" }
  - curve: { is: a ^ 2 + (2 * a + h) * (x - a), accent: true }
```
:::

::: card
A deep ReLU network does this in many dimensions. Its bends are hyperplanes, they cut the input
space into many regions, and each region gets its own linear map. From far away the result looks
smooth. With enough units, such a network can approximate any continuous function.
:::

::: exercise q1
In the XOR network, what does the output give for the input $(0.5, 0.5)$?

::: answer
0. No unit fires: $h_1 = h_2 = \mathrm{ReLU}(0) = 0$ and $h_3 = \mathrm{ReLU}(-0.5) = 0$.
:::
:::

::: exercise q2
A curve on a line is made of 10 straight pieces. How many bends does it have, and so how many
ReLU units at least does it need?

::: answer
9 bends, so at least 9 units. Each unit adds one bend.
:::
:::

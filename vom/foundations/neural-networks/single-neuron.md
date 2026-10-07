---
title: The single neuron
---

::: card
The smallest neural network is one neuron. It takes $n$ inputs $x_1, \ldots, x_n$, multiplies
each by its own weight, adds the products and a constant called the bias, and passes the sum
through a function $\sigma$.[Neuron](reference:neuron)

$$ y = \sigma\left(\sum_{i=1}^{n} w_i x_i + b\right) = \sigma\left(\mathbf{w}^T \mathbf{x} + b\right) $$

The weights form the vector $\mathbf{w} = [w_1, \ldots, w_n]^T$, $b$ is the bias, and $\sigma$
is the **activation function**.
:::

::: card
Look at one input first. The neuron computes $z = w x + b$ and then $y = \sigma(z)$. With the
sigmoid as $\sigma$, the output climbs from 0 to 1 and crosses 0.5 where $z = 0$, at
$x = -b/w$.

Raise $w$ and the step gets steeper. Change $b$ and the step slides along the axis. A negative
weight mirrors the curve, so that it falls instead of climbs.

```plot
x: { var: x, label: "$x$", from: -6, to: 6, ticks: 1, grid: true }
y: { label: "$y$", from: -0.1, to: 1.1 }

inputs:
  - { name: w, min: 0.25, max: 4, default: 1, step: 0.25, label: "the weight w" }
  - { name: b, min: -4, max: 4, default: 0, step: 0.25, label: "the bias b" }

draw:
  - hline: { at: 0.5, dash: true }
  - curve: { is: 1 / (1 + exp(-(w * x + b))), accent: true }
  - point: { at: [-b / w, 0.5], label: "$z = 0$" }
```
:::

::: card
The linear part $z = \mathbf{w}^T \mathbf{x} + b$ has a shape in input space. With two inputs,
the points where $z = 0$ satisfy $w_1 x_1 + w_2 x_2 + b = 0$, and that is a line. With $n$
inputs it is a hyperplane.

The weight vector $\mathbf{w}$ is perpendicular to the line, and the bias moves the line away
from the origin, to a distance $|b| / \|\mathbf{w}\|$.[The decision boundary](reference:neuron-decision-boundary)
:::

::: card
Take $\mathbf{w} = [2, 1]^T$ and $b = -3$. The linear part is $2x_1 + x_2 - 3$, and it is
positive exactly when $x_2 > -2x_1 + 3$, above the line $x_2 = -2x_1 + 3$.

The neuron splits the plane in two: one side where $z > 0$, one side where $z < 0$. Drag the
weights and watch $\mathbf{w}$ stay perpendicular to the line.

```plot
x: { var: s, label: "$x₁$", from: -4, to: 4, ticks: 1, grid: true }
y: { label: "$x₂$", from: -4, to: 4 }

inputs:
  - { name: wa, min: 0.5, max: 3, default: 2, step: 0.5, label: "the weight w1" }
  - { name: wb, min: -3, max: 3, default: 1, step: 0.5, label: "the weight w2" }
  - { name: b, min: -4, max: 4, default: -3, step: 0.5, label: "the bias b" }

let:
  n2: wa * wa + wb * wb
  n: sqrt(n2)
  px: -b * wa / n2
  py: -b * wb / n2

draw:
  - param: { var: u, over: [-12, 12], x: px - u * wb / n, y: py + u * wa / n }
  - param: { var: u, over: [0, 1], x: u * wa, y: u * wb, accent: true, label: "$w$" }
  - point: { at: [px + wa / n, py + wb / n], label: "$z > 0$" }
  - point: { at: [px - wa / n, py - wb / n], label: "$z < 0$" }
```
:::

::: card
Now run one neuron with numbers. The inputs are $\mathbf{x} = [0.5, 0.8]^T$, the weights are
$\mathbf{w} = [0.4, 0.6]^T$, the bias is $b = -0.5$, and the activation is the sigmoid. First the
linear part:

$$ z = 0.4 \cdot 0.5 + 0.6 \cdot 0.8 - 0.5 = 0.2 + 0.48 - 0.5 = 0.18 $$
:::

::: card
Then the activation:[Sigmoid](reference:sigmoid)

$$ y = \sigma(0.18) = \frac{1}{1 + e^{-0.18}} = \frac{1}{1 + 0.835} = \frac{1}{1.835} \approx 0.545 $$

The output sits a little above the midpoint 0.5, because the weighted sum 0.18 is a little
above zero.
:::

::: exercise neuron-output
A sigmoid neuron has weights $\mathbf{w} = [0.5, -1]^T$ and bias $b = 0$. What does it output for
the input $\mathbf{x} = [4, 1]^T$?

::: answer
$y = \sigma(1) \approx 0.731$. Compute $z$ first, then apply the sigmoid.
:::

::: solution
$$ z = 0.5 \cdot 4 + (-1) \cdot 1 + 0 = 2 - 1 = 1 $$

$$ y = \sigma(1) = \frac{1}{1 + e^{-1}} = \frac{1}{1 + 0.368} = \frac{1}{1.368} \approx 0.731 $$

∎
:::
:::

::: exercise neuron-side
A neuron has $\mathbf{w} = [2, 1]^T$ and $b = -3$. On which side of its line is the point
$(1, 2)$: the side where $z > 0$, or the side where $z < 0$?

::: answer
The side where $z > 0$: $z = 1$ there.
:::

::: solution
$$ z = 2 \cdot 1 + 1 \cdot 2 - 3 = 1 > 0 $$

∎
:::
:::

::: exercise neuron-line-distance
A neuron has $\mathbf{w} = [1, 1]^T$ and $b = -2$. How far is its line $z = 0$ from the origin?

::: answer
$\sqrt{2} \approx 1.414$. The distance is $|b| / \|\mathbf{w}\|$.
:::

::: solution
$$ \|\mathbf{w}\| = \sqrt{1^2 + 1^2} = \sqrt{2} $$

$$ d = \frac{|b|}{\|\mathbf{w}\|} = \frac{2}{\sqrt{2}} = \sqrt{2} \approx 1.414 $$

Check: the closest point is $-b\,\mathbf{w}/\|\mathbf{w}\|^2 = [1, 1]^T$, and $1 + 1 - 2 = 0$. ∎
:::
:::

::: reference neuron
# The neuron

A neuron takes a weighted sum of its inputs, adds a bias, and applies an activation function.

::: equation
y = \sigma\left(\sum_{i=1}^{n} w_i x_i + b\right) = \sigma\left(\mathbf{w}^T \mathbf{x} + b\right)
:::

::: legend
$y$: the output of the neuron, a scalar
$\mathbf{x}$: the input, a vector in $\mathbb{R}^n$
$\mathbf{w}$: the weights, a vector in $\mathbb{R}^n$
$b$: the bias, a scalar
$\sigma$: the activation function
:::
:::

::: reference neuron-decision-boundary
# The decision boundary of a neuron

The inputs where the linear part of a neuron is zero form a hyperplane. The weight vector is
perpendicular to it, and its distance from the origin is $|b| / \|\mathbf{w}\|$.

::: equation
\mathbf{w}^T \mathbf{x} + b = 0 \qquad d = \frac{|b|}{\|\mathbf{w}\|}
:::

::: legend
$\mathbf{x}$: a point of input space
$\mathbf{w}$: the weights of the neuron
$b$: the bias of the neuron
$d$: the distance from the origin to the hyperplane
:::

::: derivation
Take two points $\mathbf{x}_a$ and $\mathbf{x}_b$ on the hyperplane: $\mathbf{w}^T \mathbf{x}_a + b = 0$ and $\mathbf{w}^T \mathbf{x}_b + b = 0$.
Subtract: $\mathbf{w}^T (\mathbf{x}_a - \mathbf{x}_b) = 0$.
A zero dot product means a right angle, so $\mathbf{w}$ is perpendicular to every direction in the hyperplane.[Dot product](reference:dot-product)
The closest point to the origin lies along $\mathbf{w}$: $\mathbf{x}^* = t\,\mathbf{w}$.
Put it in the equation: $t\,\|\mathbf{w}\|^2 + b = 0$, so $t = -b / \|\mathbf{w}\|^2$.
Its length is $|t|\,\|\mathbf{w}\| = |b| / \|\mathbf{w}\|$.[Vector norm](reference:vector-norm) ∎
:::
:::

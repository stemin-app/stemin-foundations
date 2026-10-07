---
title: Mean squared error
---

::: card
A network has parameters $\boldsymbol{\theta}$: all its weights and biases. You have training
data, $m$ pairs $\{(\mathbf{x}^{(i)}, y^{(i)})\}_{i=1}^{m}$ of an input and the output you want.

Training looks for the parameters that bring the predictions close to the true outputs. A
**loss function** $\mathcal{L}(\boldsymbol{\theta})$ measures how far they are: one number, and
smaller is better.
:::

::: card
For **regression**, when the output is a continuous value, the usual choice is the
**mean squared error**:[Mean squared error](reference:mean-squared-error)

$$ \mathcal{L}_{\text{MSE}} = \frac{1}{m} \sum_{i=1}^{m} \left(f(\mathbf{x}^{(i)}; \boldsymbol{\theta}) - y^{(i)}\right)^2 $$

Here $f(\mathbf{x}; \boldsymbol{\theta})$ is the output of the network. The square penalises an
error quadratically: an error of 2 costs four times as much as an error of 1.
:::

::: card
Three examples have true values $1, 0, 1$, and the network predicts $0.8, 0.3, 0.9$:

$$ \mathcal{L}_{\text{MSE}} = \frac{1}{3}\left[(0.8 - 1)^2 + (0.3 - 0)^2 + (0.9 - 1)^2\right] = \frac{1}{3}\left[0.04 + 0.09 + 0.01\right] = \frac{0.14}{3} \approx 0.047 $$
:::

::: card
Now watch the loss react to one weight. Fit the line $\hat{y} = w x$ to the three points
$(1, 1)$, $(2, 3)$ and $(3, 2)$. Each dashed segment is one error $\hat{y}^{(i)} - y^{(i)}$. Drag
$w$ and watch the segments grow and shrink.

```plot
x: { var: x, label: "$x$", from: 0, to: 4, ticks: 1, grid: true }
y: { label: "$y$", from: 0, to: 5 }

inputs:
  - { name: w, min: 0, max: 2, default: 1.5, step: 0.05, label: "the weight w" }

draw:
  - curve: { is: w * x, accent: true }
  - param: { var: u, over: [0, 1], x: 1, y: 1 + u * (w - 1), dash: true }
  - param: { var: u, over: [0, 1], x: 2, y: 3 + u * (2 * w - 3), dash: true }
  - param: { var: u, over: [0, 1], x: 3, y: 2 + u * (3 * w - 2), dash: true }
  - points: [[1, 1], [2, 3], [3, 2]]
```
:::

::: card
As a function of $w$, the mean squared error of that fit is a parabola:

$$ \mathcal{L}(w) = \frac{1}{3}\left[(w - 1)^2 + (2w - 3)^2 + (3w - 2)^2\right] = \frac{1}{3}\left(14w^2 - 26w + 14\right) $$

Its lowest point is at $w = 13/14 \approx 0.929$, where the loss is $27/42 \approx 0.643$. The
loss never reaches zero, because no line through the origin passes through all three points.

```plot
x: { var: v, label: "$w$", from: 0, to: 2, ticks: 0.5, grid: true }
y: { label: "$ℒ$", from: 0, to: 4 }

inputs:
  - { name: w, min: 0, max: 2, default: 1.5, step: 0.05, label: "the weight w" }

draw:
  - curve: { is: (14 * v * v - 26 * v + 14) / 3, accent: true }
  - point: { at: [0.929, 0.643], label: "minimum" }
  - point: { at: [w, (14 * w * w - 26 * w + 14) / 3], label: "$w$" }
```
:::

::: card
For one example, you often write the squared error with a factor $\frac{1}{2}$:

$$ \mathcal{L} = \frac{1}{2}\left(y - \hat{y}\right)^2 \qquad \frac{\partial \mathcal{L}}{\partial \hat{y}} = \hat{y} - y $$

The $\frac{1}{2}$ cancels the 2 that the derivative brings down. It scales the loss, but it does
not move the minimum.
:::

::: exercise mse-two-points
A network predicts $2$ and $4$ where the true values are $1$ and $4$. What is the mean squared
error?

::: answer
0.5. Square each error, then take the mean.
:::

::: solution
$$ \mathcal{L}_{\text{MSE}} = \frac{1}{2}\left[(2 - 1)^2 + (4 - 4)^2\right] = \frac{1}{2}\left[1 + 0\right] = 0.5 $$

∎
:::
:::

::: exercise least-squares-two-points
You fit $\hat{y} = w x$ to the points $(1, 1)$ and $(2, 3)$ with the mean squared error. Which
$w$ gives the smallest loss?

::: answer
$w = 1.4$. Set $\frac{d\mathcal{L}}{dw} = 0$, or use $w^* = \sum x y / \sum x^2$.
:::

::: solution
$$ \mathcal{L}(w) = \frac{1}{2}\left[(w - 1)^2 + (2w - 3)^2\right] $$

$$ \frac{d\mathcal{L}}{dw} = (w - 1) + 2(2w - 3) = 5w - 7 $$

$$ 5w - 7 = 0 \implies w = 1.4 $$

Check: $\sum x y / \sum x^2 = (1 + 6)/(1 + 4) = 1.4$. ∎
:::
:::

::: exercise squared-penalty-ratio
Under the squared error, how many times more does an error of 3 cost than an error of 1?

::: answer
9 times: $3^2 / 1^2 = 9$.
:::
:::

::: reference mean-squared-error
# Mean squared error

The mean squared error averages the squared differences between the predictions and the true
values.

::: equation
\mathcal{L}_{\text{MSE}} = \frac{1}{m} \sum_{i=1}^{m} \left(f(\mathbf{x}^{(i)}; \boldsymbol{\theta}) - y^{(i)}\right)^2
:::

::: legend
$m$: the number of examples
$f(\mathbf{x}; \boldsymbol{\theta})$: the prediction of the network for the input $\mathbf{x}$
$\boldsymbol{\theta}$: the parameters of the network
$y^{(i)}$: the true value of example $i$
:::
:::

::: reference least-squares-slope
# The best slope through the origin

For the model $\hat{y} = w x$, the weight that minimises the squared error is the sum of the
products over the sum of the squared inputs.

::: equation
w^* = \frac{\sum_{i=1}^{m} x^{(i)} y^{(i)}}{\sum_{i=1}^{m} \left(x^{(i)}\right)^2}
:::

::: legend
$w^*$: the weight with the lowest loss
$x^{(i)}, y^{(i)}$: the input and the true value of example $i$
:::

::: derivation
$\mathcal{L}(w) = \frac{1}{m} \sum_i \left(w x^{(i)} - y^{(i)}\right)^2$.[Mean squared error](reference:mean-squared-error)
$\frac{d\mathcal{L}}{dw} = \frac{2}{m} \sum_i x^{(i)}\left(w x^{(i)} - y^{(i)}\right)$.[Chain rule](reference:chain-rule)
Set it to zero: $w \sum_i \left(x^{(i)}\right)^2 = \sum_i x^{(i)} y^{(i)}$.
Divide: $w^* = \sum_i x^{(i)} y^{(i)} / \sum_i \left(x^{(i)}\right)^2$.
$\mathcal{L}$ is a parabola that opens upward, so this stationary point is its minimum. ∎
:::
:::

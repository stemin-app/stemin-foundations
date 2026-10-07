---
title: The loss for classification
---

::: card
For **classification**, the network outputs a probability. In the binary case the true label is
$y \in \{0, 1\}$, and a sigmoid output gives $\hat{y} = \sigma(z) \in (0, 1)$, the probability
of class 1. The loss is the **binary cross-entropy**:

$$ \mathcal{L}_{\text{CE}} = -\frac{1}{m} \sum_{i=1}^{m} \left[ y^{(i)} \log \hat{y}^{(i)} + \left(1 - y^{(i)}\right) \log\left(1 - \hat{y}^{(i)}\right) \right] $$

It is the [cross-entropy](reference:cross-entropy) between the true label and the prediction.
:::

::: card
Only one of the two terms is alive for each example. When $y = 1$, the loss is
$-\log \hat{y}$; when $y = 0$, it is $-\log(1 - \hat{y})$. Either way it is the surprise of the
true class. Move the prediction and compare the two curves: a confident wrong answer is costly
on both sides.

```plot
x: { var: p, label: "the prediction ŷ", from: 0, to: 1, ticks: 0.1, grid: true }
y: { label: "loss", from: 0, to: 4.5 }

inputs:
  - { name: p0, min: 0.02, max: 0.98, default: 0.8, step: 0.02, label: "the prediction" }

draw:
  - curve: { is: "-log(e(), p)", over: [0.01, 1], accent: true, label: "y = 1" }
  - curve: { is: "-log(e(), 1 - p)", over: [0, 0.99], dash: true, label: "y = 0" }
  - point: { at: [p0, "-log(e(), p0)"] }
  - point: { at: [p0, "-log(e(), 1 - p0)"] }
```
:::

::: card
Three examples have labels $1, 0, 1$ and predictions $0.9, 0.2, 0.8$. The second has $y = 0$,
so it counts $\log(1 - 0.2) = \log 0.8$:

$$ \mathcal{L}_{\text{CE}} = -\frac{1}{3}\left[\log 0.9 + \log 0.8 + \log 0.8\right] = \frac{0.105 + 0.223 + 0.223}{3} \approx 0.184 $$
:::

::: card
With $K$ classes, the output is $\hat{\mathbf{y}} = \mathrm{softmax}(\mathbf{z})$, and the label is
a **one-hot** vector: $y_k^{(i)} = 1$ for the true class of example $i$ and 0 elsewhere.

$$ \mathcal{L}_{\text{CE}} = -\frac{1}{m} \sum_{i=1}^{m} \sum_{k=1}^{K} y_k^{(i)} \log \hat{y}_k^{(i)} $$

The inner sum keeps one term: the log probability of the true class.
:::

::: card
Softmax and cross-entropy fit together. Their combined gradient with respect to the logits is
the prediction minus the target:

$$ \frac{\partial \mathcal{L}}{\partial \mathbf{z}} = \hat{\mathbf{y}} - \mathbf{y} $$

The softmax Jacobian and the $1/\hat{y}$ of the logarithm cancel, and only a difference remains
([gradient of softmax with cross-entropy](reference:softmax-cross-entropy-gradient)). The same
holds for the sigmoid with binary cross-entropy: $\partial \mathcal{L} / \partial z = \hat{y} - y$.
:::

::: exercise q1
The label is $y = 0$ and the prediction is $\hat{y} = 0.9$. What is the binary cross-entropy?

::: answer
$-\log 0.1 \approx 2.303$. With $y = 0$, only $-\log(1 - \hat{y})$ counts.
:::
:::

::: exercise q2
A softmax over three classes predicts $[0.7, 0.2, 0.1]$, and the true class is the first. What
are the loss and the gradient with respect to the logits?

::: answer
The loss is $-\log 0.7 \approx 0.357$. The gradient is $[-0.3, 0.2, 0.1]$.
:::

::: solution
$\mathcal{L} = -\log 0.7 \approx 0.357$.

$\hat{\mathbf{y}} - \mathbf{y} = [0.7 - 1, \; 0.2 - 0, \; 0.1 - 0] = [-0.3, 0.2, 0.1]$. ∎
:::
:::

::: exercise q3
A sigmoid output has $z = 0$ and the label is $y = 1$. What is $\frac{\partial \mathcal{L}}{\partial z}$
for the binary cross-entropy?

::: answer
$-0.5$. $\hat{y} = \sigma(0) = 0.5$, and the gradient is $\hat{y} - y$.
:::
:::

::: reference softmax-cross-entropy-gradient
# Gradient of softmax with cross-entropy

When a softmax produces the prediction and the loss is the cross-entropy against a one-hot
target, the gradient with respect to the logits is the prediction minus the target.

::: equation
\mathcal{L} = -\sum_{k=1}^{K} y_k \log \hat{y}_k, \quad \hat{\mathbf{y}} = \mathrm{softmax}(\mathbf{z}) \qquad \frac{\partial \mathcal{L}}{\partial z_j} = \hat{y}_j - y_j
:::

::: legend
$\mathbf{z}$: the logits, $K$ of them
$\hat{\mathbf{y}}$: the predicted probabilities
$\mathbf{y}$: the one-hot target, with $\sum_k y_k = 1$
:::

::: derivation
$\frac{\partial \mathcal{L}}{\partial \hat{y}_k} = -\frac{y_k}{\hat{y}_k}$.
$\frac{\partial \hat{y}_k}{\partial z_j} = \hat{y}_k(\delta_{kj} - \hat{y}_j)$, with $\delta_{kj} = 1$ when $k = j$ and 0 otherwise.[softmax Jacobian](reference:softmax-jacobian)
Sum over $k$: $\frac{\partial \mathcal{L}}{\partial z_j} = -\sum_k \frac{y_k}{\hat{y}_k}\hat{y}_k(\delta_{kj} - \hat{y}_j) = -\sum_k y_k \delta_{kj} + \hat{y}_j \sum_k y_k$.[multivariable chain rule](reference:multivariable-chain-rule)
The first sum is $y_j$, and $\sum_k y_k = 1$.
So $\frac{\partial \mathcal{L}}{\partial z_j} = \hat{y}_j - y_j$. ∎
:::
:::

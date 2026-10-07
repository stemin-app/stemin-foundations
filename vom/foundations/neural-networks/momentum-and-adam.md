---
title: Momentum and Adam
---

::: card
Plain gradient descent is slow in a **ravine**: a valley steep across and gentle along. The
gradient points mostly across, so the steps bounce from wall to wall and creep along the floor.
**Momentum** fixes this with a velocity that remembers past steps:

$$ \mathbf{v} \leftarrow \beta\mathbf{v} - \eta \nabla_{\boldsymbol{\theta}} \mathcal{L}, \qquad \boldsymbol{\theta} \leftarrow \boldsymbol{\theta} + \mathbf{v} $$

The coefficient $\beta$ is usually about 0.9.
:::

::: card
The velocity is a running sum of past gradients, each older one shrunk by another factor $\beta$.
Across the ravine the gradient flips sign every step, and the terms cancel. Along the floor it
keeps its sign, and the terms pile up. Under a steady gradient $g$ the speed grows toward
$\eta g / (1 - \beta)$: ten times the plain step when $\beta = 0.9$.

```plot
x: { var: t, label: "step t", from: 0, to: 60, ticks: 10, grid: true }
y: { label: "speed, in plain steps", from: 0, to: 21 }

inputs:
  - { name: b, min: 0, max: 0.95, default: 0.9, step: 0.05, label: "momentum β" }

draw:
  - hline: { at: 1, dash: true, label: "plain" }
  - hline: { at: 1 / (1 - b), dash: true }
  - curve: { is: (1 - b ^ t) / (1 - b), accent: true, label: "momentum" }
```
:::

::: card
**Adam**, for adaptive moment estimation, also gives each parameter its own step size. It keeps
two running averages, of the gradient $\mathbf{g}$ and of its square, entry by entry:

$$ \mathbf{m} \leftarrow \beta_1\mathbf{m} + (1 - \beta_1)\,\mathbf{g}, \qquad \mathbf{v} \leftarrow \beta_2\mathbf{v} + (1 - \beta_2)\,\mathbf{g}^2 $$

Typical values are $\beta_1 = 0.9$ and $\beta_2 = 0.999$.
:::

::: card
Both averages start at zero, so they start too small. After $t$ steps of a steady gradient $g$,
$m_t = (1 - \beta_1^t)\,g$. Dividing by $1 - \beta_1^t$ undoes the shrink exactly. This is the
**bias correction**: $\hat{\mathbf{m}} = \mathbf{m}/(1 - \beta_1^t)$, and
$\hat{\mathbf{v}} = \mathbf{v}/(1 - \beta_2^t)$.

```plot
x: { var: t, label: "step t", from: 0, to: 40, ticks: 5, grid: true }
y: { label: "estimate of g", from: 0, to: 1.2 }

inputs:
  - { name: b, min: 0.5, max: 0.99, default: 0.9, step: 0.01, label: "β₁" }

draw:
  - hline: { at: 1, accent: true, label: "corrected" }
  - curve: { is: 1 - b ^ t, over: [1, 40], label: "raw m" }
```
:::

::: card
The update divides the average gradient by the root of the average square:

$$ \boldsymbol{\theta} \leftarrow \boldsymbol{\theta} - \eta\,\frac{\hat{\mathbf{m}}}{\sqrt{\hat{\mathbf{v}}} + \epsilon} $$

A parameter with large gradients gets a smaller step; one with small gradients a larger one. The
$\epsilon \approx 10^{-8}$ avoids a division by zero. Adam is the default optimizer for
transformers ([Adam](reference:adam)).
:::

::: card
The ratio is roughly the sign of a steady gradient. If $g$ is the same every step, then
$\hat{m} = g$ and $\sqrt{\hat{v}} = |g|$, so each step moves by about $\eta$ whatever the size of
$g$. A gradient of 0.001 and a gradient of 1000 give the same step, so one learning rate suits
every parameter.
:::

::: card
Put together, one round of training is: sample a mini-batch; run the **forward pass** and compute
the loss; run the **backward pass** to get every gradient; **update** every parameter with the
optimizer. Repeat until the loss stops falling, and check the result on data the network never
trained on. Every network trains this way, from a small MLP to the largest transformer.
:::

::: exercise q1
With $\beta = 0.9$, $\eta = 0.1$, $\mathbf{v} = 0$ and a steady gradient $g = 1$, what is the
velocity after two steps?

::: answer
$-0.19$. First $v = -0.1$, then $v = 0.9(-0.1) - 0.1$.
:::
:::

::: exercise q2
With $\beta_1 = 0.9$ and $m_0 = 0$, the gradient is 2 at step 1. What are $m_1$ and $\hat{m}_1$?

::: answer
$m_1 = 0.2$ and $\hat{m}_1 = 2$. Divide by $1 - 0.9 = 0.1$.
:::
:::

::: exercise q3
Under a steady gradient, what speed does momentum approach with $\beta = 0.99$, in units of the
plain step $\eta g$?

::: answer
100. The limit is $1 / (1 - \beta)$.
:::
:::

::: reference adam
# Adam

Adam keeps running averages of the gradient and of its square, corrects their bias toward zero,
and steps each parameter by the ratio of the two.

::: equation
\begin{aligned}
\mathbf{m}_t &= \beta_1\mathbf{m}_{t-1} + (1 - \beta_1)\,\mathbf{g}_t, & \hat{\mathbf{m}}_t &= \frac{\mathbf{m}_t}{1 - \beta_1^t} \\
\mathbf{v}_t &= \beta_2\mathbf{v}_{t-1} + (1 - \beta_2)\,\mathbf{g}_t^2, & \hat{\mathbf{v}}_t &= \frac{\mathbf{v}_t}{1 - \beta_2^t} \\
\boldsymbol{\theta}_t &= \boldsymbol{\theta}_{t-1} - \eta\,\frac{\hat{\mathbf{m}}_t}{\sqrt{\hat{\mathbf{v}}_t} + \epsilon}
\end{aligned}
:::

::: legend
$\mathbf{g}_t$: the gradient at step $t$
$\mathbf{m}_t, \mathbf{v}_t$: the running averages of the gradient and of its square, both starting at zero
$\beta_1, \beta_2$: the decay rates, about 0.9 and 0.999
$\eta$: the learning rate
$\epsilon$: a small constant, about $10^{-8}$
:::

::: derivation
With $\mathbf{m}_0 = \mathbf{0}$, unroll the average: $\mathbf{m}_t = (1 - \beta_1)\sum_{k=1}^{t} \beta_1^{t-k}\mathbf{g}_k$.
For a steady gradient $\mathbf{g}$ the sum is geometric: $\mathbf{m}_t = (1 - \beta_1)\frac{1 - \beta_1^t}{1 - \beta_1}\mathbf{g} = (1 - \beta_1^t)\,\mathbf{g}$.
So $\hat{\mathbf{m}}_t = \mathbf{m}_t / (1 - \beta_1^t) = \mathbf{g}$, the true value.
The same steps give $\hat{\mathbf{v}}_t = \mathbf{g}^2$. ∎
:::
:::

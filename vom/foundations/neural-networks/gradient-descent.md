---
title: Gradient descent
---

::: card
With the gradient in hand, move every parameter a small step against it:

$$ \boldsymbol{\theta} \leftarrow \boldsymbol{\theta} - \eta \nabla_{\boldsymbol{\theta}} \mathcal{L} $$

The gradient points uphill, so its opposite points downhill. The **learning rate** $\eta > 0$
sets the size of the step ([gradient descent](reference:gradient-descent)).
:::

::: card
The learning rate decides whether training works. Take $f(\theta) = \theta^2$, with
$\frac{df}{d\theta} = 2\theta$, and start at $\theta_0 = 1$. One step gives
$\theta_1 = \theta_0 - 2\eta\theta_0 = (1 - 2\eta)\,\theta_0$, so after $k$ steps

$$ \theta_k = (1 - 2\eta)^k $$
:::

::: card
Each point below is one step. At $\eta = 0.1$ the iterates creep down: 0.8, 0.64, and so on. At
$\eta = 0.5$ they reach 0 in one step. At $\eta = 0.9$ they jump across the minimum and back,
$-0.8$, $0.64$, but still settle. Above $\eta = 1$ every jump is longer than the last, and the
iterates run away.

```plot
x: { var: k, label: "step k", from: 0, to: 8, ticks: 1, grid: true }
y: { label: "θₖ", from: -2, to: 2 }

inputs:
  - { name: eta, min: 0.05, max: 1.15, default: 0.1, step: 0.05, label: "learning rate η" }

let:
  r: 1 - 2 * eta

draw:
  - hline: { at: 0, dash: true }
  - point: { at: [0, 1] }
  - point: { at: [1, r], accent: true }
  - point: { at: [2, r ^ 2], accent: true }
  - point: { at: [3, r ^ 3], accent: true }
  - point: { at: [4, r ^ 4], accent: true }
  - point: { at: [5, r ^ 5], accent: true }
  - point: { at: [6, r ^ 6], accent: true }
  - point: { at: [7, r ^ 7], accent: true }
  - point: { at: [8, r ^ 8], accent: true }
```
:::

::: card
The same steps, drawn on the curve $f(\theta) = \theta^2$. A small rate slides down one side. A
rate near 1 zigzags across the valley. Above 1 the zigzag widens with every step.

```plot
x: { var: t, label: "θ", from: -2.5, to: 2.5, ticks: 0.5, grid: true }
y: { label: "f(θ)", from: 0, to: 4 }

inputs:
  - { name: eta, min: 0.05, max: 1.15, default: 0.9, step: 0.05, label: "learning rate η" }

let:
  r: 1 - 2 * eta

draw:
  - curve: t ^ 2
  - param: { var: s, over: [0, 1], x: 1 + s * (r - 1), y: 1 + s * (r ^ 2 - 1), accent: true }
  - param: { var: s, over: [0, 1], x: r + s * (r ^ 2 - r), y: r ^ 2 + s * (r ^ 4 - r ^ 2), accent: true }
  - param: { var: s, over: [0, 1], x: r ^ 2 + s * (r ^ 3 - r ^ 2), y: r ^ 4 + s * (r ^ 6 - r ^ 4), accent: true }
  - param: { var: s, over: [0, 1], x: r ^ 3 + s * (r ^ 4 - r ^ 3), y: r ^ 6 + s * (r ^ 8 - r ^ 6), accent: true }
  - param: { var: s, over: [0, 1], x: r ^ 4 + s * (r ^ 5 - r ^ 4), y: r ^ 8 + s * (r ^ 10 - r ^ 8), accent: true }
  - point: { at: [1, 1], label: "start" }
```
:::

::: card
For this parabola, any $\eta < 1$ converges, $\eta = 0.5$ is best, and $\eta > 1$ diverges. Check
$\eta = 1.1$: $\theta_1 = 1 - 2.2 = -1.2$, then $\theta_2 = -1.2 + 2.64 = 1.44$. A real loss is not
a parabola, but the lesson holds: too small is slow, too large blows up, and good rates lie in
between.
:::

::: card
The full gradient averages over all $m$ training examples, which is expensive.
**Stochastic gradient descent** uses one random example $i$ per step:
$\boldsymbol{\theta} \leftarrow \boldsymbol{\theta} - \eta \nabla_{\boldsymbol{\theta}} \mathcal{L}^{(i)}$.
Each step is cheap and noisy. **Mini-batch** descent averages over a batch of $B$ examples:

$$ \boldsymbol{\theta} \leftarrow \boldsymbol{\theta} - \eta \cdot \frac{1}{B} \sum_{i \in \text{batch}} \nabla_{\boldsymbol{\theta}} \mathcal{L}^{(i)} $$

Typical batches hold 32, 64, 128 or 256 examples.
:::

::: card
Why a batch helps: the average of $B$ independent gradients has its noise, the standard deviation,
divided by $\sqrt{B}$. A batch of 64 cuts the noise of one example by 8. Doubling the batch does
not halve the noise, though: 128 examples cut it by only about 11.

```plot
x: { var: B, label: "batch size B", from: 1, to: 512, scale: log, grid: true }
y: { label: "relative noise", from: 0, to: 1.05, ticks: 0.25 }

draw:
  - curve: { is: 1 / sqrt(B), accent: true }
  - point: { at: [64, 0.125], label: "B = 64" }
```
:::

::: exercise q1
$f(\theta) = \theta^2$, $\theta_0 = 2$ and $\eta = 0.25$. What are $\theta_1$ and $\theta_2$?

::: answer
$\theta_1 = 1$ and $\theta_2 = 0.5$. Each step multiplies $\theta$ by $1 - 2\eta = 0.5$.
:::
:::

::: exercise q2
For $f(\theta) = \theta^2$, which learning rate reaches the minimum in one step from any start?

::: answer
$\eta = 0.5$. Then $1 - 2\eta = 0$.
:::
:::

::: exercise q3
For $f(\theta) = 3\theta^2$, above which learning rate does gradient descent diverge?

::: answer
$\eta > 1/3$. Each step multiplies $\theta$ by $1 - 6\eta$, which must stay between $-1$ and 1.
:::

::: solution
$\frac{df}{d\theta} = 6\theta$, so $\theta_{k+1} = \theta_k - 6\eta\theta_k = (1 - 6\eta)\theta_k$.

Divergence needs $|1 - 6\eta| > 1$. With $\eta > 0$ this means $1 - 6\eta < -1$.

So $\eta > 1/3$. ∎
:::
:::

::: exercise q4
The gradient noise of a single example has standard deviation 0.8. What is it for a batch of
16 independent examples?

::: answer
0.2. Divide by $\sqrt{16} = 4$.
:::
:::

::: reference gradient-descent
# Gradient descent

Gradient descent moves the parameters a small step against the gradient of the loss. The
learning rate sets the step. For $f(\theta) = a\theta^2$ it converges when $0 < \eta < 1/a$.

::: equation
\boldsymbol{\theta} \leftarrow \boldsymbol{\theta} - \eta \nabla_{\boldsymbol{\theta}} \mathcal{L}
:::

::: legend
$\boldsymbol{\theta}$: all the parameters
$\eta$: the learning rate, positive
$\nabla_{\boldsymbol{\theta}} \mathcal{L}$: the gradient of the loss
:::

::: derivation
The slope of $\mathcal{L}$ along a unit direction $\mathbf{u}$ is $\nabla\mathcal{L} \cdot \mathbf{u} = \|\nabla\mathcal{L}\|\cos\theta$.[directional derivative](reference:directional-derivative)
It is most negative at $\theta = \pi$, when $\mathbf{u}$ points against the gradient.
For a small step $\eta$: $\mathcal{L}(\boldsymbol{\theta} - \eta\nabla\mathcal{L}) \approx \mathcal{L}(\boldsymbol{\theta}) - \eta\|\nabla\mathcal{L}\|^2$, which is lower.
For $f = a\theta^2$: $\theta_{k+1} = (1 - 2a\eta)\theta_k$, which shrinks exactly when $|1 - 2a\eta| < 1$, that is $0 < \eta < 1/a$. ∎
:::
:::

---
title: Vanishing and exploding gradients
---

::: card
Suppose every tanh slope is close to 1. Then the product of Jacobians from step $T$ back to step 1
is a power of one matrix:

$$ \frac{\partial \mathbf{h}_T}{\partial \mathbf{h}_1} \approx \mathbf{W}_h^{T-1} $$

Write $\mathbf{W}_h = \mathbf{V}\boldsymbol{\Lambda}\mathbf{V}^{-1}$, with the eigenvalues
$\lambda_1, \ldots, \lambda_d$ on the diagonal of $\boldsymbol{\Lambda}$. The power acts on the
eigenvalues alone: $\mathbf{W}_h^{T-1} = \mathbf{V}\boldsymbol{\Lambda}^{T-1}\mathbf{V}^{-1}$
([eigenvalue equation](reference:eigenvalue-equation)).
:::

::: card
Each eigenvalue is raised to the power $T - 1$. If $|\lambda| < 1$, $\lambda^{T-1}$ falls to zero
exponentially: the gradient **vanishes**. If $|\lambda| > 1$, it grows exponentially: the
gradient **explodes**. Only $|\lambda| = 1$ stays put. On a log scale each power is a straight
line, tilting down or up.

```plot
x: { var: t, label: "steps back in time", from: 0, to: 100, ticks: 10, grid: true }
y: { label: "size of the factor", from: 0.00001, to: 100000, scale: log }

inputs:
  - { name: lam, min: 0.5, max: 1.5, default: 0.95, step: 0.01, label: "eigenvalue λ" }

draw:
  - hline: { at: 1, dash: true }
  - curve: { is: 0.9 ^ t, dash: true, label: "0.9" }
  - curve: { is: 1.1 ^ t, dash: true, label: "1.1" }
  - curve: { is: lam ^ t, accent: true }
```
:::

::: card
Take $\mathbf{W}_h = \mathrm{diag}(0.9, 1.1)$ over $T = 50$ steps:

$$ \mathbf{W}_h^{49} = \begin{bmatrix} 0.9^{49} & 0 \\ 0 & 1.1^{49} \end{bmatrix} \approx \begin{bmatrix} 0.0057 & 0 \\ 0 & 107 \end{bmatrix} $$

One direction has all but vanished, the other has grown a hundredfold. After 100 steps they are
about $3.0 \times 10^{-5}$ and $12{,}500$.
:::

::: card
The tanh slopes make it worse in one direction. Each is at most 1, so they shrink every factor
further, and vanishing is the more common failure. A vanished gradient means the first inputs get
no learning signal: a network cannot learn that "cat" at step 1 decides the verb at step 40.
:::

::: card
An exploding gradient makes training unstable: one huge update throws the parameters far away.
The common fix is **gradient clipping**. If the gradient's norm exceeds a threshold $\tau$, scale
it down to norm $\tau$, keeping its direction:

$$ \mathbf{g} \leftarrow \mathbf{g} \cdot \min\left(1, \frac{\tau}{\|\mathbf{g}\|}\right) $$

Clipping tames explosions. It does nothing for a gradient that vanishes.

```plot
x: { var: g, label: "norm of the gradient", from: 0, to: 10, ticks: 1, grid: true }
y: { label: "norm after clipping", from: 0, to: 10 }

inputs:
  - { name: tau, min: 0.5, max: 8, default: 3, step: 0.5, label: "threshold τ" }

draw:
  - curve: { is: g, dash: true }
  - curve: { is: "min(g, tau)", accent: true }
```
:::

::: exercise q1
$\mathbf{W}_h = \mathrm{diag}(0.5, 1)$. What is $\mathbf{W}_h^{10}$?

::: answer
$\mathrm{diag}(0.5^{10}, 1) \approx \mathrm{diag}(0.00098, 1)$. Each diagonal entry is raised to
the power.
:::
:::

::: exercise q2
A gradient $\mathbf{g} = [6, 8]^T$ is clipped with $\tau = 5$. What is the result?

::: answer
$[3, 4]^T$. The norm is 10, so the gradient is scaled by $5/10$.
:::
:::

::: exercise q3
An eigenvalue is $0.95$. After how many steps does its power first fall below $0.5$?

::: answer
14 steps. $0.95^{13} \approx 0.513$ and $0.95^{14} \approx 0.488$.
:::

::: solution
Solve $0.95^k < 0.5$: $k > \ln 0.5 / \ln 0.95 = 0.693 / 0.0513 \approx 13.5$.

So $k = 14$. Check: $0.95^{14} \approx 0.488$. ∎
:::
:::

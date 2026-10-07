---
title: The softmax function
---

::: card
A network often ends with a list of raw scores, one for each option, and needs probabilities.
The **softmax** turns a vector $\mathbf{z} \in \mathbb{R}^n$ into a probability distribution:

$$ \mathrm{softmax}(\mathbf{z})_i = \frac{e^{z_i}}{\sum_{j=1}^n e^{z_j}} $$

Every exponential is positive, and dividing by their sum makes the outputs add to 1
([softmax](reference:softmax)).
:::

::: card
Softmax is a smooth version of picking the largest score. When one score stands well above the
others, it takes almost all the probability. Below, three scores go through softmax. Slide
$z_1$ along the horizontal axis and watch its probability (blue) take over. A temperature $T$
divides every score first: a low $T$ sharpens the choice, a high $T$ flattens it.

```plot
x: { var: z, label: "$z₁$", from: -6, to: 6, ticks: 1, grid: true }
y: { label: "probability", from: 0, to: 1 }

inputs:
  - { name: z2, min: -3, max: 3, default: 1, step: 0.1, label: "score z2" }
  - { name: z3, min: -3, max: 3, default: -1, step: 0.1, label: "score z3" }
  - { name: T, min: 0.2, max: 4, default: 1, step: 0.1, label: "temperature T" }

let:
  s: exp(z / T) + exp(z2 / T) + exp(z3 / T)

draw:
  - curve: { is: exp(z / T) / s, accent: true, label: "p1" }
  - curve: { is: exp(z2 / T) / s, label: "p2" }
  - curve: { is: exp(z3 / T) / s, dash: true, label: "p3" }
```
:::

::: card
Adding the same constant $c$ to every score changes nothing:

$$ \frac{e^{z_i + c}}{\sum_j e^{z_j + c}} = \frac{e^c e^{z_i}}{e^c \sum_j e^{z_j}} = \frac{e^{z_i}}{\sum_j e^{z_j}} $$

Only differences between scores matter. A program uses this to stay safe: it subtracts the largest
score before it exponentiates, so no $e^{z_i}$ overflows.
:::

::: card
Softmax has $n$ inputs and $n$ outputs, so its derivative is an $n \times n$ Jacobian. With
$\mathbf{p} = \mathrm{softmax}(\mathbf{z})$:

$$ \frac{\partial p_i}{\partial z_j} = \begin{cases} p_i(1 - p_i) & i = j \\ -p_i p_j & i \ne j \end{cases} \qquad \mathbf{J} = \mathrm{diag}(\mathbf{p}) - \mathbf{p}\mathbf{p}^T $$

Raise one score and its own probability grows, while every other one shrinks
([softmax Jacobian](reference:softmax-jacobian)).
:::

::: card
The diagonal slope $p_1(1 - p_1)$ is the sigmoid's slope in another form. It peaks at $1/4$
when $p_1 = 1/2$ and falls to 0 as $p_1$ nears 0 or 1. A confident softmax passes almost no
gradient back to its scores. Attention puts softmax over its scores, so this saturation shapes
how attention learns.

```plot
x: { var: z, label: "$z₁$", from: -6, to: 6, ticks: 1, grid: true }
y: { label: "value", from: 0, to: 1 }

inputs:
  - { name: z2, min: -3, max: 3, default: 0, step: 0.1, label: "score z2" }

let:
  p: exp(z) / (exp(z) + exp(z2))

draw:
  - curve: { is: p, label: "p1" }
  - curve: { is: p * (1 - p), accent: true, label: "slope of p1" }
  - hline: { at: 0.25, dash: true }
```
:::

::: exercise q1
Compute $\mathrm{softmax}([0, \ln 3])$.

::: answer
$[1/4, 3/4]$. The exponentials are 1 and 3, and their sum is 4.
:::
:::

::: exercise q2
$\mathrm{softmax}([2, 1, 0]) \approx [0.665, 0.245, 0.090]$. What is
$\mathrm{softmax}([102, 101, 100])$?

::: answer
The same, $[0.665, 0.245, 0.090]$. Adding 100 to every score changes nothing.
:::
:::

::: exercise q3
With $\mathbf{p} = [0.5, 0.3, 0.2]$, compute $\frac{\partial p_1}{\partial z_1}$ and
$\frac{\partial p_1}{\partial z_2}$.

::: answer
$0.25$ and $-0.15$. Use $p_1(1 - p_1)$ and $-p_1 p_2$.
:::

::: solution
$\frac{\partial p_1}{\partial z_1} = 0.5 \times (1 - 0.5) = 0.25$

$\frac{\partial p_1}{\partial z_2} = -0.5 \times 0.3 = -0.15$ ∎
:::
:::

::: exercise q4
Every row of the softmax Jacobian sums to the same number. Which one, and why?

::: answer
0. The probabilities always add to 1, so nudging any score cannot change their sum.
:::

::: solution
Row $i$: $p_i(1 - p_i) - \sum_{j \ne i} p_i p_j = p_i - p_i \sum_j p_j = p_i - p_i = 0$. ∎
:::
:::

::: reference softmax
# Softmax

Softmax maps a vector of scores to a probability distribution: every output is positive and
the outputs add to 1. Adding a constant to every score leaves it unchanged.

::: equation
\mathrm{softmax}(\mathbf{z})_i = \frac{e^{z_i}}{\sum_{j=1}^n e^{z_j}}
:::

::: legend
$\mathbf{z}$: the scores, a vector of $\mathbb{R}^n$
$z_i$: the score of option $i$
$n$: the number of options
:::

::: derivation
Each $e^{z_i} > 0$, so each output is positive.
$\sum_i \mathrm{softmax}(\mathbf{z})_i = \sum_i e^{z_i} / \sum_j e^{z_j} = 1$.
For a constant $c$: $e^{z_i + c} / \sum_j e^{z_j + c} = e^c e^{z_i} / (e^c \sum_j e^{z_j}) = e^{z_i} / \sum_j e^{z_j}$. ∎
:::
:::

::: reference softmax-jacobian
# Softmax Jacobian

The derivative of each softmax output with respect to each score. Raising a score raises its own
probability and lowers every other.

::: equation
\frac{\partial p_i}{\partial z_j} = \begin{cases} p_i(1 - p_i) & i = j \\ -p_i p_j & i \ne j \end{cases} \qquad \mathbf{J} = \mathrm{diag}(\mathbf{p}) - \mathbf{p}\mathbf{p}^T
:::

::: legend
$\mathbf{p}$: the softmax output, $p_i = e^{z_i} / \sum_k e^{z_k}$
$z_j$: score $j$
$\mathbf{J}$: the Jacobian, entry $(i, j)$ is $\partial p_i / \partial z_j$
:::

::: derivation
Write $p_i = e^{z_i} / S$ with $S = \sum_k e^{z_k}$, so $\partial S / \partial z_j = e^{z_j}$.
Case $i = j$: $\frac{\partial p_i}{\partial z_i} = \frac{e^{z_i} S - e^{z_i} e^{z_i}}{S^2}$.[quotient rule](reference:quotient-rule)
This is $\frac{e^{z_i}}{S} \cdot \frac{S - e^{z_i}}{S} = p_i(1 - p_i)$.
Case $i \ne j$: the numerator $e^{z_i}$ does not depend on $z_j$, so $\frac{\partial p_i}{\partial z_j} = \frac{0 \cdot S - e^{z_i} e^{z_j}}{S^2} = -p_i p_j$.
In matrix form, the diagonal is $p_i$ minus $p_i^2$ and every entry subtracts $p_i p_j$: $\mathbf{J} = \mathrm{diag}(\mathbf{p}) - \mathbf{p}\mathbf{p}^T$. ∎
:::
:::

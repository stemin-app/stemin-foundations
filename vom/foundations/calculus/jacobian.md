---
title: The Jacobian matrix
---

::: card
Now let both sides be vectors: an input $\mathbf{x} \in \mathbb{R}^n$ and an output
$\mathbf{y} \in \mathbb{R}^m$. Each of the $m$ outputs can depend on each of the $n$ inputs, so
there are $m \times n$ partial derivatives. The **Jacobian matrix** holds them all:

$$ \mathbf{J} = \begin{bmatrix} \frac{\partial y_1}{\partial x_1} & \cdots & \frac{\partial y_1}{\partial x_n} \\ \vdots & \ddots & \vdots \\ \frac{\partial y_m}{\partial x_1} & \cdots & \frac{\partial y_m}{\partial x_n} \end{bmatrix} $$

[Jacobian](reference:jacobian)
:::

::: card
Read it two ways. Row $j$ is the gradient of the single output $y_j$, laid on its side. Column $i$
tells how every output responds to the single input $x_i$. The shape is $m \times n$: one row per
output, one column per input.
:::

::: card
Take the function with

$$ y_1 = x_1^2 + x_2, \qquad y_2 = x_1 x_2 $$

Differentiate each output by each input:

$$ \mathbf{J} = \begin{bmatrix} \frac{\partial y_1}{\partial x_1} & \frac{\partial y_1}{\partial x_2} \\ \frac{\partial y_2}{\partial x_1} & \frac{\partial y_2}{\partial x_2} \end{bmatrix} = \begin{bmatrix} 2x_1 & 1 \\ x_2 & x_1 \end{bmatrix} $$
:::

::: card
At the point $(x_1, x_2) = (3, 2)$ the outputs are $y_1 = 9 + 2 = 11$ and $y_2 = 6$, and the
Jacobian is

$$ \mathbf{J} = \begin{bmatrix} 6 & 1 \\ 2 & 3 \end{bmatrix} $$
:::

::: card
The Jacobian predicts how a small change of input moves the output,
$\Delta \mathbf{y} \approx \mathbf{J} \Delta \mathbf{x}$. Nudge the input by
$\Delta \mathbf{x} = [0.1, 0.2]^T$:

$$ \Delta \mathbf{y} \approx \begin{bmatrix} 6 & 1 \\ 2 & 3 \end{bmatrix} \begin{bmatrix} 0.1 \\ 0.2 \end{bmatrix} = \begin{bmatrix} 6(0.1) + 1(0.2) \\ 2(0.1) + 3(0.2) \end{bmatrix} = \begin{bmatrix} 0.8 \\ 0.8 \end{bmatrix} $$

The product is a matrix-vector product.
[Matrix-vector product](reference:matrix-vector-product)
:::

::: card
Check at $(3.1, 2.2)$. Then $y_1 = 9.61 + 2.2 = 11.81$, a change of $0.81$, and
$y_2 = 3.1 \times 2.2 = 6.82$, a change of $0.82$. The prediction $(0.8, 0.8)$ is close. Scale
the nudge by $s$: the true changes are $0.8s + 0.01s^2$ and $0.8s + 0.02s^2$. The gap grows
with $s^2$, so the linear prediction is exact only in the limit of tiny changes.

```plot
x: { var: s, label: "the scale $s$ of the nudge $s · [0.1, 0.2]$", from: 0, to: 4, ticks: 0.5, grid: true }
y: { label: "the change in output", from: 0, to: 3.8 }

draw:
  - curve: { is: 0.8 * s, accent: true, dash: true, label: "$J Δ x$, both" }
  - curve: { is: 0.8 * s + 0.01 * s^2, label: "true $Δ y₁$" }
  - curve: { is: 0.8 * s + 0.02 * s^2, label: "true $Δ y₂$" }
  - vline: { at: 1, dash: true }
  - point: { at: [1, 0.81], label: "0.81" }
  - point: { at: [1, 0.82], label: "0.82" }
```
:::

::: card
The Jacobian is the derivative for vector functions. With one input and one output it is the
$1 \times 1$ matrix $\frac{df}{dx}$. With $n$ inputs and one output it is the $1 \times n$ row
$(\nabla f)^T$. In every case it answers the same question: how does the output respond to a
small change of input?
:::

::: exercise jacobian-of-pair
For $y_1 = x_1 x_2$ and $y_2 = x_1 + x_2^2$, what is the Jacobian at $(1, 2)$?

::: answer
$\begin{bmatrix} 2 & 1 \\ 1 & 4 \end{bmatrix}$. Row one is $[x_2, x_1]$, row two is $[1, 2x_2]$.
:::

::: solution
$$ \mathbf{J} = \begin{bmatrix} x_2 & x_1 \\ 1 & 2x_2 \end{bmatrix} = \begin{bmatrix} 2 & 1 \\ 1 & 4 \end{bmatrix} $$

∎
:::
:::

::: exercise jacobian-shape
A function maps $\mathbb{R}^3$ to $\mathbb{R}^2$. What is the shape of its Jacobian?

::: answer
$2 \times 3$. One row per output, one column per input.
:::
:::

::: exercise jacobian-prediction
At a point where $\mathbf{J} = \begin{bmatrix} 2 & 0 \\ 1 & 3 \end{bmatrix}$, the input moves by
$\Delta \mathbf{x} = [0.1, -0.1]^T$. What change of output does the Jacobian predict?

::: answer
$[0.2, -0.2]^T$. Multiply $\mathbf{J} \Delta \mathbf{x}$.
:::

::: solution
$$ \begin{bmatrix} 2 & 0 \\ 1 & 3 \end{bmatrix} \begin{bmatrix} 0.1 \\ -0.1 \end{bmatrix} = \begin{bmatrix} 0.2 + 0 \\ 0.1 - 0.3 \end{bmatrix} = \begin{bmatrix} 0.2 \\ -0.2 \end{bmatrix} $$

∎
:::
:::

::: reference jacobian
# Jacobian matrix

The Jacobian of a function from $\mathbb{R}^n$ to $\mathbb{R}^m$ is the $m \times n$ matrix of all
its partial derivatives. It gives the best linear prediction of how a small change of input moves
the output.

::: equation
J_{ji} = \frac{\partial y_j}{\partial x_i} \qquad \Delta \mathbf{y} \approx \mathbf{J}\, \Delta \mathbf{x}
:::

::: legend
$\mathbf{x}$: the input, a vector with n entries
$\mathbf{y}$: the output, a vector with m entries
$J_{ji}$: the entry in row j and column i
$\Delta \mathbf{x}$: a small change of input
:::

::: derivation
Output $y_j$ depends on all the inputs. A small change $\Delta \mathbf{x}$ moves it by $\Delta y_j \approx \sum_i \frac{\partial y_j}{\partial x_i} \Delta x_i$.[Multivariable chain rule](reference:multivariable-chain-rule)
That sum is row $j$ of $\mathbf{J}$ times $\Delta \mathbf{x}$.[Matrix-vector product](reference:matrix-vector-product)
Stack the $m$ rows: $\Delta \mathbf{y} \approx \mathbf{J} \Delta \mathbf{x}$. ∎
:::
:::

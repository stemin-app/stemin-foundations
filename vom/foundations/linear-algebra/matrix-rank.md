---
title: Rank
---

::: card
The **rank** of a matrix is the number of independent directions in its output: the dimension
of the space its columns reach. Read $\mathbf{A} \in \mathbb{R}^{m \times n}$ as a map from
$\mathbb{R}^n$ to $\mathbb{R}^m$. The rank says how much of $\mathbb{R}^m$ the map can reach.
Counting independent rows gives the same number.
:::

::: card
$\begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$ has rank 2: its columns are independent, and it
reaches every point of the plane. Now take

$$ \mathbf{B} = \begin{bmatrix} 1 & 2 \\ 2 & 4 \end{bmatrix} $$

Its second column is twice the first. Both columns lie on the line through $[1, 2]^T$.
:::

::: card
$\mathbf{B}\mathbf{x}$ mixes the columns with the weights $x_1, x_2$, and both columns sit on
one line, so every mix stays on it:

$$ \mathbf{B}\mathbf{x} = x_1\begin{bmatrix} 1 \\ 2 \end{bmatrix} + x_2\begin{bmatrix} 2 \\ 4 \end{bmatrix} = (x_1 + 2x_2)\begin{bmatrix} 1 \\ 2 \end{bmatrix} $$

The plane collapses onto a line. $\mathbf{B}$ has rank 1.
:::

::: card
Below, the unit circle goes through $\begin{bmatrix} 1 & 2 \\ 2 & k \end{bmatrix}$. For most
$k$ it becomes an ellipse, and the map reaches the whole plane. Move $k$ to 4: the second column
becomes twice the first, and the ellipse flattens into a segment on the line through $[1, 2]^T$.

```plot
x: { var: t, label: "$y₁$", from: -6, to: 6, ticks: 1, grid: true }
y: { label: "$y₂$", from: -5, to: 5 }

inputs:
  - { name: k, min: 1, max: 7, default: 2, step: 0.25, label: "the entry k" }

draw:
  - param: { var: s, over: [0, 2 * pi()], x: cos(s), y: sin(s), dash: true }
  - param: { var: s, over: [-2.5, 2.5], x: s, y: 2 * s, dash: true }
  - param: { var: s, over: [0, 2 * pi()], x: cos(s) + 2 * sin(s), y: 2 * cos(s) + k * sin(s), accent: true }
```
:::

::: card
A collapse loses information. Take $\mathbf{x}_1 = [1, 0]^T$ and $\mathbf{x}_2 = [-1, 1]^T$:

$$ \mathbf{B}\mathbf{x}_1 = \begin{bmatrix} 1 \\ 2 \end{bmatrix}, \qquad \mathbf{B}\mathbf{x}_2 = \begin{bmatrix} -1 + 2 \\ -2 + 4 \end{bmatrix} = \begin{bmatrix} 1 \\ 2 \end{bmatrix} $$

Two different inputs, one output. Given $[1, 2]^T$, you cannot tell which input made it. The
direction perpendicular to the line is wiped out, so no matrix can undo $\mathbf{B}$.
:::

::: card
A matrix has **full rank** when its rank equals the smaller of its two sizes. A square
$n \times n$ matrix of full rank has rank $n$. It loses nothing, so it has an inverse
$\mathbf{A}^{-1}$ that undoes it: $\mathbf{A}^{-1}\mathbf{A} = \mathbf{I}$. A square matrix
below full rank collapses space and has no inverse.
:::

::: exercise q1
What is the rank of $\begin{bmatrix} 2 & -1 \\ -6 & 3 \end{bmatrix}$?

::: answer
1. The second column is $-1/2$ times the first.
:::
:::

::: exercise q2
Find two different vectors that $\begin{bmatrix} 1 & 2 \\ 2 & 4 \end{bmatrix}$ sends to zero.

::: answer
For example $[2, -1]^T$ and $[-4, 2]^T$. Any multiple of $[2, -1]^T$ works, because it makes
$x_1 + 2x_2 = 0$.
:::

::: solution
$\mathbf{B}\mathbf{x} = (x_1 + 2x_2)[1, 2]^T$, which is zero when $x_1 = -2x_2$.

$x_2 = -1$ gives $[2, -1]^T$: $\mathbf{B}[2, -1]^T = [2 - 2, 4 - 4]^T = \mathbf{0}$.

$x_2 = 2$ gives $[-4, 2]^T$: $\mathbf{B}[-4, 2]^T = [-4 + 4, -8 + 8]^T = \mathbf{0}$. ∎
:::
:::

::: exercise q3
A $3 \times 5$ matrix has rank 3. Is it full rank, and does it have an inverse?

::: answer
It is full rank, since $3 = \min(3, 5)$. It has no inverse: only a square matrix can.
:::
:::

---
title: A matrix as a map
---

::: card
A **matrix** is a rectangular array of numbers, named with a bold uppercase letter:

$$ \mathbf{A} = \begin{bmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\ a_{21} & a_{22} & \cdots & a_{2n} \\ \vdots & \vdots & \ddots & \vdots \\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{bmatrix} $$

It has $m$ rows and $n$ columns, so $\mathbf{A} \in \mathbb{R}^{m \times n}$. The entry in row
$i$ and column $j$ is $a_{ij}$, also written $[\mathbf{A}]_{ij}$.
:::

::: card
Name the parts of a matrix by its letter. The column $j$ of $\mathbf{A}$ is the vector
$\mathbf{a}_j$, and the row $i$, read as a row vector, is $\mathbf{a}_{i:}$. So a matrix is $n$
columns side by side, or $m$ rows stacked.
:::

::: card
A matrix acts on a vector. Multiply $\mathbf{x} \in \mathbb{R}^n$ by $\mathbf{A} \in \mathbb{R}^{m \times n}$
and you get $\mathbf{y} = \mathbf{A}\mathbf{x} \in \mathbb{R}^m$, with components

$$ y_i = \sum_{j=1}^n a_{ij} x_j $$

The number of columns of $\mathbf{A}$ must match the length of $\mathbf{x}$. The map is linear,
and every linear map from $\mathbb{R}^n$ to $\mathbb{R}^m$ is a matrix
([matrix-vector product](reference:matrix-vector-product)).
:::

::: card
**The row view.** Each $y_i$ is the dot product of row $i$ of $\mathbf{A}$ with $\mathbf{x}$:

$$ \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{bmatrix} \begin{bmatrix} 7 \\ 8 \\ 9 \end{bmatrix} = \begin{bmatrix} 7 + 16 + 27 \\ 28 + 40 + 54 \end{bmatrix} = \begin{bmatrix} 50 \\ 122 \end{bmatrix} $$

The 50 is row $[1, 2, 3]$ dotted with $[7, 8, 9]$. The 122 is row $[4, 5, 6]$ dotted with the
same vector. This view tells you how each output depends on the input.
:::

::: card
**The column view.** $\mathbf{y}$ is a linear combination of the columns of $\mathbf{A}$, with
the components of $\mathbf{x}$ as weights: $\mathbf{y} = x_1\mathbf{a}_1 + \cdots + x_n\mathbf{a}_n$.

$$ 7\begin{bmatrix} 1 \\ 4 \end{bmatrix} + 8\begin{bmatrix} 2 \\ 5 \end{bmatrix} + 9\begin{bmatrix} 3 \\ 6 \end{bmatrix} = \begin{bmatrix} 50 \\ 122 \end{bmatrix} $$

Seven copies of the first column, eight of the second, nine of the third. The same answer, read
as a mix of columns.
:::

::: card
The column view says where the basis vectors go. $\mathbf{A}\mathbf{e}_1$ picks out the first
column, $\mathbf{A}\mathbf{e}_2$ the second. Every other vector is a combination of the
$\mathbf{e}_j$, and the map is linear, so the columns fix the whole map. Change the entries
below and the unit circle (dashed) turns into an ellipse. The two columns are drawn from the
origin.

```plot
x: { var: t, label: "$x₁$", from: -5, to: 5, ticks: 1, grid: true }
y: { label: "$x₂$", from: -3, to: 3 }

inputs:
  - { name: a, min: -2, max: 2, default: 1.5, step: 0.1, label: "a11" }
  - { name: b, min: -2, max: 2, default: 0.5, step: 0.1, label: "a12" }
  - { name: c, min: -2, max: 2, default: 0.3, step: 0.1, label: "a21" }
  - { name: d, min: -2, max: 2, default: 1, step: 0.1, label: "a22" }

draw:
  - param: { var: s, over: [0, 2 * pi()], x: cos(s), y: sin(s), dash: true }
  - param: { var: s, over: [0, 2 * pi()], x: a * cos(s) + b * sin(s), y: c * cos(s) + d * sin(s), accent: true }
  - param: { var: s, over: [0, 1], x: s * a, y: s * c, label: "A e1" }
  - param: { var: s, over: [0, 1], x: s * b, y: s * d, label: "A e2" }
```
:::

::: card
Try $a_{12} = a_{21} = 0$: the circle stretches along the axes and nothing turns. Set the two
columns parallel, for example $a_{11} = 1$, $a_{21} = 1$, $a_{12} = 2$, $a_{22} = 2$, and the
ellipse flattens to a segment. A matrix with parallel columns crushes the plane onto a line.
:::

::: exercise q1
Compute $\begin{bmatrix} 2 & -1 \\ 0 & 3 \end{bmatrix}\begin{bmatrix} 4 \\ 1 \end{bmatrix}$.

::: answer
$\begin{bmatrix} 7 \\ 3 \end{bmatrix}$. Dot each row with the vector.
:::

::: solution
Row 1: $2 \cdot 4 + (-1) \cdot 1 = 7$.

Row 2: $0 \cdot 4 + 3 \cdot 1 = 3$. ∎
:::
:::

::: exercise q2
A matrix $\mathbf{W} \in \mathbb{R}^{64 \times 512}$ multiplies a vector $\mathbf{x}$. How long
must $\mathbf{x}$ be, and how long is $\mathbf{W}\mathbf{x}$?

::: answer
$\mathbf{x} \in \mathbb{R}^{512}$ and $\mathbf{W}\mathbf{x} \in \mathbb{R}^{64}$. The input
length matches the columns; the output length matches the rows.
:::
:::

::: exercise q3
Use the column view to compute $\begin{bmatrix} 1 & 0 & 2 \\ 0 & 1 & -1 \end{bmatrix}\begin{bmatrix} 1 \\ 1 \\ 1 \end{bmatrix}$.

::: answer
$\begin{bmatrix} 3 \\ 0 \end{bmatrix}$. With all weights 1, add the three columns.
:::

::: solution
$$ 1\begin{bmatrix} 1 \\ 0 \end{bmatrix} + 1\begin{bmatrix} 0 \\ 1 \end{bmatrix} + 1\begin{bmatrix} 2 \\ -1 \end{bmatrix} = \begin{bmatrix} 3 \\ 0 \end{bmatrix} $$

∎
:::
:::

::: exercise q4
Where does $\begin{bmatrix} 3 & 1 \\ 1 & 2 \end{bmatrix}$ send $\mathbf{e}_2$?

::: answer
To $\begin{bmatrix} 1 \\ 2 \end{bmatrix}$, the second column.
:::
:::

::: reference matrix-vector-product
# Matrix-vector product

A matrix $\mathbf{A} \in \mathbb{R}^{m \times n}$ maps a vector $\mathbf{x} \in \mathbb{R}^n$
to $\mathbf{y} \in \mathbb{R}^m$. Each component of $\mathbf{y}$ is the dot product of a row of
$\mathbf{A}$ with $\mathbf{x}$; together, $\mathbf{y}$ is the combination of the columns of
$\mathbf{A}$ weighted by the components of $\mathbf{x}$. The map is linear.

::: equation
y_i = \sum_{j=1}^n a_{ij} x_j \qquad \mathbf{y} = \mathbf{A}\mathbf{x} = x_1\mathbf{a}_1 + x_2\mathbf{a}_2 + \cdots + x_n\mathbf{a}_n
:::

::: legend
$\mathbf{A}$: the matrix, $m$ rows and $n$ columns
$a_{ij}$: the entry in row $i$, column $j$
$\mathbf{a}_j$: column $j$ of $\mathbf{A}$
$\mathbf{x}$: the input vector, length $n$
$\mathbf{y}$: the output vector, length $m$
:::

::: derivation
Row $i$ of $\mathbf{A}$ dotted with $\mathbf{x}$ is $\sum_j a_{ij} x_j = y_i$.[dot product](reference:dot-product)
Stack the components: $\mathbf{y} = \sum_j x_j \begin{bmatrix} a_{1j} & \cdots & a_{mj} \end{bmatrix}^T = \sum_j x_j \mathbf{a}_j$.
Linearity: $\sum_j a_{ij}(\alpha u_j + \beta v_j) = \alpha \sum_j a_{ij} u_j + \beta \sum_j a_{ij} v_j$, so $\mathbf{A}(\alpha\mathbf{u} + \beta\mathbf{v}) = \alpha\mathbf{A}\mathbf{u} + \beta\mathbf{A}\mathbf{v}$.[linear map](reference:linear-map) ∎
:::
:::

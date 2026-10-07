---
title: Multiplying matrices
---

::: card
Two matrices multiply when the inner sizes match. For $\mathbf{A} \in \mathbb{R}^{m \times n}$
and $\mathbf{B} \in \mathbb{R}^{n \times p}$, the product $\mathbf{C} = \mathbf{A}\mathbf{B}$
lies in $\mathbb{R}^{m \times p}$, and each entry is a row of $\mathbf{A}$ dotted with a column
of $\mathbf{B}$:

$$ c_{ij} = \sum_{k=1}^n a_{ik} b_{kj} $$

The shared size $n$ disappears. The outer sizes $m$ and $p$ are what remain.
:::

::: card
Read the product as two maps in a row. $\mathbf{B}$ acts first, then $\mathbf{A}$:

$$ (\mathbf{A}\mathbf{B})\mathbf{x} = \mathbf{A}(\mathbf{B}\mathbf{x}) $$

Column $j$ of $\mathbf{A}\mathbf{B}$ is $\mathbf{A}$ applied to column $j$ of $\mathbf{B}$. So a
product of matrices is a chain of linear maps, packed into one matrix
([matrix product](reference:matrix-product)).
:::

::: card
The product is associative, $\mathbf{A}(\mathbf{B}\mathbf{C}) = (\mathbf{A}\mathbf{B})\mathbf{C}$,
and distributive, $\mathbf{A}(\mathbf{B} + \mathbf{C}) = \mathbf{A}\mathbf{B} + \mathbf{A}\mathbf{C}$.
It is not commutative. The order matters:

$$ \begin{bmatrix} 1 & 2 \\ 0 & 1 \end{bmatrix} \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix} = \begin{bmatrix} 2 & 1 \\ 1 & 0 \end{bmatrix}, \qquad \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix} \begin{bmatrix} 1 & 2 \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 1 \\ 1 & 2 \end{bmatrix} $$
:::

::: card
Call the first matrix $\mathbf{A}$, a shear, and the second $\mathbf{B}$, a swap of the two
axes. Below, the unit circle goes through both maps in the two orders. Turn the input vector
$\mathbf{x}$ and follow where it lands: $\mathbf{A}\mathbf{B}\mathbf{x}$ on the solid ellipse,
$\mathbf{B}\mathbf{A}\mathbf{x}$ on the dashed one. The two points never meet: on
the unit circle, no input gives the same output in both orders.

```plot
x: { var: t, label: "$x₁$", from: -4, to: 4, ticks: 1, grid: true }
y: { label: "$x₂$", from: -3, to: 3 }

inputs:
  - { name: deg, min: 0, max: 360, default: 30, step: 5, label: "angle of x, in degrees" }

let:
  th: deg * pi() / 180

draw:
  - param: { var: s, over: [0, 2 * pi()], x: 2 * cos(s) + sin(s), y: cos(s), accent: true, label: "AB" }
  - param: { var: s, over: [0, 2 * pi()], x: sin(s), y: cos(s) + 2 * sin(s), dash: true, label: "BA" }
  - param: { var: s, over: [0, 1], x: s * cos(th), y: s * sin(th), label: "x" }
  - point: { at: [2 * cos(th) + sin(th), cos(th)], label: "ABx", accent: true }
  - point: { at: [sin(th), cos(th) + 2 * sin(th)], label: "BAx" }
```
:::

::: card
The **transpose** $\mathbf{A}^T$ swaps rows and columns: $[\mathbf{A}^T]_{ij} = a_{ji}$. A
matrix of size $m \times n$ becomes $n \times m$. Three rules follow:

$$ (\mathbf{A}^T)^T = \mathbf{A}, \qquad (\mathbf{A} + \mathbf{B})^T = \mathbf{A}^T + \mathbf{B}^T, \qquad (\mathbf{A}\mathbf{B})^T = \mathbf{B}^T\mathbf{A}^T $$

The last one reverses the order. The sizes force it: $\mathbf{B}^T$ is $p \times n$ and
$\mathbf{A}^T$ is $n \times m$, so only $\mathbf{B}^T\mathbf{A}^T$ fits.
:::

::: card
A square matrix is **symmetric** when $\mathbf{A}^T = \mathbf{A}$: the entry in row $i$,
column $j$ equals the entry in row $j$, column $i$. A table of dot products between a set of
vectors is symmetric, because $\mathbf{u} \cdot \mathbf{v} = \mathbf{v} \cdot \mathbf{u}$.
Attention scores are not, as you will see: a query and a key are made by two different matrices.
:::

::: exercise q1
Compute $\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}\begin{bmatrix} 0 & 1 \\ 1 & 1 \end{bmatrix}$.

::: answer
$\begin{bmatrix} 2 & 3 \\ 4 & 7 \end{bmatrix}$. Each entry is a row of the first matrix dotted
with a column of the second.
:::

::: solution
$c_{11} = 1 \cdot 0 + 2 \cdot 1 = 2$

$c_{12} = 1 \cdot 1 + 2 \cdot 1 = 3$

$c_{21} = 3 \cdot 0 + 4 \cdot 1 = 4$

$c_{22} = 3 \cdot 1 + 4 \cdot 1 = 7$ ∎
:::
:::

::: exercise q2
$\mathbf{Q} \in \mathbb{R}^{10 \times 64}$ and $\mathbf{K} \in \mathbb{R}^{10 \times 64}$. What
is the size of $\mathbf{Q}\mathbf{K}^T$? Does $\mathbf{Q}\mathbf{K}$ exist?

::: answer
$\mathbf{Q}\mathbf{K}^T$ is $10 \times 10$. $\mathbf{Q}\mathbf{K}$ does not exist: the inner
sizes 64 and 10 differ.
:::
:::

::: exercise q3
$\mathbf{A}$ is $3 \times 5$ and $\mathbf{B}$ is $5 \times 2$. What is the size of
$(\mathbf{A}\mathbf{B})^T$, and which product of transposes equals it?

::: answer
$2 \times 3$, and it equals $\mathbf{B}^T\mathbf{A}^T$. The transpose of a product reverses
the order.
:::
:::

::: reference matrix-product
# Matrix product

The product of $\mathbf{A} \in \mathbb{R}^{m \times n}$ and $\mathbf{B} \in \mathbb{R}^{n \times p}$
is the matrix $\mathbf{C} \in \mathbb{R}^{m \times p}$ whose entry $c_{ij}$ is row $i$ of
$\mathbf{A}$ dotted with column $j$ of $\mathbf{B}$. It is the matrix of the map "apply
$\mathbf{B}$, then $\mathbf{A}$". It is associative and distributive, not commutative, and its
transpose reverses the order.

::: equation
c_{ij} = \sum_{k=1}^n a_{ik} b_{kj} \qquad (\mathbf{A}\mathbf{B})\mathbf{x} = \mathbf{A}(\mathbf{B}\mathbf{x}) \qquad (\mathbf{A}\mathbf{B})^T = \mathbf{B}^T\mathbf{A}^T
:::

::: legend
$\mathbf{A}$: an $m \times n$ matrix with entries $a_{ik}$
$\mathbf{B}$: an $n \times p$ matrix with entries $b_{kj}$
$\mathbf{C}$: the $m \times p$ product
$\mathbf{x}$: any vector of $\mathbb{R}^p$
:::

::: derivation
Component $k$ of $\mathbf{B}\mathbf{x}$ is $\sum_j b_{kj} x_j$.[matrix-vector product](reference:matrix-vector-product)
Apply $\mathbf{A}$: $[\mathbf{A}(\mathbf{B}\mathbf{x})]_i = \sum_k a_{ik} \sum_j b_{kj} x_j = \sum_j \left(\sum_k a_{ik} b_{kj}\right) x_j$.
The bracket is $c_{ij}$, so $\mathbf{A}(\mathbf{B}\mathbf{x}) = \mathbf{C}\mathbf{x}$.
Transpose: $[(\mathbf{A}\mathbf{B})^T]_{ij} = c_{ji} = \sum_k a_{jk} b_{ki} = \sum_k [\mathbf{B}^T]_{ik} [\mathbf{A}^T]_{kj} = [\mathbf{B}^T\mathbf{A}^T]_{ij}$. ∎
:::
:::

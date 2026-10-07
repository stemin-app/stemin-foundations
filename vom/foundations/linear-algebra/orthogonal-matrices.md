---
title: Orthogonal matrices
---

::: card
A square matrix $\mathbf{Q}$ is **orthogonal** when

$$ \mathbf{Q}^T\mathbf{Q} = \mathbf{I} $$

Entry $(i, j)$ of $\mathbf{Q}^T\mathbf{Q}$ is column $i$ dotted with column $j$. So each column
has length 1, and different columns are perpendicular: the columns form an **orthonormal**
basis. The inverse is free: $\mathbf{Q}^{-1} = \mathbf{Q}^T$.
:::

::: card
An orthogonal matrix keeps lengths and angles:

$$ \|\mathbf{Q}\mathbf{v}\|_2 = \|\mathbf{v}\|_2, \qquad (\mathbf{Q}\mathbf{u}) \cdot (\mathbf{Q}\mathbf{v}) = \mathbf{u} \cdot \mathbf{v} $$

The proof is one line: $(\mathbf{Q}\mathbf{u})^T(\mathbf{Q}\mathbf{v}) = \mathbf{u}^T\mathbf{Q}^T\mathbf{Q}\mathbf{v} = \mathbf{u}^T\mathbf{v}$.
Lengths follow with $\mathbf{u} = \mathbf{v}$. It moves vectors without bending or stretching
anything: a rotation or a reflection.
:::

::: card
Rotate two vectors together. Their lengths do not change, and neither does the angle between
them. Switch the reflection on and the pair is mirrored across the horizontal axis first: still
the same lengths and the same angle, with the order of the two reversed.

```plot
x: { var: t, label: "$x₁$", from: -4, to: 4, ticks: 1, grid: true }
y: { label: "$x₂$", from: -3, to: 3 }

inputs:
  - { name: deg, min: 0, max: 360, default: 40, step: 5, label: "rotation, in degrees" }
  - { name: f, min: 0, max: 1, default: 0, step: 1, label: "reflection, off or on" }

let:
  th: deg * pi() / 180
  sg: 1 - 2 * f
  ux: 2.5 * cos(th)
  uy: 2.5 * sin(th)
  vx: cos(th) - sg * 1.5 * sin(th)
  vy: sin(th) + sg * 1.5 * cos(th)

draw:
  - param: { var: s, over: [0, 1], x: s * 2.5, y: 0, dash: true }
  - param: { var: s, over: [0, 1], x: s, y: s * 1.5, dash: true }
  - param: { var: s, over: [0, 1], x: s * ux, y: s * uy, accent: true }
  - param: { var: s, over: [0, 1], x: s * vx, y: s * vy, accent: true }
  - point: { at: [ux, uy], label: "Qu" }
  - point: { at: [vx, vy], label: "Qv" }
```
:::

::: card
The determinant of an orthogonal matrix is $+1$ or $-1$: no area is gained or lost. A rotation
has $+1$. A reflection has $-1$, because it reverses orientation. Take
$\det(\mathbf{Q}^T\mathbf{Q}) = \det(\mathbf{Q})^2 = \det\mathbf{I} = 1$.
:::

::: card
A shear is the counterexample. $\begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}$ has determinant 1,
so it keeps area, and it is still not orthogonal: its second column $[1, 1]^T$ has length
$\sqrt{2}$, and it sends $[0, 1]^T$ to $[1, 1]^T$. Keeping area is not enough. An orthogonal
matrix keeps every length.
:::

::: exercise q1
Is $\frac{1}{5}\begin{bmatrix} 3 & -4 \\ 4 & 3 \end{bmatrix}$ orthogonal?

::: answer
Yes. Each column has length 1, and the columns' dot product is $(-12 + 12)/25 = 0$.
:::
:::

::: exercise q2
$\mathbf{Q}$ is orthogonal and $\|\mathbf{v}\|_2 = 7$. What is $\|\mathbf{Q}^3\mathbf{v}\|_2$?

::: answer
7. Each product by $\mathbf{Q}$ keeps the length.
:::
:::

::: exercise q3
What is the inverse of $\begin{bmatrix} 0 & 1 \\ -1 & 0 \end{bmatrix}$?

::: answer
$\begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$, its transpose. The matrix is orthogonal.
:::
:::

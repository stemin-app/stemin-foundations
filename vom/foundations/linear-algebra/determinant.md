---
title: The determinant
---

::: card
The **determinant** of a square matrix is the factor by which it scales area, or volume in
higher dimensions. $\begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix}$ stretches the unit square into
a $2 \times 3$ rectangle of area 6, and its determinant is $2 \cdot 3 - 0 \cdot 0 = 6$.
:::

::: card
The unit square has its sides along $\mathbf{e}_1$ and $\mathbf{e}_2$. A matrix sends them to
its columns:

$$ \mathbf{A} = \begin{bmatrix} a & b \\ c & d \end{bmatrix}: \qquad \mathbf{e}_1 \mapsto \begin{bmatrix} a \\ c \end{bmatrix}, \qquad \mathbf{e}_2 \mapsto \begin{bmatrix} b \\ d \end{bmatrix} $$

So the square becomes the parallelogram spanned by the columns. Its signed area is
$\det\mathbf{A} = ad - bc$ ([determinant](reference:determinant)).
:::

::: card
Change the columns and watch the parallelogram. Spread them apart and it grows. Turn them toward
each other and it thins out. Make them parallel and it collapses to a segment of area 0. Swap
their order around the origin and the area turns negative.

```plot
x: { var: t, label: "$x₁$", from: -4, to: 5, ticks: 1, grid: true }
y: { label: "$x₂$", from: -2, to: 4 }

inputs:
  - { name: a, min: -2, max: 3, default: 3, step: 0.25, label: "a" }
  - { name: c, min: -2, max: 3, default: 1, step: 0.25, label: "c" }
  - { name: b, min: -2, max: 3, default: 1, step: 0.25, label: "b" }
  - { name: d, min: -2, max: 3, default: 2, step: 0.25, label: "d" }

draw:
  - param: { var: s, over: [0, 1], x: s, y: 0, dash: true }
  - param: { var: s, over: [0, 1], x: 0, y: s, dash: true }
  - param: { var: s, over: [0, 1], x: 1, y: s, dash: true }
  - param: { var: s, over: [0, 1], x: s, y: 1, dash: true }
  - param: { var: s, over: [0, 1], x: s * a, y: s * c, accent: true }
  - param: { var: s, over: [0, 1], x: s * b, y: s * d, accent: true }
  - param: { var: s, over: [0, 1], x: a + s * b, y: c + s * d }
  - param: { var: s, over: [0, 1], x: b + s * a, y: d + s * c }
  - point: { at: [a, c], label: "A e1" }
  - point: { at: [b, d], label: "A e2" }
```
:::

::: card
With the default columns $[3, 1]^T$ and $[1, 2]^T$, the square's corners go to $(0, 0)$,
$(3, 1)$, $(1, 2)$ and $(4, 3)$. The determinant is

$$ \det\begin{bmatrix} 3 & 1 \\ 1 & 2 \end{bmatrix} = 3 \cdot 2 - 1 \cdot 1 = 5 $$

The square had area 1 and the parallelogram has area 5. Every shape scales the same way: a
triangle of area 10 becomes one of area 50.
:::

::: card
The rank 1 matrix $\begin{bmatrix} 1 & 2 \\ 2 & 4 \end{bmatrix}$ sends the corners to
$(0, 0)$, $(1, 2)$, $(2, 4)$ and $(3, 6)$, all on one line. The square becomes a segment with
no area, and $\det = 1 \cdot 4 - 2 \cdot 2 = 0$. In general, $\det\mathbf{A} = 0$ exactly when
$\mathbf{A}$ is not full rank. A nonzero determinant means the map can be undone.
:::

::: card
The sign tracks orientation. $\begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$ flips the
vertical axis. Its determinant is $-1$: the area stays 1, but the corners, met
counterclockwise before, are met clockwise after. A positive determinant keeps the
orientation, a negative one mirrors it.
:::

::: card
In three dimensions the determinant scales volume:

$$ \det\begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix} = a(ei - fh) - b(di - fg) + c(dh - eg) $$

For a diagonal matrix it is the product of the diagonal. $\mathrm{diag}(2, 3, 1)$ turns the unit
cube into a $2 \times 3 \times 1$ box, and its determinant is 6.
:::

::: exercise q1
Compute $\det\begin{bmatrix} 4 & 2 \\ 1 & 3 \end{bmatrix}$. What area does the unit square get?

::: answer
10, so the area becomes 10. $4 \cdot 3 - 2 \cdot 1 = 10$.
:::
:::

::: exercise q2
For which $k$ does $\begin{bmatrix} 2 & k \\ 3 & 6 \end{bmatrix}$ have no inverse?

::: answer
$k = 4$. Set $12 - 3k = 0$.
:::
:::

::: exercise q3
A matrix has determinant $-2$. A triangle of area 3 goes through it. What is the new area, and
what happens to its orientation?

::: answer
6, and the orientation is reversed. The area scales by $|-2|$, and the minus sign mirrors it.
:::
:::

::: exercise q4
Compute $\det\begin{bmatrix} 1 & 0 & 2 \\ 0 & 3 & 0 \\ 1 & 0 & 4 \end{bmatrix}$.

::: answer
6. Expand along the first row.
:::

::: solution
$a(ei - fh) = 1 \cdot (3 \cdot 4 - 0 \cdot 0) = 12$

$-b(di - fg) = -0 \cdot (\ldots) = 0$

$c(dh - eg) = 2 \cdot (0 \cdot 0 - 3 \cdot 1) = -6$

$\det = 12 + 0 - 6 = 6$ ∎
:::
:::

::: reference determinant
# Determinant

The determinant of a $2 \times 2$ matrix is the signed area of the parallelogram spanned by its
columns, so it is the factor by which the matrix scales areas. It is zero exactly when the
matrix is not full rank, and negative when the matrix reverses orientation.

::: equation
\det\begin{bmatrix} a & b \\ c & d \end{bmatrix} = ad - bc
:::

::: legend
$a, c$: the first column, the image of $\mathbf{e}_1$
$b, d$: the second column, the image of $\mathbf{e}_2$
:::

::: derivation
Take the base $\mathbf{u} = [a, c]^T$, of length $\sqrt{a^2 + c^2}$.
A unit vector perpendicular to it is $\mathbf{n} = [-c, a]^T / \sqrt{a^2 + c^2}$.
The height of $\mathbf{v} = [b, d]^T$ above the base is $|\mathbf{v} \cdot \mathbf{n}| = |ad - bc| / \sqrt{a^2 + c^2}$.[dot product](reference:dot-product)
Area is base times height: $\sqrt{a^2 + c^2} \cdot |ad - bc| / \sqrt{a^2 + c^2} = |ad - bc|$.
The sign of $ad - bc$ is the sign of $\mathbf{v} \cdot \mathbf{n}$: positive when $\mathbf{v}$ lies counterclockwise from $\mathbf{u}$. ∎
:::
:::

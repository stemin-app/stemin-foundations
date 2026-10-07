---
title: Eigenvalues and eigenvectors
---

::: card
Most vectors change direction when a matrix acts on them. A few only stretch. A nonzero
vector $\mathbf{v}$ is an **eigenvector** of $\mathbf{A}$, with **eigenvalue** $\lambda$, when

$$ \mathbf{A}\mathbf{v} = \lambda\mathbf{v} $$

The matrix leaves $\mathbf{v}$ on its own line and scales it by $\lambda$. These directions are
the natural axes of the map ([eigenvalue equation](reference:eigenvalue-equation)).
:::

::: card
Take $\mathbf{A} = \begin{bmatrix} 4 & 1 \\ 2 & 3 \end{bmatrix}$ and turn a unit vector
$\mathbf{x}$ around the circle. Its image $\mathbf{A}\mathbf{x}$ usually leaves the dashed line
of $\mathbf{x}$. At $45°$ it stays on the line, five times longer. At about $116.6°$, the
direction of $[-1, 2]^T$, it stays on the line again, twice as long.

```plot
x: { var: t, label: "$x₁$", from: -6, to: 6, ticks: 1, grid: true }
y: { label: "$x₂$", from: -5, to: 5 }

inputs:
  - { name: deg, min: 0, max: 180, default: 20, step: 0.5, label: "angle of x, in degrees" }

let:
  th: deg * pi() / 180
  ax: 4 * cos(th) + sin(th)
  ay: 2 * cos(th) + 3 * sin(th)

draw:
  - param: { var: s, over: [-6, 6], x: s * cos(th), y: s * sin(th), dash: true }
  - param: { var: s, over: [0, 1], x: s * cos(th), y: s * sin(th), label: "x" }
  - param: { var: s, over: [0, 1], x: s * ax, y: s * ay, accent: true }
  - point: { at: [ax, ay], label: "Ax", accent: true }
```
:::

::: card
To find the eigenvalues, move everything to one side: $(\mathbf{A} - \lambda\mathbf{I})\mathbf{v} = \mathbf{0}$.
A nonzero $\mathbf{v}$ goes to zero only if $\mathbf{A} - \lambda\mathbf{I}$ collapses space,
so its determinant must vanish:

$$ \det\begin{bmatrix} 4 - \lambda & 1 \\ 2 & 3 - \lambda \end{bmatrix} = (4 - \lambda)(3 - \lambda) - 2 = \lambda^2 - 7\lambda + 10 = 0 $$

It factors as $(\lambda - 5)(\lambda - 2) = 0$, so $\lambda_1 = 5$ and $\lambda_2 = 2$.
:::

::: card
For each eigenvalue, solve for the direction. With $\lambda_1 = 5$, the first row of
$\mathbf{A} - 5\mathbf{I}$ gives $-v_1 + v_2 = 0$, so $\mathbf{v}_1 = [1, 1]^T$. With
$\lambda_2 = 2$, it gives $2v_1 + v_2 = 0$, so $\mathbf{v}_2 = [1, -2]^T$. Check both:

$$ \mathbf{A}\begin{bmatrix} 1 \\ 1 \end{bmatrix} = \begin{bmatrix} 5 \\ 5 \end{bmatrix}, \qquad \mathbf{A}\begin{bmatrix} 1 \\ -2 \end{bmatrix} = \begin{bmatrix} 2 \\ -4 \end{bmatrix} $$
:::

::: card
In the basis of eigenvectors, the matrix only scales. Write $\mathbf{x} = c_1\mathbf{v}_1 + c_2\mathbf{v}_2$.
Linearity gives

$$ \mathbf{A}\mathbf{x} = c_1\lambda_1\mathbf{v}_1 + c_2\lambda_2\mathbf{v}_2 $$

For $\mathbf{x} = [3, 1]^T$, $c_1 = 7/3$ and $c_2 = 2/3$, so
$\mathbf{A}\mathbf{x} = \frac{35}{3}[1, 1]^T + \frac{4}{3}[1, -2]^T = [13, 9]^T$, the same as the
direct product.
:::

::: card
The gain shows on repeated products. $\mathbf{A}^{100}\mathbf{x} = c_1 5^{100}\mathbf{v}_1 + c_2 2^{100}\mathbf{v}_2$,
with no hundred matrix products. It also shows which direction wins: after many steps, the
largest eigenvalue dominates. A deep network multiplies by weight matrices again and again, so
eigenvalues above 1 make signals and gradients explode, and eigenvalues below 1 make them fade.
:::

::: card
Some matrices have no real eigenvector. A rotation by $\theta$,

$$ \mathbf{R} = \begin{bmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{bmatrix} $$

turns every direction, unless $\theta$ is $0$ or $\pi$. Its eigenvalues are the complex numbers
$\cos\theta \pm i\sin\theta$. Complex eigenvalues are the mark of a rotation.
:::

::: exercise q1
Find the eigenvalues of $\begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}$.

::: answer
3 and 1. Solve $(2 - \lambda)^2 - 1 = 0$.
:::

::: solution
$\det(\mathbf{A} - \lambda\mathbf{I}) = (2 - \lambda)^2 - 1$

$= \lambda^2 - 4\lambda + 3$

$= (\lambda - 3)(\lambda - 1) = 0$

$\lambda_1 = 3$, $\lambda_2 = 1$ ∎
:::
:::

::: exercise q2
Find an eigenvector of $\begin{bmatrix} 2 & 1 \\ 1 & 2 \end{bmatrix}$ for the eigenvalue 3.

::: answer
$[1, 1]^T$, or any nonzero multiple. The first row of $\mathbf{A} - 3\mathbf{I}$ gives
$-v_1 + v_2 = 0$.
:::
:::

::: exercise q3
$\mathbf{A}$ has eigenvector $\mathbf{v}$ with eigenvalue 0.5. What is $\mathbf{A}^3\mathbf{v}$?

::: answer
$0.125\,\mathbf{v}$. Each product scales $\mathbf{v}$ by 0.5.
:::
:::

::: exercise q4
What are the eigenvalues of $\mathrm{diag}(4, -1, 7)$?

::: answer
4, $-1$ and 7. Each $\mathbf{e}_i$ is an eigenvector, scaled by the diagonal entry.
:::
:::

::: reference eigenvalue-equation
# Eigenvalue equation

A nonzero vector $\mathbf{v}$ is an eigenvector of a square matrix $\mathbf{A}$, with eigenvalue
$\lambda$, when $\mathbf{A}$ only scales it. The eigenvalues are the roots of the characteristic
equation.

::: equation
\mathbf{A}\mathbf{v} = \lambda\mathbf{v} \qquad \det(\mathbf{A} - \lambda\mathbf{I}) = 0
:::

::: legend
$\mathbf{A}$: a square matrix
$\mathbf{v}$: an eigenvector, not zero
$\lambda$: its eigenvalue
$\mathbf{I}$: the identity matrix
:::

::: derivation
Rewrite $\mathbf{A}\mathbf{v} = \lambda\mathbf{v}$ as $(\mathbf{A} - \lambda\mathbf{I})\mathbf{v} = \mathbf{0}$.
If $\mathbf{A} - \lambda\mathbf{I}$ had an inverse, multiplying by it would give $\mathbf{v} = \mathbf{0}$.
So a nonzero $\mathbf{v}$ needs $\mathbf{A} - \lambda\mathbf{I}$ to have no inverse.
A square matrix has no inverse exactly when its determinant is zero.[determinant](reference:determinant)
So $\det(\mathbf{A} - \lambda\mathbf{I}) = 0$. ∎
:::
:::

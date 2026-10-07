---
title: Gradients of vector expressions
---

::: card
With vectors and matrices, keep track of shapes. For a scalar function $f: \mathbb{R}^n \to \mathbb{R}$
of $\mathbf{x} \in \mathbb{R}^n$, the gradient $\nabla_\mathbf{x} f$ is a column vector with $n$
entries, the same shape as $\mathbf{x}$. For a vector function
$\mathbf{f}: \mathbb{R}^n \to \mathbb{R}^m$, the Jacobian is $m \times n$. Below, $\mathbf{x}$ and
$\mathbf{a}$ are in $\mathbb{R}^n$ and $\mathbf{A}$ is in $\mathbb{R}^{n \times n}$.
:::

::: card
A linear function has a constant gradient:

$$ \nabla_\mathbf{x} (\mathbf{a}^T\mathbf{x}) = \mathbf{a} $$

The function $\mathbf{a}^T\mathbf{x} = a_1 x_1 + a_2 x_2 + \cdots$ grows at rate $a_i$ for each
unit of $x_i$, wherever you stand.
[Gradient of a linear function](reference:gradient-linear-function)
:::

::: card
Take $\mathbf{a} = [3, 2]^T$, so $f(\mathbf{x}) = 3x_1 + 2x_2$:

$$ \nabla f = \begin{bmatrix} \frac{\partial}{\partial x_1}(3x_1 + 2x_2) \\ \frac{\partial}{\partial x_2}(3x_1 + 2x_2) \end{bmatrix} = \begin{bmatrix} 3 \\ 2 \end{bmatrix} = \mathbf{a} $$
:::

::: card
The squared norm $\mathbf{x}^T\mathbf{x} = x_1^2 + x_2^2 + \cdots$ is a bowl centred at the
origin. Its gradient is twice the point:

$$ \nabla_\mathbf{x} (\mathbf{x}^T\mathbf{x}) = 2\mathbf{x} $$

At $\mathbf{x} = [3, 4]^T$, $f = 9 + 16 = 25$ and $\nabla f = [6, 8]^T$: the same gradient you
found for $x^2 + y^2$ at $(3, 4)$.
[Gradient of the squared norm](reference:gradient-squared-norm)
:::

::: card
A quadratic form $\mathbf{x}^T\mathbf{A}\mathbf{x}$ has the gradient

$$ \nabla_\mathbf{x} (\mathbf{x}^T\mathbf{A}\mathbf{x}) = (\mathbf{A} + \mathbf{A}^T)\mathbf{x} $$

If $\mathbf{A}$ is symmetric, $\mathbf{A} = \mathbf{A}^T$, and this is $2\mathbf{A}\mathbf{x}$.
With $\mathbf{A} = \mathbf{I}$ it gives back $2\mathbf{x}$.
[Gradient of a quadratic form](reference:gradient-quadratic-form)
:::

::: card
Take $\mathbf{A} = \begin{bmatrix} 1 & 2 \\ 0 & 3 \end{bmatrix}$ and $\mathbf{x} = [1, 1]^T$.
Then $\mathbf{A}\mathbf{x} = [3, 3]^T$ and $f = [1, 1] \cdot [3, 3] = 6$. The gradient:

$$ \mathbf{A} + \mathbf{A}^T = \begin{bmatrix} 2 & 2 \\ 2 & 6 \end{bmatrix}, \qquad \nabla f = \begin{bmatrix} 2 & 2 \\ 2 & 6 \end{bmatrix}\begin{bmatrix} 1 \\ 1 \end{bmatrix} = \begin{bmatrix} 4 \\ 8 \end{bmatrix} $$
:::

::: card
Check by expanding. $f = x_1(x_1 + 2x_2) + x_2(3x_2) = x_1^2 + 2x_1 x_2 + 3x_2^2$, so

$$ \frac{\partial f}{\partial x_1} = 2x_1 + 2x_2 = 4, \qquad \frac{\partial f}{\partial x_2} = 2x_1 + 6x_2 = 8 $$

The $2$ above the diagonal of $\mathbf{A}$ feeds both partials, which is why
$\mathbf{A}^T$ appears.
:::

::: card
The norm itself, not squared, has a gradient of length one:

$$ \nabla_\mathbf{x} \|\mathbf{x}\|_2 = \frac{\mathbf{x}}{\|\mathbf{x}\|_2} $$

At $\mathbf{x} = [3, 4]^T$, $\|\mathbf{x}\|_2 = 5$ and the gradient is $[0.6, 0.8]^T$, a unit
vector pointing away from the origin. Step straight outward and the norm grows at rate 1.
[Gradient of the norm](reference:gradient-euclidean-norm)
:::

::: card
Cut both functions along one axis. The squared norm becomes $t^2$, whose slope $2t$ grows as you
move out. The norm becomes $|t|$, whose slope stays 1. At the origin $|t|$ has a corner, and
$\frac{\mathbf{x}}{\|\mathbf{x}\|_2}$ divides by zero: the norm has no gradient at
$\mathbf{x} = \mathbf{0}$.

```plot
x: { var: t, label: "$t$, the distance along one axis", from: -3, to: 3, ticks: 1, grid: true }
y: { label: "$f$", from: -1, to: 9 }

inputs:
  - { name: t0, min: 0.1, max: 2.9, default: 1.5, step: 0.2, label: "the point t₀" }

draw:
  - curve: { is: t^2, label: "$xᵀ x$" }
  - curve: { is: abs(t), dash: true, label: "$‖x‖$" }
  - curve: { is: t0^2 + 2 * t0 * (t - t0), over: [t0 - 1, t0 + 1], accent: true, label: "slope $2t₀$" }
  - point: { at: [t0, t0^2] }
  - point: { at: [t0, t0] }
  - point: { at: [0, 0], label: "corner of $‖x‖$" }
```
:::

::: exercise matrix-calculus-linear
For $f(\mathbf{x}) = \mathbf{a}^T\mathbf{x}$ with $\mathbf{a} = [1, -2, 5]^T$, what is $\nabla f$
at $\mathbf{x} = [7, 0, 2]^T$?

::: answer
$[1, -2, 5]^T$. A linear function's gradient is $\mathbf{a}$ at every point.
:::
:::

::: exercise matrix-calculus-symmetric
For $\mathbf{A} = \begin{bmatrix} 2 & 1 \\ 1 & 3 \end{bmatrix}$ and $\mathbf{x} = [1, 2]^T$, what is
$\nabla_\mathbf{x} (\mathbf{x}^T\mathbf{A}\mathbf{x})$?

::: answer
$[8, 14]^T$. The matrix is symmetric, so the gradient is $2\mathbf{A}\mathbf{x}$.
:::

::: solution
$$ \mathbf{A}\mathbf{x} = \begin{bmatrix} 2 + 2 \\ 1 + 6 \end{bmatrix} = \begin{bmatrix} 4 \\ 7 \end{bmatrix} $$

$$ 2\mathbf{A}\mathbf{x} = \begin{bmatrix} 8 \\ 14 \end{bmatrix} $$

∎
:::
:::

::: exercise matrix-calculus-norm
What is $\nabla_\mathbf{x} \|\mathbf{x}\|_2$ at $\mathbf{x} = [5, 12]^T$?

::: answer
$[\frac{5}{13}, \frac{12}{13}]^T$. The norm is 13.
:::

::: solution
$$ \|\mathbf{x}\|_2 = \sqrt{25 + 144} = 13 $$

$$ \frac{\mathbf{x}}{\|\mathbf{x}\|_2} = \begin{bmatrix} 5/13 \\ 12/13 \end{bmatrix} $$

∎
:::
:::

::: exercise matrix-calculus-squared-norm
What is $\nabla_\mathbf{x} (\mathbf{x}^T\mathbf{x})$ at $\mathbf{x} = [1, -2, 2]^T$?

::: answer
$[2, -4, 4]^T$. The gradient is $2\mathbf{x}$.
:::
:::

::: reference gradient-linear-function
# Gradient of a linear function

The gradient of $\mathbf{a}^T\mathbf{x}$ is the constant vector $\mathbf{a}$.

::: equation
\nabla_\mathbf{x} (\mathbf{a}^T\mathbf{x}) = \mathbf{a}
:::

::: legend
$\mathbf{x}$: the variable vector, n entries
$\mathbf{a}$: a constant vector, n entries
:::

::: derivation
Write the product out: $\mathbf{a}^T\mathbf{x} = \sum_j a_j x_j$.[Dot product](reference:dot-product)
Differentiate by $x_i$: every term but $a_i x_i$ is constant, so $\frac{\partial}{\partial x_i} = a_i$.[Partial derivative](reference:partial-derivative)
Stack the entries: $\nabla_\mathbf{x} (\mathbf{a}^T\mathbf{x}) = \mathbf{a}$. ∎
:::
:::

::: reference gradient-squared-norm
# Gradient of the squared norm

The gradient of $\mathbf{x}^T\mathbf{x}$ is $2\mathbf{x}$.

::: equation
\nabla_\mathbf{x} (\mathbf{x}^T\mathbf{x}) = 2\mathbf{x}
:::

::: legend
$\mathbf{x}$: the variable vector, n entries
:::

::: derivation
Write it out: $\mathbf{x}^T\mathbf{x} = \sum_j x_j^2$.
Differentiate by $x_i$: only $x_i^2$ depends on it, so $\frac{\partial}{\partial x_i} = 2x_i$.[Partial derivative](reference:partial-derivative)
Stack the entries: $2\mathbf{x}$. ∎
:::
:::

::: reference gradient-quadratic-form
# Gradient of a quadratic form

The gradient of $\mathbf{x}^T\mathbf{A}\mathbf{x}$ is $(\mathbf{A} + \mathbf{A}^T)\mathbf{x}$, which
is $2\mathbf{A}\mathbf{x}$ when $\mathbf{A}$ is symmetric.

::: equation
\nabla_\mathbf{x} (\mathbf{x}^T\mathbf{A}\mathbf{x}) = (\mathbf{A} + \mathbf{A}^T)\mathbf{x}
:::

::: legend
$\mathbf{x}$: the variable vector, n entries
$\mathbf{A}$: a constant square matrix, n by n
:::

::: derivation
Write it out: $\mathbf{x}^T\mathbf{A}\mathbf{x} = \sum_j \sum_k A_{jk} x_j x_k$.
Differentiate by $x_i$. The terms with $j = i$ give $\sum_k A_{ik} x_k$; the terms with $k = i$ give $\sum_j A_{ji} x_j$.[Product rule](reference:product-rule)
The first sum is entry $i$ of $\mathbf{A}\mathbf{x}$, and the second is entry $i$ of $\mathbf{A}^T\mathbf{x}$.[Matrix-vector product](reference:matrix-vector-product)
Stack the entries: $(\mathbf{A} + \mathbf{A}^T)\mathbf{x}$. ∎
:::
:::

::: reference gradient-euclidean-norm
# Gradient of the norm

Away from the origin, the gradient of the Euclidean norm is the unit vector along $\mathbf{x}$. At
$\mathbf{x} = \mathbf{0}$ the norm has no gradient.

::: equation
\nabla_\mathbf{x} \|\mathbf{x}\|_2 = \frac{\mathbf{x}}{\|\mathbf{x}\|_2} \qquad \mathbf{x} \neq \mathbf{0}
:::

::: legend
$\mathbf{x}$: the variable vector, n entries, not zero
$\|\mathbf{x}\|_2$: its Euclidean length
:::

::: derivation
Write the norm as a square root: $\|\mathbf{x}\|_2 = (\mathbf{x}^T\mathbf{x})^{1/2}$.[Vector norm](reference:vector-norm)
Apply the chain rule with $u = \mathbf{x}^T\mathbf{x}$: $\nabla \|\mathbf{x}\|_2 = \frac{1}{2} u^{-1/2} \nabla u$.[Chain rule](reference:chain-rule)
Use $\nabla u = 2\mathbf{x}$.[Gradient of the squared norm](reference:gradient-squared-norm)
So $\nabla \|\mathbf{x}\|_2 = \frac{1}{2} \cdot \frac{2\mathbf{x}}{\|\mathbf{x}\|_2} = \frac{\mathbf{x}}{\|\mathbf{x}\|_2}$. ∎
:::
:::

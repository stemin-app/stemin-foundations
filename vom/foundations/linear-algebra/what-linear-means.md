---
title: What linear means
---

::: card
The word *linear* comes back again and again: linear combinations, linear independence, linear
maps. It always names the same idea. A function $f$ is **linear** when it respects scaling and
addition:

$$ f(a\mathbf{v}) = a f(\mathbf{v}) \qquad f(\mathbf{u} + \mathbf{v}) = f(\mathbf{u}) + f(\mathbf{v}) $$

for every scalar $a$ and all vectors $\mathbf{u}$, $\mathbf{v}$.
:::

::: card
The two rules fold into one:

$$ f(a\mathbf{u} + b\mathbf{v}) = a f(\mathbf{u}) + b f(\mathbf{v}) $$

Double the input and the output doubles. Add two inputs and you may process each one alone, then
add the results. Nothing interacts, and that is what makes a linear map easy to analyse
([linear map](reference:linear-map)). A transformer is not purely linear, but its linear parts
come first.
:::

::: card
A linear map always sends zero to zero. Write the zero vector as $0 \cdot \mathbf{v}$ and use the
scaling rule:

$$ f(\mathbf{0}) = f(0 \cdot \mathbf{v}) = 0 \cdot f(\mathbf{v}) = \mathbf{0} $$

This gives a quick test. A map that sends zero anywhere else is not linear.
:::

::: card
Squaring breaks the scaling rule. With $f(x) = x^2$:

$$ f(2x) = (2x)^2 = 4x^2 \qquad 2f(x) = 2x^2 $$

The squaring creates an extra factor of 2, so $f(2x) \neq 2f(x)$ for every $x \neq 0$.
:::

::: card
A shift breaks linearity too. The line $f(x) = kx + b$ looks linear, but $f(0) = b$, and the
zero test fails unless $b = 0$. Below, compare $f(2)$ with $2f(1)$. Set $b$ to 0 and the two
points meet. Any other $b$ pulls them apart by exactly $b$.

```plot
x: { var: x, label: "$x$", from: 0, to: 3, ticks: 1, grid: true }
y: { label: "$f(x)$", from: -2, to: 8 }

inputs:
  - { name: k, min: 0, max: 2, default: 1.5, step: 0.1, label: "slope k" }
  - { name: b, min: -1, max: 2, default: 1, step: 0.1, label: "shift b" }

draw:
  - curve: { is: k * x + b }
  - vline: { at: 2, dash: true }
  - point: { at: [2, 2 * k + b], label: "f(2)" }
  - point: { at: [2, 2 * (k + b)], label: "2 f(1)", accent: true }
```
:::

::: card
A **linear combination** of vectors $\mathbf{v}_1, \ldots, \mathbf{v}_k$ mixes them with scalar
weights $c_1, \ldots, c_k$:

$$ c_1\mathbf{v}_1 + c_2\mathbf{v}_2 + \cdots + c_k\mathbf{v}_k $$

The result depends linearly on the weights. Double every $c_i$ and the result doubles. Add two
linear combinations of the same vectors and you get a third one.
:::

::: exercise q1
Is $f\left(\begin{bmatrix} v_1 \\ v_2 \end{bmatrix}\right) = \begin{bmatrix} v_1 + v_2 \\ 2v_1 \end{bmatrix}$
linear?

::: answer
Yes. Each output component is a weighted sum of the inputs with no constant term.
:::

::: solution
Take $\mathbf{w} = a\mathbf{u} + b\mathbf{v}$, so $w_1 = au_1 + bv_1$ and $w_2 = au_2 + bv_2$.

First component: $w_1 + w_2 = a(u_1 + u_2) + b(v_1 + v_2)$.

Second component: $2w_1 = a(2u_1) + b(2v_1)$.

Together: $f(a\mathbf{u} + b\mathbf{v}) = a f(\mathbf{u}) + b f(\mathbf{v})$. ∎
:::
:::

::: exercise q2
Is $g\left(\begin{bmatrix} v_1 \\ v_2 \end{bmatrix}\right) = \begin{bmatrix} v_1 v_2 \\ v_1 \end{bmatrix}$
linear? Test it with $\mathbf{v} = \begin{bmatrix} 1 \\ 1 \end{bmatrix}$ and $a = 2$.

::: answer
No. $g(2\mathbf{v}) = \begin{bmatrix} 4 \\ 2 \end{bmatrix}$, but $2g(\mathbf{v}) = \begin{bmatrix} 2 \\ 2 \end{bmatrix}$.
:::

::: solution
$$ g(2\mathbf{v}) = g\left(\begin{bmatrix} 2 \\ 2 \end{bmatrix}\right) = \begin{bmatrix} 4 \\ 2 \end{bmatrix} $$

$$ 2g(\mathbf{v}) = 2\begin{bmatrix} 1 \\ 1 \end{bmatrix} = \begin{bmatrix} 2 \\ 2 \end{bmatrix} $$

The first components differ, so the scaling rule fails. The product $v_1 v_2$ is the culprit. ∎
:::
:::

::: exercise q3
Compute the linear combination $2\begin{bmatrix} 1 \\ 0 \\ 1 \end{bmatrix} - 3\begin{bmatrix} 0 \\ 1 \\ 2 \end{bmatrix}$.

::: answer
$\begin{bmatrix} 2 \\ -3 \\ -4 \end{bmatrix}$. Scale each vector, then add component by
component.
:::

::: solution
$$ \begin{bmatrix} 2 \\ 0 \\ 2 \end{bmatrix} + \begin{bmatrix} 0 \\ -3 \\ -6 \end{bmatrix} = \begin{bmatrix} 2 \\ -3 \\ -4 \end{bmatrix} $$

∎
:::
:::

::: reference linear-map
# Linear map

A map $f$ between vector spaces is linear when it carries every linear combination of inputs to
the same linear combination of outputs. A linear map sends the zero vector to the zero vector.

::: equation
f(a\mathbf{u} + b\mathbf{v}) = a f(\mathbf{u}) + b f(\mathbf{v}) \qquad f(\mathbf{0}) = \mathbf{0}
:::

::: legend
$f$: the map
$\mathbf{u}, \mathbf{v}$: any input vectors
$a, b$: any real scalars
$\mathbf{0}$: the zero vector
:::

::: derivation
Set $b = 0$: $f(a\mathbf{u}) = a f(\mathbf{u})$, the scaling rule.
Set $a = b = 1$: $f(\mathbf{u} + \mathbf{v}) = f(\mathbf{u}) + f(\mathbf{v})$, the addition rule.
Conversely, the two rules give $f(a\mathbf{u} + b\mathbf{v}) = f(a\mathbf{u}) + f(b\mathbf{v}) = a f(\mathbf{u}) + b f(\mathbf{v})$.
For zero, write $\mathbf{0} = 0 \cdot \mathbf{u}$: $f(\mathbf{0}) = 0 \cdot f(\mathbf{u}) = \mathbf{0}$. ∎
:::
:::

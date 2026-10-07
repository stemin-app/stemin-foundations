---
title: The dot product
---

::: card
The **dot product** of two vectors $\mathbf{u}, \mathbf{v} \in \mathbb{R}^n$ multiplies them
component by component and adds the results:

$$ \mathbf{u} \cdot \mathbf{v} = \mathbf{u}^T\mathbf{v} = \sum_{i=1}^n u_i v_i = u_1v_1 + u_2v_2 + \cdots + u_nv_n $$

The answer is one number. You will also see it written $\langle \mathbf{u}, \mathbf{v} \rangle$.
For $\begin{bmatrix} 1 & 2 & 3 \end{bmatrix}^T$ and $\begin{bmatrix} 4 & -5 & 6 \end{bmatrix}^T$
it is $4 - 10 + 18 = 12$.
:::

::: card
The dot product of a vector with itself gives its squared length. The **Euclidean norm** of
$\mathbf{v}$ is

$$ \|\mathbf{v}\| = \sqrt{\mathbf{v} \cdot \mathbf{v}} = \sqrt{v_1^2 + v_2^2 + \cdots + v_n^2} $$

This is Pythagoras in $n$ dimensions. The vector $\begin{bmatrix} 3 \\ 4 \end{bmatrix}$ has length
$\sqrt{9 + 16} = 5$.
:::

::: card
The same number has a geometric meaning. If $\theta$ is the angle between $\mathbf{u}$ and
$\mathbf{v}$, then

$$ \mathbf{u} \cdot \mathbf{v} = \|\mathbf{u}\|\,\|\mathbf{v}\|\cos\theta $$

The law of cosines proves it ([dot product](reference:dot-product)). So the dot product
measures how far two vectors point the same way.
:::

::: card
Drop a perpendicular from the tip of $\mathbf{u}$ onto the line of $\mathbf{v}$. The foot sits at
a signed distance $\|\mathbf{u}\|\cos\theta$ from the origin: the shadow of $\mathbf{u}$ on
$\mathbf{v}$. The dot product is that shadow times $\|\mathbf{v}\|$. Here $\|\mathbf{u}\| = 2.5$
and $\|\mathbf{v}\| = 2$. Turn the vectors and watch the shadow shrink to zero at a right angle,
then fall behind the origin.

```plot
x: { var: t, label: "$x₁$", from: -5, to: 5, ticks: 1, grid: true }
y: { label: "$x₂$", from: -3, to: 3 }

inputs:
  - { name: alpha, min: 0, max: 360, default: 60, step: 5, label: "angle of u, in degrees" }
  - { name: beta, min: 0, max: 360, default: 10, step: 5, label: "angle of v, in degrees" }

let:
  au: alpha * pi() / 180
  bv: beta * pi() / 180
  p: 2.5 * cos(au - bv)

draw:
  - param: { var: s, over: [-3, 3], x: s * cos(bv), y: s * sin(bv), dash: true }
  - param: { var: s, over: [0, 1], x: s * 2.5 * cos(au), y: s * 2.5 * sin(au), label: "u" }
  - param: { var: s, over: [0, 1], x: s * 2 * cos(bv), y: s * 2 * sin(bv), accent: true, label: "v" }
  - param: { var: s, over: [0, 1], x: 2.5 * cos(au) + s * (p * cos(bv) - 2.5 * cos(au)), y: 2.5 * sin(au) + s * (p * sin(bv) - 2.5 * sin(au)), dash: true }
  - point: { at: [p * cos(bv), p * sin(bv)], label: "shadow" }
```
:::

::: card
Three angles mark the range. At $\theta = 0$ the vectors are parallel, $\cos\theta = 1$, and the
dot product takes its largest value $\|\mathbf{u}\|\|\mathbf{v}\|$. At $90°$ they are
**orthogonal** and the dot product is 0. At $180°$ they point opposite ways, $\cos\theta = -1$,
and the dot product is $-\|\mathbf{u}\|\|\mathbf{v}\|$, its smallest value.

```plot
x: { var: th, label: "$θ$ in degrees", from: 0, to: 220, ticks: 30, grid: true }
y: { label: "dot product", from: -10, to: 10 }

inputs:
  - { name: nu, min: 0.5, max: 3, default: 2.5, step: 0.1, label: "length of u" }
  - { name: nv, min: 0.5, max: 3, default: 2, step: 0.1, label: "length of v" }

draw:
  - hline: { at: 0 }
  - curve: { is: nu * nv * cos(th * pi() / 180), over: [0, 180], accent: true }
  - point: { at: [0, nu * nv], label: "parallel" }
  - point: { at: [90, 0], label: "orthogonal" }
  - point: { at: [180, -nu * nv], label: "opposite" }
```
:::

::: card
A transformer computes dot products all the time to measure similarity. A large positive dot
product means two vectors point in similar directions. Near zero, they are unrelated. A large
negative value means they point in opposite directions. This is how the model decides which pieces
of information belong together.
:::

::: card
The dot product also grows with length, which can hide the direction. Divide out both lengths and
only the angle is left. This is the **cosine similarity**:

$$ \cos\theta = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\|\,\|\mathbf{v}\|} $$

It runs from $-1$ to $1$ ([cosine similarity](reference:cosine-similarity)). The vectors
$\begin{bmatrix} 1 & 2 & 3 \end{bmatrix}^T$ and $\begin{bmatrix} 2 & 4 & 6 \end{bmatrix}^T$
have cosine similarity 1: one is twice the other.
:::

::: exercise q1
Compute $\begin{bmatrix} 2 \\ -1 \\ 3 \end{bmatrix} \cdot \begin{bmatrix} 4 \\ 0 \\ -2 \end{bmatrix}$.

::: answer
2. Multiply matching components and add: $8 + 0 - 6$.
:::
:::

::: exercise q2
What is the angle between $\begin{bmatrix} 1 \\ 0 \end{bmatrix}$ and $\begin{bmatrix} 1 \\ 1 \end{bmatrix}$?

::: answer
$45°$. The cosine is $1/\sqrt{2}$.
:::

::: solution
$$ \mathbf{u} \cdot \mathbf{v} = 1 \cdot 1 + 0 \cdot 1 = 1 $$

$$ \|\mathbf{u}\| = 1, \quad \|\mathbf{v}\| = \sqrt{1 + 1} = \sqrt{2} $$

$$ \cos\theta = \frac{1}{1 \cdot \sqrt{2}} = \frac{1}{\sqrt{2}} \implies \theta = 45° $$

∎
:::
:::

::: exercise q3
Find the cosine similarity of $\begin{bmatrix} 1 \\ 2 \\ 2 \end{bmatrix}$ and
$\begin{bmatrix} 2 \\ 1 \\ -2 \end{bmatrix}$.

::: answer
0. The dot product is $2 + 2 - 4 = 0$, so the vectors are orthogonal.
:::

::: solution
$$ \mathbf{u} \cdot \mathbf{v} = 1 \cdot 2 + 2 \cdot 1 + 2 \cdot (-2) = 0 $$

Both lengths are $\sqrt{1 + 4 + 4} = 3$.

$$ \cos\theta = \frac{0}{3 \cdot 3} = 0 $$

∎
:::
:::

::: exercise q4
For which $k$ is $\begin{bmatrix} 3 \\ k \end{bmatrix}$ orthogonal to $\begin{bmatrix} 2 \\ 6 \end{bmatrix}$?

::: answer
$k = -1$. Set the dot product $6 + 6k$ to zero.
:::
:::

::: reference dot-product
# Dot product

The dot product of two vectors of $\mathbb{R}^n$ is the sum of the products of their matching
components. It equals the product of their lengths and the cosine of the angle between them.

::: equation
\mathbf{u} \cdot \mathbf{v} = \mathbf{u}^T\mathbf{v} = \sum_{i=1}^n u_i v_i = \|\mathbf{u}\|\,\|\mathbf{v}\|\cos\theta
:::

::: legend
$\mathbf{u}, \mathbf{v}$: vectors of $\mathbb{R}^n$
$u_i, v_i$: their components
$\|\mathbf{u}\|$: the Euclidean length, $\sqrt{\mathbf{u} \cdot \mathbf{u}}$
$\theta$: the angle between the two vectors
:::

::: derivation
Take the triangle with sides $\mathbf{u}$, $\mathbf{v}$ and $\mathbf{u} - \mathbf{v}$.
The law of cosines: $\|\mathbf{u} - \mathbf{v}\|^2 = \|\mathbf{u}\|^2 + \|\mathbf{v}\|^2 - 2\|\mathbf{u}\|\|\mathbf{v}\|\cos\theta$.
Expand the left side with the dot product: $(\mathbf{u} - \mathbf{v}) \cdot (\mathbf{u} - \mathbf{v}) = \|\mathbf{u}\|^2 - 2\,\mathbf{u} \cdot \mathbf{v} + \|\mathbf{v}\|^2$.
Equate the two: $-2\,\mathbf{u} \cdot \mathbf{v} = -2\|\mathbf{u}\|\|\mathbf{v}\|\cos\theta$.
Divide by $-2$: $\mathbf{u} \cdot \mathbf{v} = \|\mathbf{u}\|\|\mathbf{v}\|\cos\theta$. ∎
:::
:::

::: reference cosine-similarity
# Cosine similarity

The cosine similarity of two nonzero vectors is the cosine of the angle between them. It ignores
their lengths and runs from $-1$ (opposite) through $0$ (orthogonal) to $1$ (same direction).

::: equation
\cos\theta = \frac{\mathbf{u} \cdot \mathbf{v}}{\|\mathbf{u}\|\,\|\mathbf{v}\|}
:::

::: legend
$\mathbf{u}, \mathbf{v}$: nonzero vectors of $\mathbb{R}^n$
$\theta$: the angle between them
$\|\mathbf{u}\|$: the Euclidean length of $\mathbf{u}$
:::

::: derivation
Start from $\mathbf{u} \cdot \mathbf{v} = \|\mathbf{u}\|\|\mathbf{v}\|\cos\theta$.[dot product](reference:dot-product)
Both lengths are nonzero, so divide by their product.
Since $-1 \le \cos\theta \le 1$, the value stays in $[-1, 1]$.
Scale $\mathbf{u}$ by $a > 0$: numerator and denominator both gain the factor $a$, so the value does not change. ∎
:::
:::

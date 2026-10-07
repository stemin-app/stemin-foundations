---
title: Vectors and the space they live in
---

::: card
A **vector** is an ordered list of numbers. Write it as a column, and name it with a bold
lowercase letter:

$$ \mathbf{v} = \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix} $$

The number $v_i$ in place $i$ is a **component**. You may also write it $[\mathbf{v}]_i$.
:::

::: card
The letters carry their kind, so you can read a line of mathematics before you parse it. A
scalar, one plain number, is a lowercase italic letter: $x$, $n$, $\alpha$, $\beta$. A vector is
a lowercase bold letter, $\mathbf{x}$, $\mathbf{v}$, $\mathbf{w}$, and is a column unless it is
transposed. A matrix is an uppercase bold letter, $\mathbf{A}$, $\mathbf{W}$. A set is an
uppercase calligraphic letter, $\mathcal{D}$ or $\mathcal{V}$.
:::

::: card
The real numbers are $\mathbb{R}$. A vector of $n$ real components belongs to
$\mathbb{R}^n$, the set of all such vectors, so $\mathbf{v} \in \mathbb{R}^3$ says that
$\mathbf{v}$ holds three real numbers. Two more sets appear later: the natural numbers
$\mathbb{N} = \{0, 1, 2, \ldots\}$ and the integers $\mathbb{Z}$.
:::

::: card
Add two vectors component by component. In the plane, draw $\mathbf{v}$ starting from the tip
of $\mathbf{u}$: the sum $\mathbf{u} + \mathbf{v}$ runs from the origin to where $\mathbf{v}$
ends. Draw $\mathbf{u}$ after $\mathbf{v}$ instead and you land on the same point.

```plot
x: { var: t, label: "$x₁$", from: -5, to: 5, ticks: 1, grid: true }
y: { label: "$x₂$", from: -3, to: 3 }

inputs:
  - { name: u1, min: -2.5, max: 2.5, default: 2, step: 0.1, label: "first component of u" }
  - { name: u2, min: -1.5, max: 1.5, default: 0.5, step: 0.1, label: "second component of u" }
  - { name: v1, min: -2.5, max: 2.5, default: 0.5, step: 0.1, label: "first component of v" }
  - { name: v2, min: -1.5, max: 1.5, default: 1.5, step: 0.1, label: "second component of v" }

draw:
  - param: { var: s, over: [0, 1], x: u1 + s * v1, y: u2 + s * v2, dash: true }
  - param: { var: s, over: [0, 1], x: v1 + s * u1, y: v2 + s * u2, dash: true }
  - param: { var: s, over: [0, 1], x: s * u1, y: s * u2, label: "u" }
  - param: { var: s, over: [0, 1], x: s * v1, y: s * v2, label: "v" }
  - param: { var: s, over: [0, 1], x: s * (u1 + v1), y: s * (u2 + v2), accent: true }
  - point: { at: [u1 + v1, u2 + v2], label: "u + v", accent: true }
```
:::

::: card
Multiply a vector by a scalar $a$ and every component is multiplied by $a$. The vector keeps its
line through the origin. A factor above 1 stretches it, a factor between 0 and 1 shrinks it, and
a negative factor turns it to point the other way.

```plot
x: { var: t, label: "$x₁$", from: -5, to: 5, ticks: 1, grid: true }
y: { label: "$x₂$", from: -3, to: 3 }

inputs:
  - { name: a, min: -2, max: 2, default: 1.5, step: 0.1, label: "the scalar a" }

draw:
  - param: { var: s, over: [0, 1], x: s * 2 * a, y: s * a, accent: true }
  - param: { var: s, over: [0, 1], x: 2 * s, y: s, dash: true, label: "v" }
  - point: { at: [2 * a, a], label: "av", accent: true }
```
:::

::: card
A **vector space** is a set of vectors in which these two operations never lead out of the set.
For any $\mathbf{u}, \mathbf{v}$ in it and any scalar $a$, the sum $\mathbf{u} + \mathbf{v}$
and the multiple $a\mathbf{v}$ are in it too. The space $\mathbb{R}^n$ is the one you meet
everywhere in this course.
:::

::: card
The remaining rules make the algebra behave. For vectors $\mathbf{u}, \mathbf{v}, \mathbf{w}$ and
scalars $a, b$:

$$
\begin{aligned}
(\mathbf{u} + \mathbf{v}) + \mathbf{w} &= \mathbf{u} + (\mathbf{v} + \mathbf{w}) \\
\mathbf{u} + \mathbf{v} &= \mathbf{v} + \mathbf{u} \\
\mathbf{v} + \mathbf{0} = \mathbf{v}, \quad \mathbf{v} + (-\mathbf{v}) &= \mathbf{0} \\
a(\mathbf{u} + \mathbf{v}) &= a\mathbf{u} + a\mathbf{v} \\
(a + b)\mathbf{v} &= a\mathbf{v} + b\mathbf{v} \\
a(b\mathbf{v}) = (ab)\mathbf{v}, \quad 1\mathbf{v} &= \mathbf{v}
\end{aligned}
$$

The full list is in [the vector space rules](reference:vector-space-axioms).
:::

::: card
These rules are why you can move terms across an equals sign and regroup sums without a second
thought. A transformer adds vectors all the time to combine information, and scales them to
adjust their size. The rules guarantee that both operations behave the same way every time.
:::

::: exercise q1
Let $\mathbf{u} = \begin{bmatrix} 1 \\ 3 \end{bmatrix}$ and
$\mathbf{v} = \begin{bmatrix} 4 \\ -1 \end{bmatrix}$. Compute $2\mathbf{u} - \mathbf{v}$.

::: answer
$\begin{bmatrix} -2 \\ 7 \end{bmatrix}$. Scale each component of $\mathbf{u}$ by 2, then
subtract $\mathbf{v}$ component by component.
:::

::: solution
$$ 2\mathbf{u} = \begin{bmatrix} 2 \\ 6 \end{bmatrix} $$

$$ 2\mathbf{u} - \mathbf{v} = \begin{bmatrix} 2 - 4 \\ 6 - (-1) \end{bmatrix} = \begin{bmatrix} -2 \\ 7 \end{bmatrix} $$

∎
:::
:::

::: exercise q2
Take the set of all vectors in $\mathbb{R}^2$ whose second component is 1. Is it a vector space?

::: answer
No. The sum of two of its vectors has second component 2, so addition leads out of the set.
:::

::: solution
Take $\begin{bmatrix} 0 \\ 1 \end{bmatrix}$ and $\begin{bmatrix} 3 \\ 1 \end{bmatrix}$, both in
the set.

Their sum is $\begin{bmatrix} 3 \\ 2 \end{bmatrix}$, whose second component is 2.

The sum is not in the set, so the set is not closed under addition. ∎
:::
:::

::: exercise q3
For $\mathbf{v} = \begin{bmatrix} 4 & 0 & -2 & 7 \end{bmatrix}^T$, what is $[\mathbf{v}]_3$, and
which set does $\mathbf{v}$ belong to?

::: answer
$[\mathbf{v}]_3 = -2$, and $\mathbf{v} \in \mathbb{R}^4$. Count components from 1.
:::
:::

::: reference vector-space-axioms
# The vector space rules

A set $\mathcal{V}$ of vectors, with addition and multiplication by a scalar, is a vector space
when both operations stay inside $\mathcal{V}$ and obey these rules for all
$\mathbf{u}, \mathbf{v}, \mathbf{w} \in \mathcal{V}$ and all scalars $a, b$.

::: equation
\begin{aligned}
&\mathbf{u} + \mathbf{v} \in \mathcal{V}, \quad a\mathbf{v} \in \mathcal{V} \\
&(\mathbf{u} + \mathbf{v}) + \mathbf{w} = \mathbf{u} + (\mathbf{v} + \mathbf{w}), \quad \mathbf{u} + \mathbf{v} = \mathbf{v} + \mathbf{u} \\
&\mathbf{v} + \mathbf{0} = \mathbf{v}, \quad \mathbf{v} + (-\mathbf{v}) = \mathbf{0} \\
&a(\mathbf{u} + \mathbf{v}) = a\mathbf{u} + a\mathbf{v}, \quad (a + b)\mathbf{v} = a\mathbf{v} + b\mathbf{v} \\
&a(b\mathbf{v}) = (ab)\mathbf{v}, \quad 1\mathbf{v} = \mathbf{v}
\end{aligned}
:::

::: legend
$\mathcal{V}$: the set of vectors
$\mathbf{u}, \mathbf{v}, \mathbf{w}$: any vectors of the set
$a, b$: any real scalars
$\mathbf{0}$: the zero vector
$-\mathbf{v}$: the opposite of $\mathbf{v}$
:::
:::

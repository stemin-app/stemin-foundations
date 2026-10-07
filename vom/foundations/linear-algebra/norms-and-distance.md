---
title: Norms and distance
---

::: card
A **norm** gives every vector a size, a number $\|\mathbf{v}\| \ge 0$. To be a norm, the
function must obey three rules:

$$ \|\mathbf{v}\| = 0 \iff \mathbf{v} = \mathbf{0}, \qquad \|a\mathbf{v}\| = |a|\,\|\mathbf{v}\|, \qquad \|\mathbf{u} + \mathbf{v}\| \le \|\mathbf{u}\| + \|\mathbf{v}\| $$

Only the zero vector has size zero. Scale a vector by $a$ and its size scales by $|a|$. The
direct path to $\mathbf{u} + \mathbf{v}$ is never longer than the detour through $\mathbf{u}$.
:::

::: card
Two plausible sizes break the rules. $f(\mathbf{v}) = \|\mathbf{v}\|_2 + 1$ fails the scaling
rule: $f(2\mathbf{v}) = 2\|\mathbf{v}\|_2 + 1$, but $2f(\mathbf{v}) = 2\|\mathbf{v}\|_2 + 2$.
$g(\mathbf{v}) = \sum_i v_i$ fails the first rule: for $\mathbf{v} = [1, -1]^T$ it gives 0,
and $\mathbf{v}$ is not zero.
:::

::: card
Without the triangle rule, a detour could be shorter than the direct route. Walk from A to B in
10 minutes, or from A to C to B in 8, and adding stops would keep shortening the trip. No
measure of distance can work that way, so the rule is part of the definition.
:::

::: card
The common norms form one family, the **$p$-norms**:

$$ \|\mathbf{v}\|_p = \left(\sum_{i=1}^n |v_i|^p\right)^{1/p} $$

$p = 1$ adds the absolute values: the Manhattan or taxicab norm, a walk along the streets of a
grid. $p = 2$ is the Euclidean length. As $p$ grows, the largest component takes over, and in
the limit $\|\mathbf{v}\|_\infty = \max_i |v_i|$ ([vector norm](reference:vector-norm)).
:::

::: card
For $\mathbf{v} = [3, -4]^T$: $\|\mathbf{v}\|_1 = 7$, $\|\mathbf{v}\|_2 = 5$,
$\|\mathbf{v}\|_\infty = 4$. For $\mathbf{v} = [2, 3, -1]^T$:

$$ \|\mathbf{v}\|_1 = 6, \qquad \|\mathbf{v}\|_2 = \sqrt{14} \approx 3.74, \qquad \|\mathbf{v}\|_\infty = 3 $$

All three are valid norms, and they disagree. Transformers mostly use $L^2$.
:::

::: card
Draw every vector of size 1, and each norm draws its own shape. $L^1$ gives a diamond, $L^2$ a
circle, $L^\infty$ a square. Raise $p$ and the blue curve swells from the diamond, through the
circle at $p = 2$, toward the square.

```plot
x: { var: t, label: "$v₁$", from: -2, to: 2, ticks: 0.5, grid: true }
y: { label: "$v₂$", from: -1.3, to: 1.3 }

inputs:
  - { name: p, min: 1, max: 12, default: 2, step: 0.5, label: "p" }

draw:
  - polar: { var: a, over: [0, 2 * pi()], r: 1 / (abs(cos(a)) + abs(sin(a))), dash: true, label: "L1" }
  - polar: { var: a, over: [0, 2 * pi()], r: "1 / max(abs(cos(a)), abs(sin(a)))", dash: true, label: "L∞" }
  - polar: { var: a, over: [0, 2 * pi()], r: 1 / (abs(cos(a)) ^ p + abs(sin(a)) ^ p) ^ (1 / p), accent: true }
```
:::

::: card
A norm gives a **distance**: the size of the difference,

$$ d(\mathbf{u}, \mathbf{v}) = \|\mathbf{u} - \mathbf{v}\| $$

$L^2$ gives the straight-line distance, $L^1$ the Manhattan distance. Between word vectors, a
small distance means a similar meaning.
:::

::: card
A **unit vector** has norm 1. Divide any nonzero vector by its norm and you get the unit vector
in its direction:

$$ \hat{\mathbf{v}} = \frac{\mathbf{v}}{\|\mathbf{v}\|}, \qquad \|\hat{\mathbf{v}}\| = \frac{1}{\|\mathbf{v}\|}\,\|\mathbf{v}\| = 1 $$

The second step is the scaling rule. For $\mathbf{v} = [3, 4]^T$, $\hat{\mathbf{v}} = [3/5, 4/5]^T$.
Transformers rescale vectors all the time, so that values neither blow up nor fade away.
:::

::: card
A matrix has a size too. The **Frobenius norm** reads the matrix as one long vector:

$$ \|\mathbf{A}\|_F = \sqrt{\sum_{i,j} a_{ij}^2}, \qquad \left\|\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}\right\|_F = \sqrt{1 + 4 + 9 + 16} = \sqrt{30} $$
:::

::: exercise q1
Compute the $L^1$, $L^2$ and $L^\infty$ norms of $[1, -2, 2]^T$.

::: answer
5, 3 and 2. Add the absolute values; take the root of the sum of squares; take the largest
absolute value.
:::

::: solution
$\|\mathbf{v}\|_1 = 1 + 2 + 2 = 5$

$\|\mathbf{v}\|_2 = \sqrt{1 + 4 + 4} = 3$

$\|\mathbf{v}\|_\infty = \max(1, 2, 2) = 2$ ∎
:::
:::

::: exercise q2
Normalize $\mathbf{v} = [6, 8]^T$ in the $L^2$ norm.

::: answer
$[0.6, 0.8]^T$. Divide by $\|\mathbf{v}\|_2 = 10$.
:::
:::

::: exercise q3
Is $f(\mathbf{v}) = |v_1|$ a norm on $\mathbb{R}^2$?

::: answer
No. $f([0, 5]^T) = 0$, but the vector is not zero, so the first rule fails.
:::
:::

::: exercise q4
What is the Euclidean distance between $[1, 2]^T$ and $[4, 6]^T$?

::: answer
5. The difference is $[3, 4]^T$, whose length is 5.
:::
:::

::: reference vector-norm
# Vector norm

A norm assigns a size to each vector. It is zero only for the zero vector, it scales with the
absolute value of a scalar, and it obeys the triangle inequality. The $p$-norms are the common
family; $p = 2$ is the Euclidean length.

::: equation
\|\mathbf{v}\|_p = \left(\sum_{i=1}^n |v_i|^p\right)^{1/p} \qquad \|\mathbf{v}\|_\infty = \max_i |v_i| \qquad d(\mathbf{u}, \mathbf{v}) = \|\mathbf{u} - \mathbf{v}\|
:::

::: legend
$\mathbf{v}$: a vector of $\mathbb{R}^n$
$v_i$: its components
$p$: the order of the norm, $p \ge 1$
$d(\mathbf{u}, \mathbf{v})$: the distance between two vectors
:::

::: derivation
Let $m = \max_i |v_i|$, and let $k$ components reach it, $1 \le k \le n$.
Then $m^p \cdot k \le \sum_i |v_i|^p \le m^p \cdot n$.
Take the $p$-th root: $m\,k^{1/p} \le \|\mathbf{v}\|_p \le m\,n^{1/p}$.
As $p \to \infty$, both $k^{1/p}$ and $n^{1/p}$ tend to 1, so $\|\mathbf{v}\|_p \to m$. ∎
:::
:::

---
title: Linear algebra
---

Answer every question without notes. Show the working for each number.

::: exercise q1
Let $\mathbf{u} = [2, -1, 0]^T$ and $\mathbf{v} = [1, 3, 4]^T$. Compute $3\mathbf{u} + \mathbf{v}$
and $\mathbf{u} \cdot \mathbf{v}$.

::: answer
$3\mathbf{u} + \mathbf{v} = [7, 0, 4]^T$ and $\mathbf{u} \cdot \mathbf{v} = -1$.
:::

::: solution
$3\mathbf{u} = [6, -3, 0]^T$, so $3\mathbf{u} + \mathbf{v} = [7, 0, 4]^T$.

$\mathbf{u} \cdot \mathbf{v} = 2 \cdot 1 + (-1) \cdot 3 + 0 \cdot 4 = -1$. ∎
:::
:::

::: exercise q2
Is the map $f([v_1, v_2]^T) = [v_1 + 1, v_2]^T$ linear? Justify in one line.

::: answer
No. $f(\mathbf{0}) = [1, 0]^T$, and a linear map sends zero to zero.
:::
:::

::: exercise q3
Find the angle between $[1, 1, 0]^T$ and $[0, 1, 1]^T$.

::: answer
$60°$. The cosine is $1/2$.
:::

::: solution
$\mathbf{u} \cdot \mathbf{v} = 0 + 1 + 0 = 1$.

$\|\mathbf{u}\| = \|\mathbf{v}\| = \sqrt{2}$.

$\cos\theta = 1 / (\sqrt{2} \cdot \sqrt{2}) = 1/2$, so $\theta = 60°$. ∎
:::
:::

::: exercise q4
Compute $\mathbf{A}\mathbf{B}$ and $\mathbf{B}\mathbf{A}$ for
$\mathbf{A} = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}$ and
$\mathbf{B} = \begin{bmatrix} 2 & 0 \\ 0 & 1 \end{bmatrix}$.

::: answer
$\mathbf{A}\mathbf{B} = \begin{bmatrix} 2 & 1 \\ 0 & 1 \end{bmatrix}$ and
$\mathbf{B}\mathbf{A} = \begin{bmatrix} 2 & 2 \\ 0 & 1 \end{bmatrix}$. They differ.
:::

::: solution
$\mathbf{A}\mathbf{B}$: row $[1, 1]$ with the columns $[2, 0]^T$, $[0, 1]^T$ gives $2, 1$; row $[0, 1]$ gives $0, 1$.

$\mathbf{B}\mathbf{A}$: row $[2, 0]$ with the columns $[1, 0]^T$, $[1, 1]^T$ gives $2, 2$; row $[0, 1]$ gives $0, 1$. ∎
:::
:::

::: exercise q5
Find the coordinates of $[4, 2]^T$ in the basis $\mathbf{b}_1 = [1, 1]^T$,
$\mathbf{b}_2 = [1, -1]^T$.

::: answer
$[3, 1]$.
:::

::: solution
$c_1 + c_2 = 4$ and $c_1 - c_2 = 2$.

Add: $2c_1 = 6$, so $c_1 = 3$. Subtract: $2c_2 = 2$, so $c_2 = 1$.

Check: $3[1, 1]^T + [1, -1]^T = [4, 2]^T$. ∎
:::
:::

::: exercise q6
What are the rank and the determinant of $\begin{bmatrix} 3 & 6 \\ 1 & 2 \end{bmatrix}$? Does it
have an inverse?

::: answer
Rank 1, determinant 0, no inverse. The second column is twice the first.
:::
:::

::: exercise q7
Find the eigenvalues and one eigenvector for each, of $\begin{bmatrix} 3 & 0 \\ 1 & 2 \end{bmatrix}$.

::: answer
$\lambda = 3$ with $[1, 1]^T$, and $\lambda = 2$ with $[0, 1]^T$.
:::

::: solution
$\det(\mathbf{A} - \lambda\mathbf{I}) = (3 - \lambda)(2 - \lambda) - 0 = 0$, so $\lambda = 3$ or $\lambda = 2$.

$\lambda = 3$: the second row of $\mathbf{A} - 3\mathbf{I}$ is $[1, -1]$, so $v_1 = v_2$: $[1, 1]^T$.

$\lambda = 2$: the first row of $\mathbf{A} - 2\mathbf{I}$ is $[1, 0]$, so $v_1 = 0$: $[0, 1]^T$.

Check: $\mathbf{A}[1, 1]^T = [3, 3]^T$ and $\mathbf{A}[0, 1]^T = [0, 2]^T$. ∎
:::
:::

::: exercise q8
Give the $L^1$, $L^2$ and $L^\infty$ norms of $[-3, 0, 4]^T$, and the unit vector in its
direction.

::: answer
7, 5 and 4. The unit vector is $[-0.6, 0, 0.8]^T$.
:::
:::

::: exercise q9
A weight matrix maps $\mathbb{R}^{768}$ to $\mathbb{R}^{3072}$. Give its size, and say whether it
compresses, expands or mixes.

::: answer
$3072 \times 768$. It expands: it has more outputs than inputs.
:::
:::

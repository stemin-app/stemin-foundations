---
title: Self-attention
---

Answer every question without notes.

::: exercise q1
A decoder state attends over its own earlier states. Is this self-attention or cross-attention?

::: answer
Self-attention. The queries, keys and values all come from the same sequence.
:::
:::

::: exercise q2
$\mathbf{X}$ is $8 \times 512$ and $d_k = 64$. Give the shapes of $\mathbf{Q}$, of the score matrix,
and of the output $\mathbf{A}\mathbf{V}$.

::: answer
$8 \times 64$, $8 \times 8$ and $8 \times 64$.
:::
:::

::: exercise q3
Two tokens have $\mathbf{q}_1 = [1, 0]$, $\mathbf{k}_1 = [0, 1]$, $\mathbf{k}_2 = [2, 0]$, and
$d_k = 2$. What weight does token 1 put on token 2?

::: answer
About 0.804. The scaled scores are 0 and $\sqrt{2} \approx 1.414$.
:::

::: solution
$s_{11} = 0 / \sqrt{2} = 0$, $s_{12} = 2 / \sqrt{2} \approx 1.414$.

$a_{12} = e^{1.414} / (1 + e^{1.414}) = 4.113 / 5.113 \approx 0.804$. ∎
:::
:::

::: exercise q4
A context grows from 2,048 to 8,192 tokens. By what factor does the work of self-attention grow,
at fixed $d$?

::: answer
16. The work is $O(n^2 d)$, and the length grows by 4.
:::
:::

::: exercise q5
Show with numbers that $g(x) = \mathrm{ReLU}(x)$ is not linear.

::: answer
$g(-1) + g(1) = 0 + 1 = 1$, but $g(-1 + 1) = g(0) = 0$.
:::
:::

::: exercise q6
An FFN has $\mathbf{W}_1 = \begin{bmatrix} 1 & -1 \\ -1 & 1 \end{bmatrix}$, $\mathbf{W}_2 = \begin{bmatrix} 1 & 1 \end{bmatrix}$
and no biases. Compute its output for $\mathbf{x} = [3, 1]$ and for $\mathbf{x} = [1, 3]$. Which
function of $x_1, x_2$ is it?

::: answer
2 both times. It is $|x_1 - x_2|$.
:::

::: solution
$[3, 1]$: $\mathrm{ReLU}([2, -2]) = [2, 0]$, so the output is 2.

$[1, 3]$: $\mathrm{ReLU}([-2, 2]) = [0, 2]$, so the output is 2.

In general the hidden layer is $[\mathrm{ReLU}(x_1 - x_2), \mathrm{ReLU}(x_2 - x_1)]$, whose sum is $|x_1 - x_2|$. ∎
:::
:::

::: exercise q7
A sublayer has $\frac{\partial f}{\partial x} = 0.2$ in each of 10 stacked layers. What factor
reaches the first layer without residual connections? Along the skip path with them?

::: answer
$0.2^{10} \approx 10^{-7}$ without; 1 along the skip path with them.
:::
:::

::: exercise q8
Apply layer normalization to $[1, 3]$, with $\boldsymbol{\gamma} = \mathbf{1}$, $\boldsymbol{\beta} = \mathbf{0}$
and $\epsilon$ ignored.

::: answer
$[-1, 1]$. The mean is 2 and $\sigma = 1$.
:::
:::

::: exercise q9
With a causal mask, row 3 has the scores $[0, 0, 0, 2, 5]$ before the mask. What are its weights?

::: answer
$[1/3, 1/3, 1/3, 0, 0]$. Positions 4 and 5 are masked, and the first three scores are equal.
:::
:::

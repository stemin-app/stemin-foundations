---
title: The attention mechanism
---

Answer every question without notes.

::: exercise q1
Values $[2, 0]$ and $[0, 4]$ get the scores 0 and $\ln 3$. What is the attention output?

::: answer
$[0.5, 3]$. The weights are $[0.25, 0.75]$.
:::

::: solution
$e^0 = 1$ and $e^{\ln 3} = 3$; the sum is 4.

Weights: $[0.25, 0.75]$.

$\mathbf{o} = 0.25[2, 0] + 0.75[0, 4] = [0.5, 3]$. ∎
:::
:::

::: exercise q2
$\mathbf{q} = [1, 1, 1, 1]$ and $\mathbf{k} = [2, 0, 1, 1]$. What is the scaled dot-product
score?

::: answer
2. The dot product is 4, and $\sqrt{4} = 2$.
:::
:::

::: exercise q3
Queries and keys have size 256 with independent entries of variance 1. What is the standard
deviation of an unscaled dot product, and why is that a problem?

::: answer
16. Scores that large saturate the softmax: almost all weight lands on one key, and the gradient
through the softmax nearly vanishes.
:::
:::

::: exercise q4
One query $[1, 0]$ attends over the keys $[2, 0]$ and $[0, 2]$ with the values $[1, 0]$ and
$[0, 1]$, and $d = 2$. Give the weights and the output, to three decimals.

::: answer
Weights about $[0.804, 0.196]$; the output is about $[0.804, 0.196]$.
:::

::: solution
Scores: $2/\sqrt{2} \approx 1.414$ and $0$.

$e^{1.414} \approx 4.113$; the sum is $5.113$.

Weights: $4.113/5.113 \approx 0.804$ and $1/5.113 \approx 0.196$.

The values are $\mathbf{e}_1$ and $\mathbf{e}_2$, so the output is the weights themselves. ∎
:::
:::

::: exercise q5
$\mathbf{Q}$ is $6 \times 32$, $\mathbf{K}$ is $9 \times 32$, $\mathbf{V}$ is $9 \times 16$.
Give the shapes of $\mathbf{Q}\mathbf{K}^T$ and of the attention output.

::: answer
$6 \times 9$ and $6 \times 16$.
:::
:::

::: exercise q6
$d_{\text{model}} = 768$ and $d_k = d_v = 64$. How many parameters do the three projections
$\mathbf{W}^Q, \mathbf{W}^K, \mathbf{W}^V$ hold?

::: answer
147,456. Three matrices of $768 \times 64 = 49{,}152$.
:::
:::

::: exercise q7
Why does attention need positional information to tell "dog bites man" from "man bites dog"?

::: answer
Attention is permutation equivariant: it compares content only, so a reordered input gives the
same outputs, reordered.
:::
:::

::: exercise q8
How does the number of attention scores grow when the sequence length goes from $n$ to $3n$?

::: answer
It grows by 9: the count is $n^2$.
:::
:::

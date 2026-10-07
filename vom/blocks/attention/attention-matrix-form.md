---
title: All queries at once
---

::: card
Stack the queries as the rows of $\mathbf{Q} \in \mathbb{R}^{m \times d}$, the keys as the rows of
$\mathbf{K} \in \mathbb{R}^{n \times d}$, and the values as the rows of
$\mathbf{V} \in \mathbb{R}^{n \times d_v}$. Then one formula computes every output:

$$ \mathrm{Attention}(\mathbf{Q}, \mathbf{K}, \mathbf{V}) = \mathrm{softmax}\left(\frac{\mathbf{Q}\mathbf{K}^T}{\sqrt{d}}\right)\mathbf{V} $$

This is **scaled dot-product attention**, from "Attention Is All You Need" (2017). Every modern
transformer uses it ([scaled dot-product attention](reference:scaled-dot-product-attention)).
:::

::: card
Read it in four moves. $\mathbf{Q}\mathbf{K}^T$ is an $m \times n$ matrix whose entry $(i, j)$ is
query $i$ dotted with key $j$: every score in one product. Divide by $\sqrt{d}$. Apply the softmax
to **each row** on its own, so row $i$ holds the weights of query $i$. Multiply by $\mathbf{V}$:
row $i$ of the result mixes the values with row $i$'s weights.
:::

::: card
The shapes chain: $(m \times d)(d \times n) = m \times n$, then $(m \times n)(n \times d_v) = m \times d_v$.
One output row per query, each of size $d_v$.
:::

::: card
The worked example in matrix form, with one query as a $1 \times 3$ matrix:

$$ \mathbf{Q}\mathbf{K}^T = \begin{bmatrix} 1 & 0 & 1 \end{bmatrix}\begin{bmatrix} 1 & 0 & 1 \\ 1 & 1 & 0 \\ 0 & 1 & 1 \end{bmatrix} = \begin{bmatrix} 1 & 1 & 2 \end{bmatrix} $$

Scaled: $[0.577, 0.577, 1.155]$. Softmax: $[0.264, 0.264, 0.471]$. Times $\mathbf{V}$: $[3.41, 4.41]$,
the same as before.
:::

::: exercise q1
$\mathbf{Q}$ is $10 \times 64$, $\mathbf{K}$ is $12 \times 64$ and $\mathbf{V}$ is $12 \times 32$.
What are the shapes of the score matrix and of the output?

::: answer
The scores are $10 \times 12$ and the output is $10 \times 32$.
:::
:::

::: exercise q2
Along which direction is the softmax applied to the score matrix, and what does each row of the
result add up to?

::: answer
Along each row, one query at a time. Each row of weights adds up to 1.
:::
:::

::: reference scaled-dot-product-attention
# Scaled dot-product attention

Each query scores every key with a dot product scaled by $\sqrt{d}$. A softmax over each row turns
the scores into weights, and each output row is the values mixed with those weights.

::: equation
\mathrm{Attention}(\mathbf{Q}, \mathbf{K}, \mathbf{V}) = \mathrm{softmax}\left(\frac{\mathbf{Q}\mathbf{K}^T}{\sqrt{d}}\right)\mathbf{V}
:::

::: legend
$\mathbf{Q}$: the queries, $m \times d$
$\mathbf{K}$: the keys, $n \times d$
$\mathbf{V}$: the values, $n \times d_v$
$d$: the size of a query and of a key
$\mathrm{softmax}$: applied to each row on its own
:::

::: derivation
Entry $(i, j)$ of $\mathbf{Q}\mathbf{K}^T$ is row $i$ of $\mathbf{Q}$ times column $j$ of $\mathbf{K}^T$, which is $\mathbf{q}_i^T\mathbf{k}_j$.[matrix product](reference:matrix-product)
Dividing by $\sqrt{d}$ gives the scaled scores, and a softmax of row $i$ gives the weights $\alpha_{ij}$ of query $i$.[attention weights](reference:attention-weights)
Row $i$ of the product with $\mathbf{V}$ is $\sum_j \alpha_{ij}\mathbf{v}_j$, the output of query $i$. ∎
:::
:::

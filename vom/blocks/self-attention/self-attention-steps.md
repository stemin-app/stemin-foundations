---
title: Four steps of self-attention
---

::: card
The input is a matrix $\mathbf{X} \in \mathbb{R}^{n \times d}$. It has one row per token: $n$ is
the length of the sequence and $d$ is the embedding dimension. Row $i$ is the embedding of token
$i$. Self-attention turns $\mathbf{X}$ into a new matrix in four steps
([Figure](figure:self-attention-flow)).
:::

::: figure self-attention-flow
![The four steps of self-attention](assets/self-attention-flow.svg)

One input $\mathbf{X}$ feeds all three projections. Queries meet keys in an $n \times n$ score
matrix, softmax turns each row into weights, and the weights blend the values.
:::

::: card
**Step 1: project.** Multiply $\mathbf{X}$ by three learned matrices:

$$ \mathbf{Q} = \mathbf{X}\mathbf{W}^Q, \quad \mathbf{K} = \mathbf{X}\mathbf{W}^K, \quad \mathbf{V} = \mathbf{X}\mathbf{W}^V $$

Each $\mathbf{W} \in \mathbb{R}^{d \times d_k}$ maps a $d$-dimensional embedding to a
$d_k$-dimensional query, key or value. The same $\mathbf{X}$ goes into all three. That is the
"self" in self-attention.
:::

::: card
The width $d_k$ is a design choice. Often $d_k = d$. With $h$ attention heads it is usually
$d_k = d / h$, so the heads together cost about as much as one full-width head.
:::

::: card
**Step 2: score.** Compare every query with every key:

$$ \mathbf{S} = \frac{\mathbf{Q}\mathbf{K}^T}{\sqrt{d_k}} $$

$\mathbf{S} \in \mathbb{R}^{n \times n}$. Entry $s_{ij} = \mathbf{q}_i \cdot \mathbf{k}_j / \sqrt{d_k}$
measures how relevant position $j$ is to position $i$.
[Scaled dot-product attention](reference:scaled-dot-product-attention)
:::

::: card
**Step 3: normalise.** Apply softmax to each row of $\mathbf{S}$:

$$ \mathbf{A} = \operatorname{softmax}(\mathbf{S}) $$

Row $i$ of $\mathbf{A}$ is a probability distribution over the $n$ positions: how much position
$i$ attends to each one.[Softmax](reference:softmax)
:::

::: card
**Step 4: blend.** Multiply the weights by the values:

$$ \mathbf{O} = \mathbf{A}\mathbf{V}, \qquad \mathbf{o}_i = \sum_{j=1}^{n} a_{ij}\,\mathbf{v}_j $$

$\mathbf{O} \in \mathbb{R}^{n \times d_k}$. Row $i$ is a weighted average of all the value
vectors, weighted by how much position $i$ attends to each.
:::

::: card
The four steps fold into one expression:

$$ \operatorname{SelfAttention}(\mathbf{X}) = \operatorname{softmax}\!\left(\frac{\mathbf{X}\mathbf{W}^Q (\mathbf{X}\mathbf{W}^K)^T}{\sqrt{d_k}}\right) \mathbf{X}\mathbf{W}^V $$

[Self-attention](reference:self-attention)
:::

::: exercise score-matrix-shape
A sequence has $n = 10$ tokens with $d = 512$ and $d_k = 64$. What is the shape of the score
matrix $\mathbf{S}$?

::: answer
$10 \times 10$. One score for each pair of positions; $d_k$ does not appear.
:::

::: solution
$\mathbf{Q}$ and $\mathbf{K}$ are $10 \times 64$, so $\mathbf{K}^T$ is $64 \times 10$.

$$ (10 \times 64)(64 \times 10) = 10 \times 10 $$

∎
:::
:::

::: exercise projection-parameter-count
With $d = 512$ and $d_k = 64$, how many learned numbers do $\mathbf{W}^Q$, $\mathbf{W}^K$ and
$\mathbf{W}^V$ hold together?

::: answer
98,304. Each matrix is $512 \times 64$.
:::

::: solution
One matrix: $512 \times 64 = 32{,}768$.

Three matrices: $3 \times 32{,}768 = 98{,}304$. ∎
:::
:::

::: exercise output-shape
With $n = 10$, $d = 512$ and $d_k = 64$, what is the shape of the output $\mathbf{O}$?

::: answer
$10 \times 64$. $\mathbf{A}$ is $10 \times 10$ and $\mathbf{V}$ is $10 \times 64$.
:::
:::

::: reference self-attention
# Self-attention

Self-attention projects one input into queries, keys and values, scores every query against every
key, normalises each row with softmax, and blends the values with those weights.

::: equation
\operatorname{SelfAttention}(\mathbf{X}) = \operatorname{softmax}\!\left(\frac{\mathbf{Q}\mathbf{K}^T}{\sqrt{d_k}}\right)\mathbf{V}, \qquad \mathbf{Q} = \mathbf{X}\mathbf{W}^Q,\ \mathbf{K} = \mathbf{X}\mathbf{W}^K,\ \mathbf{V} = \mathbf{X}\mathbf{W}^V
:::

::: legend
$\mathbf{X}$: the input, one token embedding per row, $n \times d$
$\mathbf{W}^Q, \mathbf{W}^K, \mathbf{W}^V$: learned projections, each $d \times d_k$
$\mathbf{Q}, \mathbf{K}, \mathbf{V}$: queries, keys and values, each $n \times d_k$
$d_k$: the width of a query or key
$n$: the number of tokens
:::

::: derivation
Project the one input three ways: $\mathbf{Q} = \mathbf{X}\mathbf{W}^Q$, $\mathbf{K} = \mathbf{X}\mathbf{W}^K$, $\mathbf{V} = \mathbf{X}\mathbf{W}^V$.[Matrix product](reference:matrix-product)
Apply attention with these three matrices: $\operatorname{softmax}(\mathbf{Q}\mathbf{K}^T / \sqrt{d_k})\,\mathbf{V}$.[Scaled dot-product attention](reference:scaled-dot-product-attention)
Substitute the projections to get the expression in $\mathbf{X}$ alone. ∎
:::
:::

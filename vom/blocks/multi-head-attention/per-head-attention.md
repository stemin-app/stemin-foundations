---
title: Attention inside each head
---

::: card
Write $\mathbf{X}$ for the input: one row per token, so $n \times d_\text{model}$ for $n$
tokens. Each head $i = 1, \dots, h$ first projects the same $\mathbf{X}$ into its own space.

$$ \mathbf{Q}_i = \mathbf{X}\mathbf{W}_i^Q, \quad \mathbf{K}_i = \mathbf{X}\mathbf{W}_i^K, \quad \mathbf{V}_i = \mathbf{X}\mathbf{W}_i^V $$
:::

::: card
Count the shapes with $d_\text{model} = 512$ and $d_k = 64$. An $n \times 512$ matrix times a
$512 \times 64$ matrix is $n \times 64$.[Matrix product](reference:matrix-product) Each head
keeps 64 numbers per token for its queries, 64 for its keys and 64 for its values.
:::

::: card
Then each head attends, with the same formula as a single head, inside its 64
dimensions.[Scaled dot-product attention](reference:scaled-dot-product-attention)

$$ \text{head}_i = \text{softmax}\left(\frac{\mathbf{Q}_i \mathbf{K}_i^\top}{\sqrt{d_k}}\right) \mathbf{V}_i $$
:::

::: card
$\mathbf{Q}_i \mathbf{K}_i^\top$ is $n \times n$, so each head has its own $n \times n$ table of
weights: its own answer to "who looks at whom". The result $\text{head}_i$ is $n \times 64$: one
64-wide vector per token.
:::

::: card
The scale is $\sqrt{d_k} = \sqrt{64} = 8$, not $\sqrt{512}$. Each score in head $i$ is a dot
product of two 64-wide vectors, a sum of 64 terms, so the head divides by the square root of 64.
:::

::: card
The heads share the input $\mathbf{X}$ and nothing else. No head reads the weights of another,
so all $h$ heads run at the same time, in parallel.
:::

::: exercise q1
A sequence has $n = 10$ tokens and the layer has $h = 8$ heads. How many attention weights does
the layer compute over all its heads?

::: answer
800. Each head computes an $n \times n$ table of weights.
:::

::: solution
$$ h \times n \times n = 8 \times 10 \times 10 = 800 $$

∎
:::
:::

::: exercise q2
A sequence has $n = 20$ tokens, $d_\text{model} = 512$ and $h = 8$. What is the shape of
$\text{head}_3$?

::: answer
$20 \times 64$. One row per token, $d_k = 512 / 8 = 64$ columns.
:::
:::

::: exercise q3
A model has $d_\text{model} = 1024$ and $h = 16$ heads. By what number does each head divide its
scores?

::: answer
8. Each head has $d_k = 1024 / 16 = 64$, and $\sqrt{64} = 8$.
:::

::: solution
$$ d_k = \frac{1024}{16} = 64 $$

$$ \sqrt{d_k} = \sqrt{64} = 8 $$

∎
:::
:::

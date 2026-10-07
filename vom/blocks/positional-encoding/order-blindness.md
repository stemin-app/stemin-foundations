---
title: Attention cannot see order
---

::: card
"The dog bit the man." "The man bit the dog." The two sentences use the same words and describe
two different events. In language, order carries meaning.
:::

::: card
A recurrent network reads tokens one at a time, so its state at step $t$ comes after step
$t - 1$. Order is built in. A transformer reads every position at once. It gains speed, and it
loses the order.
:::

::: card
Look at the attention weight from position $i$ to position $j$.[Attention weights](reference:attention-weights)

$$ \alpha_{ij} = \frac{\exp\left(\mathbf{q}_i^\top \mathbf{k}_j / \sqrt{d}\right)}{\sum_m \exp\left(\mathbf{q}_i^\top \mathbf{k}_m / \sqrt{d}\right)} $$

It depends only on the content, through $\mathbf{q}_i$ and $\mathbf{k}_j$. Nothing in it says
whether $j$ comes before $i$, next to it, or 100 places away.
:::

::: card
So shuffle the input tokens, and the outputs shuffle in the same way and change in no other
way.[Permutation](reference:attention-permutation-equivariance) The output vector for "dog" is
the same in "the dog bit the man" and in "the man bit the dog".
:::

::: card
Without position, a transformer is a bag-of-words model. Some tasks survive this. The mood of a
film review is often clear from which words appear. Most tasks do not: "Who did Alice meet?" and
"Who met Alice?" ask different questions. A transformer trained without position on parsing or
translation does far worse.
:::

::: card
The problem comes from attention alone. A recurrent network, or a convolution that reads
neighbours, has position built in. A **positional encoding** gives attention the order back, and
keeps the parallel reading.
:::

::: exercise q1
Self-attention with no position information maps the input tokens $(a, b, c)$ to the outputs
$(\mathbf{y}_a, \mathbf{y}_b, \mathbf{y}_c)$. What are the outputs for the input $(c, a, b)$?

::: answer
$(\mathbf{y}_c, \mathbf{y}_a, \mathbf{y}_b)$. The outputs follow the tokens and do not change.
:::
:::

::: exercise q2
A token keeps its content and moves from position 2 to position 50. Does its key, and so its
score $\mathbf{q}_i^\top \mathbf{k}_j$ with any query, change?

::: answer
No. The key is computed from the content alone, so the score is the same.
:::
:::

::: exercise q3
Without position information, is the output vector for "bites" the same in "dog bites man" and in
"man bites dog"?

::: answer
Yes. Both inputs hold the same three tokens, so each token gets the same output.
:::
:::

::: reference attention-permutation-equivariance
# Self-attention ignores order

Without position information, self-attention is permutation equivariant: reorder the input rows,
and the output rows reorder in the same way and do not change.

::: equation
\text{Attn}(\mathbf{P}\mathbf{X}) = \mathbf{P}\,\text{Attn}(\mathbf{X})
:::

::: legend
$\mathbf{X}$: the input, one row per token
$\mathbf{P}$: a permutation matrix, which reorders the rows
$\text{Attn}$: self-attention, $\text{softmax}\left(\mathbf{Q}\mathbf{K}^\top/\sqrt{d}\right)\mathbf{V}$
:::

::: derivation
The projections act row by row: $\mathbf{P}\mathbf{X}\mathbf{W}^Q = \mathbf{P}\mathbf{Q}$, and likewise $\mathbf{P}\mathbf{K}$ and $\mathbf{P}\mathbf{V}$.[Self-attention](reference:self-attention)
The scores become $\mathbf{P}\mathbf{Q}(\mathbf{P}\mathbf{K})^\top = \mathbf{P}\,\mathbf{Q}\mathbf{K}^\top\mathbf{P}^\top$: the same entries, rows and columns reordered.
The softmax acts on each row, and reordering the entries of a row reorders its outputs, so $\text{softmax}(\mathbf{P}\mathbf{S}\mathbf{P}^\top) = \mathbf{P}\,\text{softmax}(\mathbf{S})\,\mathbf{P}^\top$.[Softmax](reference:softmax)
The output is $\mathbf{P}\,\text{softmax}(\mathbf{S})\,\mathbf{P}^\top\mathbf{P}\mathbf{V}$.
A permutation matrix has $\mathbf{P}^\top\mathbf{P} = \mathbf{I}$, so the output is $\mathbf{P}\,\text{softmax}(\mathbf{S})\,\mathbf{V} = \mathbf{P}\,\text{Attn}(\mathbf{X})$. ∎
:::
:::

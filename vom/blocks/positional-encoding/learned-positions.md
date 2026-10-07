---
title: Learned position embeddings
---

::: card
Many transformers learn their position vectors instead of computing them. They hold a matrix
$\mathbf{P} \in \mathbb{R}^{L_{\max} \times d}$, one row per position up to a maximum length
$L_{\max}$, and look up row $t$ for position $t$, the way a token looks up its embedding. The rows
start random and train with the rest of the model.
:::

::: card
The gain is freedom. No form is fixed in advance, so the model learns whatever position signal
helps its task. In practice learned embeddings work as well as sinusoidal ones, or slightly
better. GPT-2 uses them, with $L_{\max} = 1{,}024$.
:::

::: card
The cost is the edge. Train with $L_{\max} = 512$ and rows exist for positions 0 to 511 only.
Position 700 has no row. You can reuse or stretch the learned rows, but nothing guarantees that
works. A sinusoidal encoding is a formula, so it gives every position a vector, seen in training or
not.
:::

::: card
For most uses a known maximum length is acceptable: the model trains on sequences up to it. Models
that must read longer inputs use other schemes. **Relative** encodings put the distance between
two positions into the attention score itself. **Rotary** encodings rotate the queries and keys by
their position, the rotation idea of the sinusoidal encoding applied inside attention.
:::

::: exercise q1
A model has $L_{\max} = 2{,}048$ and $d = 1{,}024$. How many parameters do its learned position
embeddings hold?

::: answer
2,097,152. The matrix is $2{,}048 \times 1{,}024$.
:::
:::

::: exercise q2
A model with learned positions and $L_{\max} = 1{,}024$ must read 1,500 tokens. Which positions
have no learned row?

::: answer
Positions 1,024 to 1,499: 476 positions, counting from 0.
:::
:::

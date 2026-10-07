---
title: Four properties of attention
---

::: card
Attention is **permutation equivariant**: shuffle the input tokens and the outputs shuffle the
same way, unchanged. Take "cat sat mat" and reorder it to "mat cat sat". Each word gets the same
output as before, only in a new place. The output for "cat" is the same whether "cat" stood first
or second.
:::

::: card
The reason: attention compares content only, a query against keys. The weight between "cat" and
"sat" depends on their vectors, not on their positions. Attention sees a **set** of vectors, not
a sequence. That is a problem for language, where "dog bites man" is not "man bites dog". The fix
is to add position information to the embeddings before attention runs, with a positional
encoding.
:::

::: card
Attention is **differentiable**: the dot products, the scaling, the softmax and the weighted sum
are all smooth functions. Gradients flow through it, and the whole model trains end to end by
backpropagation.

Attention is **parallel**: no position waits for another. The product $\mathbf{Q}\mathbf{K}^T$
computes every score at once, which is exactly the work a GPU does best.
:::

::: card
Attention is **dynamic**: the weights are computed from the input, so the same trained model
attends to different positions for different sentences. A fully connected layer also links every
position to every other, but its weights are fixed parameters, the same for every input, and tied
to one input length. Attention routes by content, and it works for any length.
:::

::: exercise q1
Without positional information, does attention give different outputs for "the cat sat" and "sat
the cat"?

::: answer
No. It gives the same output vector for each word, only in a different order.
:::
:::

::: exercise q2
Name the property that lets one trained attention layer route information differently for two
different sentences.

::: answer
It is dynamic: the weights come from the queries and keys of the input, not from fixed parameters.
:::
:::

---
title: Embeddings
---

Answer every question without notes. Use the natural logarithm.

::: exercise q1
Why can a network learn nothing about "dog" from what it learned about "cat" when both are
one-hot vectors?

::: answer
The two vectors share no nonzero entry: their dot product is 0, and the weights that read one
never see the other.
:::
:::

::: exercise q2
An embedding matrix has $V = 30{,}000$ and $d = 512$. How many parameters does it hold, and how
many rows get a gradient from a batch that contains 40 distinct tokens?

::: answer
15,360,000 parameters, and 40 rows. Only the rows of tokens in the batch get a gradient.
:::
:::

::: exercise q3
Find the cosine similarity of $[1, 2, 2]$ and $[2, 1, 2]$.

::: answer
$8/9 \approx 0.889$.
:::

::: solution
Dot product: $2 + 2 + 4 = 8$.

Both lengths: $\sqrt{1 + 4 + 4} = 3$.

Similarity: $8 / (3 \cdot 3) = 8/9$. ∎
:::
:::

::: exercise q4
In two dimensions, $\mathbf{e}_{\text{Paris}} = (2, 5)$, $\mathbf{e}_{\text{France}} = (1, 1)$
and $\mathbf{e}_{\text{Italy}} = (4, 1)$. Compute
$\mathbf{e}_{\text{Paris}} - \mathbf{e}_{\text{France}} + \mathbf{e}_{\text{Italy}}$. Which word
should lie near it?

::: answer
$(5, 5)$. It should lie near "Rome".
:::
:::

::: exercise q5
In "a b c d e" with a window of $c = 2$, how many (center, context) pairs are there?

::: answer
14.
:::

::: solution
"a" and "e" each have 2 neighbours within two places: 4 pairs.

"b" and "d" each have 3: 6 pairs.

"c" has 4: 4 pairs.

Total: $4 + 6 + 4 = 14$. ∎
:::
:::

::: exercise q6
With negative sampling, $k = 2$, a positive dot product of 0 and two negative dot products of 0,
what is $\mathcal{L}$?

::: answer
$3 \log 0.5 \approx -2.079$. Every sigmoid is $\sigma(0) = 0.5$.
:::
:::

::: exercise q7
A corpus holds "aab", "aab" and "ab". Which pair does BPE merge first, and what does the corpus
look like after it?

::: answer
$(\text{a}, \text{b})$, with 3 occurrences. The corpus becomes "a ab", "a ab", "ab".
:::

::: solution
"a a b" gives $(\text{a}, \text{a})$ and $(\text{a}, \text{b})$, twice over: 2 and 2.

"a b" gives $(\text{a}, \text{b})$ once more.

Counts: $(\text{a}, \text{b})$ 3, $(\text{a}, \text{a})$ 2. Merge $(\text{a}, \text{b})$. ∎
:::
:::

::: exercise q8
What does "contextual embedding" mean, and which part of a transformer produces it?

::: answer
A vector for a token that depends on the words around it. The attention layers produce it from
the static rows of $\mathbf{E}$.
:::
:::

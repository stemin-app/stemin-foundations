---
title: The trouble with one-hot vectors
---

::: card
A neural network computes with numbers, so a word must become a vector before the network can
use it. The simplest way gives each word of a vocabulary of $V$ words its own axis.

Word $i$ becomes $\mathbf{e}_i \in \mathbb{R}^V$, with a 1 in position $i$ and a 0 in every
other position. This is *one-hot encoding*.
:::

::: card
Take a vocabulary of 50,000 words, where "cat" has index 3,142 and "dog" has index 7,891. Each
word becomes a vector of 50,000 entries:

$$
\begin{aligned}
\text{cat} &\to [0, 0, \ldots, 1, \ldots, 0, 0]^T \quad \text{(1 in position 3,142)} \\
\text{dog} &\to [0, 0, \ldots, 1, \ldots, 0, 0]^T \quad \text{(1 in position 7,891)}
\end{aligned}
$$
:::

::: card
The vectors are huge. Each has $V$ entries, and a real vocabulary holds tens or hundreds of
thousands of words.

They are also sparse. Exactly one entry is nonzero, so almost all the arithmetic a network does
with them multiplies by zero.
:::

::: card
Worse, the vectors carry no similarity. The [dot product](reference:dot-product) of two different
one-hot vectors is always zero:

$$ \mathbf{e}_i^T \mathbf{e}_j = \begin{cases} 1 & i = j \\ 0 & i \neq j \end{cases} $$

By this measure "cat" is exactly as far from "dog" as it is from "philosophy". Nothing says some
words are more related than others.
:::

::: card
With no shared structure, nothing generalizes. Whatever the network learns about "cat" does not
carry over to "dog", because the two vectors share no entry. The network must learn every word
from scratch.
:::

::: card
You want a representation with the opposite properties. Each vector is dense and short, say 256
or 512 entries instead of 50,000. Similar words get similar vectors. And the relations between
words show up as geometry: directions and distances in the vector space.
:::

::: exercise one-hot-zeros
A vocabulary holds 50,000 words. How many entries of a one-hot vector are zero?

::: answer
49,999. Exactly one entry of the 50,000 is a 1.
:::
:::

::: exercise one-hot-distance
What is the Euclidean distance between the one-hot vectors of two different words?

::: answer
$\sqrt{2}$, for every pair. The difference has one entry $+1$ and one entry $-1$.
:::

::: solution
The difference $\mathbf{e}_i - \mathbf{e}_j$ has $+1$ in position $i$, $-1$ in position $j$, and 0 elsewhere.

$$ \|\mathbf{e}_i - \mathbf{e}_j\|^2 = 1^2 + (-1)^2 = 2 $$

$$ \|\mathbf{e}_i - \mathbf{e}_j\| = \sqrt{2} \approx 1.414 $$

The result does not depend on $i$ or $j$, so every pair of words is equally far apart. ∎
:::
:::

::: exercise one-hot-cosine
What is the cosine similarity of the one-hot vectors for "cat" and "dog"?

::: answer
0. The dot product of two different one-hot vectors is 0, and each vector has length 1.
:::
:::

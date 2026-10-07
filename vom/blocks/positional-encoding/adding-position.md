---
title: Adding a position vector
---

::: card
The transformer adds a position vector to each embedding. Write $\mathbf{e}_t$ for the embedding
of the token at position $t$, and $\mathbf{p}_t$ for the **positional encoding** of position
$t$.[Embedding lookup](reference:embedding-lookup) The input to the first layer is

$$ \mathbf{x}_t = \mathbf{e}_t + \mathbf{p}_t $$
:::

::: card
$\mathbf{p}_t \in \mathbb{R}^d$ has the same width $d$ as the embedding, so the two can be added.
Adding keeps the width at $d$. Appending would make it $2d$, and every layer after it would have
to grow.
:::

::: card
Each coordinate of $\mathbf{x}_t$ now mixes "what this token is" with "where it is". The same word
at two positions gives two slightly different vectors. Here the embedding is $(2, 1)$ and the
position adds a small turning vector. Slide the position and the input moves around the word.

```plot
x: { var: u, label: "first coordinate", from: 0, to: 3.5, ticks: 0.5, grid: true }
y: { label: "second coordinate", from: 0, to: 2.2 }

inputs:
  - { name: t, min: 0, max: 12, default: 2, step: 1, label: "position t" }

let:
  px: 0.5 * sin(0.5 * t)
  py: 0.5 * cos(0.5 * t)

draw:
  - param: { var: s, over: [0, 2 * pi()], x: 2 + 0.5 * sin(s), y: 1 + 0.5 * cos(s), dash: true }
  - param: { var: s, over: [0, 1], x: 2 * s, y: s, label: "$e$" }
  - param: { var: s, over: [0, 1], x: 2 + s * px, y: 1 + s * py, accent: true, label: "$pₜ$" }
  - point: { at: [2 + px, 1 + py], label: "$xₜ$" }
```
:::

::: card
Why add and not append? The width stays fixed, so nothing else in the model changes. The model
learns to separate position from content where it needs to. Attention can then use the position,
or ignore it, task by task.
:::

::: card
That leaves one question: how to build $\mathbf{p}_t$. It must map each position $t$ to a
$d$-wide vector with bounded values, give nearby positions similar vectors, and work for any
position, seen in training or not.
:::

::: exercise q1
A token has the embedding $\mathbf{e} = (0.5, -0.2, 0.1)$ and its position has the encoding
$\mathbf{p} = (0, 1, 0)$. What is the input vector $\mathbf{x}$?

::: answer
$(0.5, 0.8, 0.1)$. Add the two vectors coordinate by coordinate.
:::
:::

::: exercise q2
Embeddings are 512 wide. How wide is the input if you append a 512-wide position vector, and how
wide if you add it?

::: answer
1024 if you append it, 512 if you add it.
:::
:::

::: exercise q3
The same word appears at positions 3 and 7. What is $\mathbf{x}_7 - \mathbf{x}_3$?

::: answer
$\mathbf{p}_7 - \mathbf{p}_3$. The embedding is the same, so it cancels.
:::

::: solution
$$ \mathbf{x}_7 - \mathbf{x}_3 = (\mathbf{e} + \mathbf{p}_7) - (\mathbf{e} + \mathbf{p}_3) = \mathbf{p}_7 - \mathbf{p}_3 $$

∎
:::
:::

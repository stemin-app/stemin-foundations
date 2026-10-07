---
title: Where queries, keys and values come from
---

::: card
In a transformer, the queries, keys and values are not given. They are computed from the input
by three learned matrices. Let $\mathbf{X} \in \mathbb{R}^{n \times d_{\text{model}}}$ hold one
token embedding per row. Then

$$ \mathbf{Q} = \mathbf{X}\mathbf{W}^Q, \qquad \mathbf{K} = \mathbf{X}\mathbf{W}^K, \qquad \mathbf{V} = \mathbf{X}\mathbf{W}^V $$

with $\mathbf{W}^Q, \mathbf{W}^K \in \mathbb{R}^{d_{\text{model}} \times d_k}$ and
$\mathbf{W}^V \in \mathbb{R}^{d_{\text{model}} \times d_v}$.
:::

::: card
Each token is projected into three spaces. The **query** projection answers "what am I looking
for?". The **key** projection answers "what do I offer?". The **value** projection answers "what
content do I hand over?". The sizes $d_k$ and $d_v$ are choices: often equal to $d_{\text{model}}$,
and smaller when the computation must shrink.
:::

::: card
Why three matrices and not one vector for all roles? Relevance depends on different features from
content. In "The cat sat on the mat", the query of "sat" can say "I am a verb, I need a subject".
The key of "cat" can say "I am a noun, I can be a subject". The value of "cat" carries what a cat
is. One vector cannot play all three parts at once.
:::

::: card
With one shared vector, the score of a token against itself would be $\mathbf{x}^T\mathbf{x} = \|\mathbf{x}\|^2$,
always large, and every token would attend mostly to itself. Separate $\mathbf{W}^Q$ and
$\mathbf{W}^K$ break this: the score is $\mathbf{x}_i^T\mathbf{W}^Q(\mathbf{W}^K)^T\mathbf{x}_j$,
which need not be largest at $i = j$, and need not be symmetric.
:::

::: exercise q1
$d_{\text{model}} = 512$ and $d_k = d_v = 64$. How many parameters do $\mathbf{W}^Q$,
$\mathbf{W}^K$ and $\mathbf{W}^V$ hold together?

::: answer
98,304. Each is $512 \times 64 = 32{,}768$, and there are three.
:::
:::

::: exercise q2
$\mathbf{X}$ holds 20 tokens with $d_{\text{model}} = 512$, and $d_k = 64$. What is the shape of
$\mathbf{K}$?

::: answer
$20 \times 64$. One key per token, each of size $d_k$.
:::
:::

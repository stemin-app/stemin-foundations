---
title: From tokens to context
---

::: card
A transformer reads a sequence, not a single token. First the text becomes token indices: "The
cat sat" might become $[464, 3797, 3332]$, each number an index into the vocabulary. Then each
index picks its row of the embedding matrix $\mathbf{E}$.
:::

::: card
Stack the rows and you get the input matrix

$$ \mathbf{X} = \begin{bmatrix} \mathbf{e}_{464} \\ \mathbf{e}_{3797} \\ \mathbf{e}_{3332} \end{bmatrix} \in \mathbb{R}^{T \times d} $$

$T = 3$ is the sequence length and $d$ the embedding dimension. Row $t$ is the embedding of token
$t$. With $d = 768$, "The cat sat" becomes a $3 \times 768$ matrix ([Figure](figure:sequence-matrix)).
:::

::: figure sequence-matrix
![From tokens to the matrix X](assets/sequence-matrix.svg)

Each token becomes an index, and each index becomes one row of $\mathbf{X}$. The row of "cat"
is outlined.
:::

::: card
$\mathbf{X}$ is the input to the transformer. The embedding layer's work ends here: it turns
token indices into vectors. The attention layers that follow let the tokens see each other and
build representations that depend on context.
:::

::: card
Word2Vec embeddings are *static*: "bank" has one vector whatever surrounds it. Yet "bank" means
one thing in "river bank" and another in "bank account". A static vector has to blur the two
meanings into one point.
:::

::: card
A transformer's embedding starts static, as the row of $\mathbf{E}$. The attention layers then
turn it into a *contextual embedding*, which depends on the surrounding words. After those layers,
"bank" in "river bank" sits near "shore" and "water", and "bank" in "bank account" sits near
"money" and "finance".
:::

::: card
Studies of trained embedding spaces find structure. Directions often match interpretable
concepts, such as a gender direction, a tense direction or a formality direction. Words with
similar meanings cluster: synonyms lie close together and categories form regions. Word2Vec
style analogies often still work, though contextual vectors make the relation less clean.
:::

::: card
Transformer embeddings are often *anisotropic*: they fill a narrow cone instead of the whole
space. Then cosine similarities run high even for unrelated words. In a cone of half-angle 20°,
no two vectors are more than 40° apart, so every pair has a similarity of at least
$\cos 40° \approx 0.77$. Normalization methods spread the vectors out. Widen the cone and watch
the guaranteed similarity fall.

```plot
x: { var: x, label: "", from: -1.3, to: 1.3 }
y: { label: "", from: -1.3, to: 1.3 }

inputs:
  - { name: h, min: 5, max: 90, default: 20, step: 1, label: "half-angle of the cone, in degrees" }

let:
  r: h * pi() / 180

draw:
  - param: { var: a, over: [0, 2 * pi()], x: cos(a), y: sin(a), dash: true }
  - param: { var: u, over: [0, 1], x: u * cos(r), y: u * sin(r), accent: true }
  - param: { var: u, over: [0, 1], x: u * cos(r), y: -u * sin(r), accent: true }
  - param: { var: u, over: [0, 1], x: u * cos(r / 3), y: u * sin(r / 3) }
  - param: { var: u, over: [0, 1], x: u * cos(r / 2), y: -u * sin(r / 2) }
  - param: { var: a, over: [-r, r], x: 0.3 * cos(a), y: 0.3 * sin(a) }
  - point: { at: [cos(r), sin(r)] }
  - point: { at: [cos(r), -sin(r)] }
```
:::

::: exercise contextual-shape
A sentence of 12 tokens enters a model with $d = 768$. What is the shape of $\mathbf{X}$, and how
many numbers does it hold?

::: answer
$12 \times 768$, which holds 9,216 numbers. One row per token, one column per dimension.
:::
:::

::: exercise contextual-cone
All embeddings of a model lie within 30° of one direction. What is the lowest cosine similarity
two of them can have?

::: answer
0.5. Two vectors are at most 60° apart, and $\cos 60° = 0.5$.
:::
:::

::: exercise contextual-static-count
In a static embedding table, how many vectors does the word "bank" have?

::: answer
One. A static table gives a token the same row in every context.
:::
:::

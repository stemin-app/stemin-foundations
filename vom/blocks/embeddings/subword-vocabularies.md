---
title: Subword vocabularies in practice
---

::: card
A subword vocabulary handles unknown words. A word the model has never met still splits into
pieces it has met. "Transformerize" does not need its own entry: "transform", "er" and "ize"
are enough.
:::

::: card
Related words share pieces, so they share part of their representation. "Playing", "played" and
"player" may all contain the token "play". A model that knows "play" has a start on "playable"
even if that exact word never appeared in training.
:::

::: card
The vocabulary stays a manageable size. Common words get one token each, which is cheap. Rare
words split into several tokens, which costs more computation but keeps every word
representable. A vocabulary of 50,000 subword tokens covers far more than 50,000 words.
:::

::: card
Other algorithms follow the same idea. *WordPiece*, used in BERT, is close to BPE but scores
candidate merges differently. *SentencePiece*, used in T5 and LLaMA, works on the raw text with
no splitting into words first, so it treats every language alike. Each learns a vocabulary of
subword units that balances coverage against size.
:::

::: card
In a transformer, every subword token has a learned row in the embedding matrix
$\mathbf{E} \in \mathbb{R}^{V \times d}$. $V$ is the number of distinct tokens and $d$ is the
model dimension. The embedding layer holds $V \times d$ parameters.
:::

::: card
Real models make this layer large. GPT-2 has $V = 50{,}257$ and $d = 768$, which gives 38.6
million parameters. GPT-3 keeps the vocabulary and uses $d = 12{,}288$, which gives 617.6
million: more than many whole earlier models. On log axes the count is a straight line in $d$.

```plot
x: { var: d, label: "model dimension $d$", from: 100, to: 20000, scale: log }
y: { label: "parameters, millions", from: 1, to: 5000, scale: log }

inputs:
  - { name: vk, min: 10, max: 250, default: 50, step: 1, label: "vocabulary size, in thousands" }

draw:
  - curve: { is: vk * d / 1000, accent: true }
  - point: { at: [768, 38.6], label: "GPT-2" }
  - point: { at: [12288, 617.6], label: "GPT-3" }
```
:::

::: card
The matrix $\mathbf{E}$ starts random and trains with the rest of the model by
[backpropagation](reference:backpropagation). The signal comes from the language modelling task:
predict the next token. Rows that help the prediction stay. The others change.
:::

::: exercise subword-llama-parameters
A model has $V = 32{,}000$ tokens and $d = 4{,}096$. How many parameters does its embedding
matrix hold?

::: answer
131,072,000, about 131 million. Multiply $V$ by $d$.
:::

::: solution
$$ V \times d = 32{,}000 \times 4{,}096 = 131{,}072{,}000 $$

∎
:::
:::

::: exercise subword-double-dimension
You double the model dimension $d$ and keep the vocabulary. By what factor does the number of
embedding parameters change?

::: answer
It doubles. The count $V \times d$ is linear in $d$.
:::
:::

::: exercise subword-memory
GPT-2 stores its 38,597,376 embedding parameters as 32-bit floats, 4 bytes each. How many
megabytes is that, with 1 MB = $10^6$ bytes?

::: answer
About 154 MB. Multiply by 4 bytes.
:::

::: solution
$$ 50{,}257 \times 768 = 38{,}597{,}376 $$

$$ 38{,}597{,}376 \times 4 = 154{,}389{,}504 \text{ bytes} \approx 154 \text{ MB} $$

∎
:::
:::

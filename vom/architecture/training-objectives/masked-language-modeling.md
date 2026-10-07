---
title: Filling in the blanks
---

::: card
An encoder such as BERT does not generate text. It reads a whole input and builds a
representation of it. Its training objective is **masked language modeling** (MLM): hide some
tokens, and predict them from everything around them.
:::

::: card
Pick 15% of the positions at random. Call the set $\mathcal{M}$. Corrupt the tokens there, and
ask the model for the originals:
[Masked language modeling loss](reference:masked-lm-loss)

$$ \mathcal{L}_{MLM} = -\sum_{t \in \mathcal{M}} \log P(x_t \mid \mathbf{x}_{\backslash t}) $$

Here $\mathbf{x}_{\backslash t}$ is the whole sequence with position $t$ corrupted. Only the
positions in $\mathcal{M}$ add to the loss.
:::

::: card
Nothing hides the future here, so attention runs both ways. To fill "The [MASK] sat on the mat",
the model uses "The" on the left and "sat on the mat" on the right.

A decoder sees only the left half of that sentence ([Figure](figure:attention-masks)). Both sides
help when the whole input is known in advance, as in classification or question answering.
:::

::: figure attention-masks
![Causal and bidirectional attention patterns](assets/attention-masks.svg)

A dot marks a position the query may read. The causal pattern is a lower triangle. The
bidirectional pattern is full: the masked token at position 2 reads all five positions.
:::

::: card
A selected position does not always show [MASK]. It shows [MASK] 80% of the time, a random
token 10% of the time, and the original token 10% of the time.

[MASK] teaches prediction from context. The random token stops the model from trusting that only
[MASK] needs a prediction. The unchanged token teaches that any position may need one, which
matters at fine-tuning, where [MASK] never appears.
:::

::: card
Of all the tokens in the input, $0.15 \times 0.8 = 12\%$ show [MASK], $0.15 \times 0.1 = 1.5\%$
show a random token, and $1.5\%$ stay unchanged.

Only that 15% trains. A causal model trains on every position of every sequence, so it gets
more signal from each pass over the data.
:::

::: card
BERT added a second objective, **next sentence prediction** (NSP). Given a pair of sentences
$A$ and $B$, decide whether $B$ really followed $A$:

$$ \mathcal{L}_{NSP} = -\log P(\text{IsNext} \mid \text{[CLS]}, A, \text{[SEP]}, B) $$

The model classifies from the representation of the [CLS] token. Half the pairs are true
neighbours, and half are random.
:::

::: card
NSP turned out to help little, and sometimes to hurt. A random second sentence is usually about a
different subject, so the task reduces to topic detection, not coherence. RoBERTa dropped NSP,
trained with MLM alone, and did better.
:::

::: exercise mlm-thousand-tokens
An input holds 1000 tokens. With the standard masking, how many positions do you expect to show
[MASK], a random token, and the original token while still being predicted?

::: answer
120 show [MASK], 15 show a random token, and 15 stay unchanged. The 150 selected split 80/10/10.
:::

::: solution
Selected: $0.15 \times 1000 = 150$

[MASK]: $0.8 \times 150 = 120$

Random: $0.1 \times 150 = 15$

Unchanged: $0.1 \times 150 = 15$ ∎
:::
:::

::: exercise mlm-unchanged-purpose
Why does MLM leave 10% of the selected tokens unchanged?

::: answer
So the model learns that any position may need a prediction. At fine-tuning there is no [MASK].
:::
:::

::: exercise nsp-chance
A model ignores the input and always gives $P(\text{IsNext}) = 0.5$. What is its NSP loss on every
pair, with the natural log?

::: answer
$\ln 2 \approx 0.693$. The true label gets probability $0.5$ either way.
:::
:::

::: reference masked-lm-loss
# Masked language modeling loss

A masked language model is trained to recover the original tokens at a random set of corrupted
positions, given the rest of the sequence on both sides.

::: equation
\mathcal{L}_{MLM} = -\sum_{t \in \mathcal{M}} \log P(x_t \mid \mathbf{x}_{\backslash t})
:::

::: legend
$\mathcal{L}_{MLM}$: the loss of one sequence, in nats
$\mathcal{M}$: the selected positions, 15% of the sequence
$x_t$: the original token at position $t$
$\mathbf{x}_{\backslash t}$: the sequence with position $t$ corrupted
:::
:::

---
title: The bottleneck, again
---

::: card
A recurrent translator works in two halves. The encoder reads the whole source sentence and
squeezes it into one vector. The decoder writes the translation from that vector alone:

$$ \text{input} \to \text{encoder} \to \text{one vector} \to \text{decoder} \to \text{output} $$

For a long sentence, one vector cannot hold everything. Early words are partly overwritten by
later ones.
:::

::: card
**Attention** removes the squeeze. The decoder keeps every hidden state of the encoder, one per
source word, and at each output step it looks back at all of them. It decides, step by step,
which source words matter for the word it is writing now.
:::

::: card
Translating "the black cat" into French, the decoder writes "le chat noir". To write "chat" it
looks mostly at "cat"; to write "noir" it looks mostly at "black", even though the word order has
flipped. No single vector has to carry the whole sentence: each output step reads what it needs.
:::

::: exercise q1
What does attention give the decoder that a plain encoder and decoder do not?

::: answer
Direct access to every hidden state of the encoder, at every output step, in place of one
summary vector.
:::
:::

::: exercise q2
A source sentence has 40 words. How many encoder states can an attention decoder read at each
step, and how many can a plain recurrent decoder read?

::: answer
40 against 1. The plain decoder sees only the final summary vector.
:::
:::

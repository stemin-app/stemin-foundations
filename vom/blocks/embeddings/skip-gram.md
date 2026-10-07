---
title: Learning from context
---

::: card
Embeddings are learned from text. The idea that makes this possible is the *distributional
hypothesis*: words that appear in similar contexts have similar meanings. You shall know a word
by the company it keeps.
:::

::: card
Fill the blanks:

- "The ___ sat on the mat."
- "The ___ chased the mouse."
- "I fed my ___ some treats."

"Cat", "dog" and "hamster" fit all three, so their vectors should be close. "Democracy" and
"algorithm" fit none, so their vectors should sit elsewhere.
:::

::: card
*Skip-gram*, part of Word2Vec (2013), turns the hypothesis into a training task: from a center
word, predict the words around it. Modern transformers do not use it. They learn their
embeddings as part of the whole model. Skip-gram still shows most clearly how meaning gets into
the vectors.
:::

::: card
A window slides along the text. At each position the middle word is the *center word*, and the
words within $c$ positions of it are its *context words*. In "the cat sat on the mat" with
$c = 2$, the center "sat" has the context "the", "cat", "on", "the"
([Figure](figure:skip-gram-window)).
:::

::: figure skip-gram-window
![The context window around "sat"](assets/skip-gram-window.svg)

With $c = 2$ the window reaches two words to each side of the center. Each arc is one
(center, context) pair the model trains on. "Mat" lies outside the window.
:::

::: card
Skip-gram keeps two embedding matrices, both in $\mathbb{R}^{V \times d}$. $\mathbf{W}$ holds a
word's vector when it is the center, and $\mathbf{W}'$ holds its vector when it is in the
context. Row $i$ of $\mathbf{W}$ is $\mathbf{w}_i$, and row $i$ of $\mathbf{W}'$ is
$\mathbf{w}'_i$.
:::

::: card
The probability that word $w_o$ appears in the context of the center word $w_c$ is a
[softmax](reference:softmax) over the whole vocabulary:

$$ P(w_o \mid w_c) = \frac{\exp(\mathbf{w}'_o{}^T \mathbf{w}_c)}{\sum_{w=1}^{V} \exp(\mathbf{w}'_w{}^T \mathbf{w}_c)} $$

The numerator grows with the dot product of the two vectors. The denominator sums the same
quantity over all $V$ words, so the probabilities add to 1.
:::

::: card
A single high score is not enough when the vocabulary is large. Give the true context word a
dot product $x$ and every other word a dot product of 0. Then
$P = e^x / (e^x + V - 1)$. With $V = 50{,}000$, the probability reaches one half only at
$x = \ln 49{,}999 \approx 10.8$. Move the vocabulary size and watch the curve shift.

```plot
x: { var: x, label: "dot product of the true pair", from: 0, to: 16, ticks: 2, grid: true }
y: { label: "P(context word | centre word)", from: 0, to: 1 }

inputs:
  - { name: vk, min: 1, max: 100, default: 50, step: 1, label: "vocabulary size, in thousands" }

let:
  n: 1000 * vk - 1
  half: log(e(), n)

draw:
  - hline: { at: 0.5, dash: true }
  - curve: { is: exp(x) / (exp(x) + n), accent: true }
  - point: { at: [half, 0.5], label: "one half" }
```
:::

::: card
Training maximizes the [log probability](reference:skip-gram-objective) of the context words
that actually occur:

$$ \mathcal{L} = \sum_{(w_c, w_o) \in D} \log P(w_o \mid w_c) $$

$D$ is the set of all (center, context) pairs taken from the training text. The logarithm turns
a product of probabilities into a sum, which gradient descent handles more easily.
:::

::: card
Here is why this produces good embeddings. If "cat" often appears near "pet", "fur" and "purr",
then $\mathbf{w}_{\text{cat}}$ must have large dot products with $\mathbf{w}'_{\text{pet}}$,
$\mathbf{w}'_{\text{fur}}$ and $\mathbf{w}'_{\text{purr}}$. "Dog" appears near "pet", "fur"
and "bark". Both center vectors are pulled toward the same context vectors, so they end up close
to each other.
:::

::: exercise skip-gram-pairs-on
In "the cat sat on the mat" with $c = 2$, list the (center, context) pairs whose center is "on".

::: answer
(on, cat), (on, sat), (on, the), (on, mat). Take two words to the left and two to the right.
:::
:::

::: exercise skip-gram-pair-count
How many (center, context) pairs does "the cat sat on the mat" give with $c = 1$?

::: answer
10. The two end words have one neighbour each, and the four inner words have two each.
:::

::: solution
The first and the last word each have one neighbour: $2 \times 1 = 2$ pairs.

The four inner words each have two neighbours: $4 \times 2 = 8$ pairs.

$$ 2 + 8 = 10 $$

∎
:::
:::

::: exercise skip-gram-softmax-three
A vocabulary has 3 words. For a center word, the dot products with the three context vectors are
2, 0 and 0. What is $P(w_o \mid w_c)$ for the word with dot product 2?

::: answer
About 0.787. Compute $e^2 / (e^2 + 2)$.
:::

::: solution
$$ P = \frac{e^2}{e^2 + e^0 + e^0} = \frac{7.389}{7.389 + 2} = \frac{7.389}{9.389} \approx 0.787 $$

∎
:::
:::

::: reference skip-gram-objective
# The skip-gram objective

Skip-gram maximizes the log probability of the observed (center, context) pairs, with a softmax
over the vocabulary as the probability of each pair.

::: equation
\mathcal{L} = \sum_{(w_c, w_o) \in D} \log \frac{\exp(\mathbf{w}'_o{}^T \mathbf{w}_c)}{\sum_{w=1}^{V} \exp(\mathbf{w}'_w{}^T \mathbf{w}_c)}
:::

::: legend
$\mathcal{L}$: the objective, maximized during training
$D$: the set of (center, context) pairs from the training text
$\mathbf{w}_c$: the center vector of the center word, a row of $\mathbf{W}$
$\mathbf{w}'_o$: the context vector of the context word, a row of $\mathbf{W}'$
$V$: the vocabulary size
:::

::: derivation
The score of word $w$ as a context of $w_c$ is the dot product $\mathbf{w}'_w{}^T \mathbf{w}_c$.
A softmax turns the $V$ scores into a distribution: $P(w_o \mid w_c) = \exp(\mathbf{w}'_o{}^T \mathbf{w}_c) / \sum_{w} \exp(\mathbf{w}'_w{}^T \mathbf{w}_c)$.[Softmax](reference:softmax)
Treat the pairs as independent: the likelihood of the text is $\prod_{(w_c, w_o) \in D} P(w_o \mid w_c)$.
The logarithm is increasing, so maximizing the likelihood is maximizing its logarithm.
The logarithm of a product is the sum of the logarithms, which gives $\mathcal{L}$. ∎
:::
:::

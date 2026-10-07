---
title: Predicting the next token
---

::: card
A transformer fixes how information flows. It does not fix the weights. To learn them, you need a
**training objective**: one number that says how well the model does, with a gradient that says
how to do better.

The objective of a decoder is the simplest one there is. Read the tokens so far, and predict the
next one.
:::

::: card
Take a sequence of tokens $x_1, x_2, \ldots, x_T$. At each position $t$ the model outputs a
probability distribution over the vocabulary, given the tokens before it. The loss adds the
negative log of the probability it gave to the token that really came next.
[Language modeling loss](reference:language-modeling-loss)

$$ \mathcal{L}_{LM} = -\sum_{t=1}^{T} \log P(x_t \mid x_1, \ldots, x_{t-1}) $$

Each term is a [cross-entropy](reference:cross-entropy) against a one-hot target. A lower loss
means more probability on the true next tokens.
:::

::: card
No one labels the data. The text is its own answer key: every token is the target for the
prefix before it. This is **self-supervised** learning.

Given "The cat sat on the", the model learns that "mat" is a likely continuation and "elephant"
is not. Over billions of such predictions it picks up syntax, meaning, facts and patterns of
reasoning.
:::

::: card
The model must not see the answer. The [causal mask](reference:causal-mask) in decoder attention
lets the prediction for position $t$ use positions $1, \ldots, t-1$ only. So
$P(x_t \mid x_1, \ldots, x_{t-1})$ depends only on legitimate context, and all $T$ predictions of
a sequence train in one forward pass.
:::

::: card
The network does not output probabilities. It outputs **logits** $z_1, \ldots, z_V$, one per
vocabulary entry, and the [softmax](reference:softmax) turns them into $q_i$. Write the loss of one
position, with correct token $y$, in terms of the logits:

$$ -\log q_y = -z_y + \log \sum_{j=1}^{V} \exp(z_j) $$

The first term rewards a high logit for the correct token. The second term, the **log-sum-exp**,
punishes large logits everywhere, so the model cannot raise them all at once.
:::

::: card
Gather every wrong logit into one number, $s = \log \sum_{j \neq y} \exp(z_j)$. The loss of the
position becomes $\log(1 + e^{s - z_y})$. Raise $z_y$ and the loss falls toward zero but never
reaches it. When $z_y = s$ the correct token holds half the probability and the loss is
$\log 2 \approx 0.693$.

```plot
x: { var: z, label: "$zᵧ$", from: -4, to: 10, ticks: 2, grid: true }
y: { label: "loss", from: 0, to: 10 }

inputs:
  - { name: s, min: 0, max: 6, default: 2, step: 0.5, label: "the wrong logits s" }

draw:
  - curve: { is: "log(e(), 1 + exp(s - z))", accent: true }
  - vline: { at: s, dash: true }
  - point: { at: [s, "log(e(), 2)"], label: "$qᵧ = 1/2$" }
```
:::

::: card
On a computer, $\exp(1000)$ overflows. Subtract the largest logit $m = \max_j z_j$ first. The
constant comes out of the sum unchanged:

$$ \log \sum_j \exp(z_j) = m + \log \sum_j \exp(z_j - m) $$

Every exponent is now zero or negative, so nothing overflows. Libraries fuse the softmax and the
log into one stable `log_softmax` for this reason.
:::

::: card
During training the model reads the **true** previous tokens, never its own guesses. This is
**teacher forcing**. It keeps the gradients stable, and it lets all positions train in parallel.

The cost is a mismatch. At inference the model feeds on its own output, mistakes included.
Scheduled sampling mixes in the model's own predictions during training, but transformers rarely
use it: the mismatch is the accepted price of stable training.
:::

::: card
Train this one objective at scale, on hundreds of billions of tokens of books, web pages and
code, and the model starts to do things it was never trained for. It follows a task from an
instruction alone (zero-shot). It learns a new task from a few examples in its context
(few-shot). It solves a problem in steps (chain of thought).

Predicting text well requires a model of what the text is about. Quality of data matters as
much as quantity: filtering, deduplication and a balance of sources all improve the result.
:::

::: exercise lm-three-tokens
A model gives the true next tokens of a three-token sequence the probabilities $0.9$, $0.6$ and
$0.3$. What is $\mathcal{L}_{LM}$, with the natural log?

::: answer
$\mathcal{L}_{LM} \approx 1.820$. Add the negative logs of the three probabilities.
:::

::: solution
$-\ln 0.9 = 0.1054$

$-\ln 0.6 = 0.5108$

$-\ln 0.3 = 1.2040$

$\mathcal{L}_{LM} = 0.1054 + 0.5108 + 1.2040 = 1.8202$ ∎
:::
:::

::: exercise lm-logit-loss
The logits over a three-token vocabulary are $z = (2, 1, 0)$, and the correct token is the first.
What is the loss of this position?

::: answer
$\approx 0.408$. Use $-z_y + \log \sum_j \exp(z_j)$.
:::

::: solution
$\sum_j \exp(z_j) = e^2 + e^1 + e^0 = 7.389 + 2.718 + 1 = 11.107$

$\ln 11.107 = 2.4076$

$-z_y + 2.4076 = -2 + 2.4076 = 0.4076$

Check: $q_y = 7.389 / 11.107 = 0.665$ and $-\ln 0.665 = 0.408$. ∎
:::
:::

::: exercise lm-stable-lse
Compute $\log(e^{1000} + e^{999})$ without overflow.

::: answer
$\approx 1000.313$. Pull out $m = 1000$ first.
:::

::: solution
$m = 1000$

$\log(e^{1000} + e^{999}) = 1000 + \log(e^{0} + e^{-1})$

$= 1000 + \ln(1 + 0.3679) = 1000 + 0.3133 = 1000.3133$ ∎
:::
:::

::: reference language-modeling-loss
# Language modeling loss

A causal language model is trained to minimize the negative log-likelihood of each token given
the tokens before it.

::: equation
\mathcal{L}_{LM} = -\sum_{t=1}^{T} \log P(x_t \mid x_1, \ldots, x_{t-1})
:::

::: legend
$\mathcal{L}_{LM}$: the loss of one sequence, in nats
$x_t$: the token at position $t$
$T$: the length of the sequence
$P$: the probability the model assigns
:::

::: derivation
Goal: the negative log-likelihood of the whole sequence.
Factor the joint probability by conditioning on the prefix: $P(x_1, \ldots, x_T) = \prod_{t=1}^{T} P(x_t \mid x_1, \ldots, x_{t-1})$.[Conditional probability](reference:conditional-probability)
Take the negative log: $-\log P(x_1, \ldots, x_T) = -\sum_{t=1}^{T} \log P(x_t \mid x_1, \ldots, x_{t-1})$.
Each term is the cross-entropy of a one-hot target $x_t$ and the model's distribution.[Cross-entropy](reference:cross-entropy)
Minimizing the sum maximizes the likelihood of the sequence. ∎
:::
:::

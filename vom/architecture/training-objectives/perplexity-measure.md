---
title: Perplexity
---

::: card
A loss of 2.3 nats per token is hard to picture. Language models report **perplexity** instead:
the exponential of the mean loss per token.
[Perplexity](reference:perplexity)

$$ \text{PPL} = \exp\left(\frac{1}{T} \sum_{t=1}^{T} -\log P(x_t \mid x_1, \ldots, x_{t-1})\right) = \exp\left(\frac{\mathcal{L}_{LM}}{T}\right) $$
:::

::: card
Perplexity counts choices. Suppose the model spreads its probability evenly over $N$ tokens at
every position, the true one among them. Each term is $-\log \frac{1}{N} = \log N$, the mean loss
is $\log N$, and the perplexity is $e^{\log N} = N$.

So a perplexity of 10 means the model is as uncertain as a fair choice among 10 tokens, on
average. Lower is better, and 1 is perfect.
:::

::: card
Drag the mean loss $\bar{\ell} = \mathcal{L}_{LM} / T$. A loss of $\ln 10 \approx 2.303$ nats gives
a perplexity of 10. Each extra nat multiplies the perplexity by $e \approx 2.718$, so small gains
in loss at the low end are large gains in perplexity.

```plot
x: { var: l, label: "mean loss $ℓ̄$ in nats", from: 0, to: 5, ticks: 1, grid: true }
y: { label: "PPL", from: 0, to: 150 }

inputs:
  - { name: l0, min: 0, max: 5, default: 2.3, step: 0.1, label: "the mean loss" }

draw:
  - curve: exp(l)
  - vline: { at: l0, dash: true }
  - hline: { at: exp(l0), dash: true }
  - point: { at: [l0, exp(l0)], label: "exp(ℓ̄)" }
```
:::

::: card
The exponential of a mean of logs is a geometric mean. So perplexity is the geometric mean of the
inverse probabilities the model gave the true tokens:

$$ \text{PPL} = \left(\prod_{t=1}^{T} \frac{1}{P(x_t \mid x_1, \ldots, x_{t-1})}\right)^{1/T} $$

One token given probability $0.001$ multiplies the product by 1000. Perplexity punishes a
confident miss hard.
:::

::: card
The base of the log does not matter, as long as the exponential matches it. A loss measured in
bits gives $\text{PPL} = 2^{\bar{\ell}_2}$. A loss in nats gives $e^{\bar{\ell}}$. Both describe
the same model with the same number.
:::

::: card
An untrained model that spreads its probability evenly over a vocabulary of $V = 50{,}000$ tokens
has a mean loss of $\ln 50{,}000 \approx 10.82$ nats, and a perplexity of 50,000. Training drives
the number down from the size of the vocabulary toward 1. It never goes below 1: no probability
exceeds 1, so no term of the loss is negative.
:::

::: exercise ppl-three-tokens
A model gives the true tokens of a three-token text the probabilities $0.5$, $0.25$ and $0.125$.
What is its perplexity on the text?

::: answer
$\text{PPL} = 4$. Take the geometric mean of $2$, $4$ and $8$.
:::

::: solution
$\mathcal{L}_{LM} = \ln 2 + \ln 4 + \ln 8 = 6 \ln 2 = 4.159$

$\mathcal{L}_{LM} / T = 2 \ln 2 = 1.386$

$\text{PPL} = e^{2 \ln 2} = 4$

Check: $(2 \times 4 \times 8)^{1/3} = 64^{1/3} = 4$. ∎
:::
:::

::: exercise ppl-from-bits
A model has a mean loss of 3 bits per token. What is its perplexity?

::: answer
$\text{PPL} = 8$. In bits, $\text{PPL} = 2^{\bar{\ell}_2}$.
:::
:::

::: exercise ppl-to-loss
A model has a perplexity of 20. What is its mean loss per token, in nats?

::: answer
$\bar{\ell} = \ln 20 \approx 2.996$ nats. Invert the exponential.
:::
:::

::: reference perplexity
# Perplexity

Perplexity is the exponential of the mean negative log-likelihood per token. A model that chooses
evenly among $N$ tokens at every position has perplexity $N$.

::: equation
\text{PPL} = \exp\left(\frac{\mathcal{L}_{LM}}{T}\right) = \left(\prod_{t=1}^{T} \frac{1}{P(x_t \mid x_{<t})}\right)^{1/T}
:::

::: legend
$\text{PPL}$: the perplexity, a pure number of at least 1
$\mathcal{L}_{LM}$: the language modeling loss of the sequence, in nats
$T$: the number of tokens
$P(x_t \mid x_{<t})$: the probability of the true token given the tokens before it
:::

::: derivation
Goal: the product form, and the meaning of the number.
Start from the loss: $\mathcal{L}_{LM} = -\sum_t \log P(x_t \mid x_{<t})$.[Language modeling loss](reference:language-modeling-loss)
Divide by $T$ and exponentiate: $\exp\left(\frac{1}{T} \sum_t \log \frac{1}{P(x_t \mid x_{<t})}\right) = \left(\prod_t \frac{1}{P(x_t \mid x_{<t})}\right)^{1/T}$.
Set every $P(x_t \mid x_{<t}) = \frac{1}{N}$: the product is $N^T$, and its $T$-th root is $N$.
Every $P \le 1$, so every factor is at least 1, and $\text{PPL} \ge 1$. ∎
:::
:::

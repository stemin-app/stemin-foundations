---
title: Negative sampling
---

::: card
The skip-gram softmax has a cost problem. Its denominator sums over the whole vocabulary:

$$ \sum_{w=1}^{V} \exp(\mathbf{w}'_w{}^T \mathbf{w}_c) $$

With $V = 50{,}000$ words, that is 50,000 dot products for every single training pair. Training
on a large corpus this way is far too slow.
:::

::: card
*Negative sampling* changes the question. The softmax asks which of the 50,000 words is the
context word, a classification with $V$ classes. Negative sampling asks whether one given word
is a real context word or a random fake, a classification with 2 classes.
:::

::: card
For each real (center, context) pair in the corpus, you also draw $k$ random words that are not
in the context. The real context word is the *positive sample*. The random words are the
*negative samples*. The model learns to say yes to the positive and no to each negative.
:::

::: card
For the center word $w_c$, the real context word $w_o$ and the negative words
$w_1, \ldots, w_k$, the model [maximizes](reference:skip-gram-negative-sampling)

$$ \mathcal{L} = \log \sigma(\mathbf{w}'_o{}^T \mathbf{w}_c) + \sum_{i=1}^k \log \sigma(-\mathbf{w}'_{w_i}{}^T \mathbf{w}_c) $$

Here $\sigma(x) = \frac{1}{1 + e^{-x}}$ is the [sigmoid](reference:sigmoid), which squashes any
real number into a probability between 0 and 1.
:::

::: card
The first term handles the positive sample. Its dot product goes into $\sigma$, and training
wants the output near 1: yes, a real context word.

For a negative sample the dot product enters with a minus sign, because
$\sigma(-x) = 1 - \sigma(x)$ is the probability of "no". Drag the dot product. A large positive
value suits a real pair, a large negative value suits a fake one.

```plot
x: { var: x, label: "the dot product", from: -6, to: 6, ticks: 1, grid: true }
y: { label: "probability", from: 0, to: 1 }

inputs:
  - { name: z, min: -6, max: 6, default: 1.5, step: 0.1, label: "the dot product" }

draw:
  - hline: { at: 0.5, dash: true }
  - curve: { is: 1 / (1 + exp(-x)), accent: true, label: "$σ(x)$: real" }
  - curve: { is: 1 / (1 + exp(x)), dash: true, label: "$σ(-x)$: fake" }
  - vline: { at: z, dash: true }
  - point: { at: [z, 1 / (1 + exp(-z))] }
  - point: { at: [z, 1 / (1 + exp(z))] }
```
:::

::: card
The learning signal matches the softmax. To raise $\sigma(\mathbf{w}'_o{}^T \mathbf{w}_c)$,
training makes the real context vector point along the center vector. To raise
$\sigma(-\mathbf{w}'_{w_i}{}^T \mathbf{w}_c)$, it makes each random word's dot product small or
negative. Real context words get pulled in, random words get pushed away.
:::

::: card
The cost falls from $V$ dot products to $k + 1$. Typically $k$ is between 5 and 20. With
$k = 5$ and $V = 50{,}000$, a training pair needs 6 dot products instead of 50,000.
:::

::: exercise negative-sampling-cost
With $k = 10$ negative samples and $V = 50{,}000$, how many dot products does one training pair
need, against the full softmax?

::: answer
11 against 50,000, about 4,500 times fewer. Count the positive sample and the $k$ negatives.
:::

::: solution
$$ k + 1 = 10 + 1 = 11 $$

$$ \frac{50{,}000}{11} \approx 4{,}545 $$

∎
:::
:::

::: exercise negative-sampling-sigmoid-flip
$\sigma(2) \approx 0.881$. What is $\sigma(-2)$?

::: answer
About 0.119. Use $\sigma(-x) = 1 - \sigma(x)$.
:::

::: solution
$$ \sigma(-x) = \frac{1}{1 + e^{x}} = \frac{e^{-x}}{e^{-x} + 1} = 1 - \frac{1}{1 + e^{-x}} = 1 - \sigma(x) $$

$$ \sigma(-2) = 1 - 0.881 = 0.119 $$

∎
:::
:::

::: exercise negative-sampling-objective-value
With $k = 1$, the positive pair has dot product 2 and the negative pair has dot product $-1$.
What is $\mathcal{L}$, with the natural logarithm, to three decimals?

::: answer
About $-0.440$. Compute $\log \sigma(2) + \log \sigma(1)$.
:::

::: solution
The negative term uses $-(-1) = 1$.

$$ \mathcal{L} = \log \sigma(2) + \log \sigma(1) $$

$$ \sigma(2) \approx 0.8808 \qquad \sigma(1) \approx 0.7311 $$

$$ \mathcal{L} \approx -0.1269 - 0.3133 = -0.440 $$

∎
:::
:::

::: reference skip-gram-negative-sampling
# Skip-gram with negative sampling

For one observed pair and $k$ random negative words, skip-gram with negative sampling maximizes
the log probability that a binary classifier labels the real pair as real and each fake pair as
fake.

::: equation
\mathcal{L} = \log \sigma(\mathbf{w}'_o{}^T \mathbf{w}_c) + \sum_{i=1}^k \log \sigma(-\mathbf{w}'_{w_i}{}^T \mathbf{w}_c)
:::

::: legend
$\mathcal{L}$: the objective for one pair, maximized during training
$\sigma$: the sigmoid, $\sigma(x) = 1 / (1 + e^{-x})$
$\mathbf{w}_c$: the center vector of the center word
$\mathbf{w}'_o$: the context vector of the real context word
$\mathbf{w}'_{w_i}$: the context vector of the negative word $w_i$
$k$: the number of negative samples, typically 5 to 20
:::

::: derivation
Model the probability that a pair is real as $P(\text{real} \mid w, w_c) = \sigma(\mathbf{w}'_w{}^T \mathbf{w}_c)$.[Sigmoid](reference:sigmoid)
Then $P(\text{fake} \mid w, w_c) = 1 - \sigma(\mathbf{w}'_w{}^T \mathbf{w}_c) = \sigma(-\mathbf{w}'_w{}^T \mathbf{w}_c)$.
The data: the pair $(w_c, w_o)$ is real, and each pair $(w_c, w_i)$ is fake.
Treat the $k + 1$ labels as independent: the likelihood is $\sigma(\mathbf{w}'_o{}^T \mathbf{w}_c) \prod_{i=1}^k \sigma(-\mathbf{w}'_{w_i}{}^T \mathbf{w}_c)$.
Take the logarithm of the product: the sum $\mathcal{L}$. ∎
:::
:::

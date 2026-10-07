---
title: From vectors to tokens
---

::: card
The decoder ends with one row of $d_{model}$ numbers per target position. A linear layer turns
each row into $V$ scores, the **logits**:

$$ \mathbf{L} = \mathbf{Y}_{dec}\mathbf{W}_{out} + \mathbf{1}_m\mathbf{b}_{out}^T \in \mathbb{R}^{m \times V} $$

Here $\mathbf{W}_{out} \in \mathbb{R}^{d_{model} \times V}$, and the same bias
$\mathbf{b}_{out} \in \mathbb{R}^V$ is added to every row.
:::

::: card
A softmax along each row turns logits into probabilities.[Softmax](reference:softmax)

$$ P_{ij} = \frac{\exp(L_{ij})}{\sum_{k=1}^{V}\exp(L_{ik})} $$

$P_{ij}$ is the probability that token $j$ comes next after position $i$. Each row of
$\mathbf{P} \in \mathbb{R}^{m \times V}$ sums to 1.
:::

::: card
Let $y_i$ be the true next token at position $i$. The loss is the cross-entropy, averaged over
the $m$ positions.[Cross-entropy](reference:cross-entropy)

$$ \mathcal{L} = -\frac{1}{m}\sum_{i=1}^{m}\log P_{i, y_i} $$

Since $0 < P_{i,y_i} \le 1$, each $\log P_{i,y_i} \le 0$, and the minus sign makes the loss
positive. It falls to 0 only when every true token gets probability 1.[Token prediction loss](reference:token-prediction-loss)
:::

::: card
Hold every wrong logit at 0 and raise the logit $z$ of the true token. At $z = 0$ all $V$ tokens
tie, and the loss is $\ln V$: $10.82$ for $V = 50000$. That is roughly where an untrained model
starts. The loss reaches $\ln 2$ when $z = \ln(V - 1)$, where the true token holds half the
mass. Shrink the vocabulary and watch the starting loss fall.

```plot
x: { var: z, label: "logit $z$ of the true token", from: -5, to: 20, ticks: 5, grid: true }
y: { label: "loss $-ln p$", from: 0, to: 16, ticks: 2 }

inputs:
  - { name: k, min: 0.3, max: 5, default: 4.7, step: 0.1, label: "vocabulary size, as log10 of V" }

let:
  V: 10^k
  loss: log(e(), exp(z) + V - 1) - z
  lnV: log(e(), V)
  zhalf: log(e(), V - 1)
  ln2: log(e(), 2)

draw:
  - curve: { is: loss, accent: true }
  - point: { at: [0, lnV], label: "$ln V$" }
  - point: { at: [zhalf, ln2], label: "$p = 1/2$" }
```
:::

::: card
Training and inference run the same network with the same masks. What changes is around it:

- the weights are updated by backpropagation in training, and fixed at inference;
- training reads complete sequences with known targets; inference reads what it has so far;
- training returns a loss; inference returns tokens;
- training feeds the true previous tokens (teacher forcing); inference feeds its own output.
:::

::: card
At inference the decoder writes one token at a time. Read the distribution at the last
position, then take its most probable token (greedy decoding) or sample from it. Append the
token to the input and run again. The masked computation is the one the model learned in
training.
:::

::: exercise three-position-loss
At three target positions the model gives the true token probability $0.5$, $0.25$ and $0.8$.
What is the loss $\mathcal{L}$?

::: answer
$\mathcal{L} = 0.768$. Average the three values of $-\ln P$.
:::

::: solution
$$ \mathcal{L} = -\tfrac{1}{3}\big(\ln 0.5 + \ln 0.25 + \ln 0.8\big) $$

$$ = \tfrac{1}{3}(0.693 + 1.386 + 0.223) = \tfrac{1}{3}(2.303) = 0.768 $$

Check: $0.5 \times 0.25 \times 0.8 = 0.1$, and $-\ln 0.1 = 2.303$. ∎
:::
:::

::: exercise uniform-start-loss
A model with $V = 32000$ tokens gives every token the same probability. What is its loss?

::: answer
$\ln 32000 = 10.37$. Each true token gets $1/V$.
:::
:::

::: exercise output-layer-size
The base model has $d_{model} = 512$ and $V = 50000$, and the target has $m = 5$ tokens. How many
weights does $\mathbf{W}_{out}$ hold, and how many logits does one forward pass produce?

::: answer
$25\,600\,000$ weights and $250\,000$ logits. $\mathbf{W}_{out}$ is $d_{model} \times V$; the
logits are $m \times V$.
:::

::: solution
$$ 512 \times 50000 = 25\,600\,000 $$

$$ 5 \times 50000 = 250\,000 $$

∎
:::
:::

::: reference token-prediction-loss
# The token prediction loss

The transformer scores a target sequence by the mean negative log probability it gives the
true next token at each position.

::: equation
\mathcal{L} = -\frac{1}{m}\sum_{i=1}^{m}\log P_{i, y_i}, \qquad P_{ij} = \frac{\exp(L_{ij})}{\sum_{k=1}^{V}\exp(L_{ik})}
:::

::: legend
$m$: the number of target positions
$y_i$: the index of the true next token at position $i$
$L_{ij}$: the logit of token $j$ at position $i$
$P_{ij}$: the probability of token $j$ at position $i$
$V$: the vocabulary size
$\log$: the natural logarithm
:::

::: derivation
Goal: the loss is the cross-entropy between the one-hot target and the prediction, averaged over positions.
At position $i$ the target puts probability 1 on $y_i$ and 0 elsewhere.
The cross-entropy of that target with $\mathbf{p}_i$ is $-\sum_j \delta_{j, y_i}\log P_{ij}$.[Cross-entropy](reference:cross-entropy)
Only the term $j = y_i$ survives: $-\log P_{i,y_i}$.
The mean over the $m$ positions gives $\mathcal{L}$.
With all logits equal, $P_{i,y_i} = 1/V$ and $\mathcal{L} = \ln V$. ∎
:::
:::

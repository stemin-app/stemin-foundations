---
title: What a transformer costs
---

::: card
Count the work of one layer on $n$ tokens. Self-attention scores every pair: $O(n^2 d_{model})$.
Cross-attention scores $m$ target rows against $n$ source rows: $O(m\,n\,d_{model})$. The FFN works
on each row alone: $O(n\,d_{model}\,d_{ff})$. The projections of attention add
$O(n\,d_{model}^2)$.
:::

::: card
Which term wins depends on the length. Per token, the pairwise part grows with $n$, the rest does
not. Count multiplications for one layer with $d_{ff} = 4d$: the four projections take $4nd^2$,
the FFN $8nd^2$, and the scores with the weighted sum $2n^2d$. The two sides meet at $n = 6d$.
Below that, the matrices of the layer dominate; above it, attention does.

```plot
x: { var: n, label: "sequence length n", from: 100, to: 100000, scale: log, ticks: 1, grid: true }
y: { label: "multiplications per layer", from: 1e8, to: 1e16, scale: log, ticks: 1 }

inputs:
  - { name: d, min: 256, max: 4096, default: 512, step: 256, label: "model dimension d" }

draw:
  - curve: { is: 12 * n * d ^ 2, label: "projections and FFN" }
  - curve: { is: 2 * n ^ 2 * d, accent: true, label: "attention scores" }
  - vline: { at: 6 * d, dash: true }
```
:::

::: card
Memory follows the same lines. Training keeps every attention matrix for the backward pass:
$n^2$ numbers per head per layer, so $O(n^2 h N)$. The activations take $O(n\,d_{model} N)$. The
weights take $O(d_{model}^2 N + V d_{model})$.
:::

::: card
Count the weights of the base model, with $d_{model} = 512$, $d_{ff} = 2{,}048$, $N = 6$ and
biases included. An encoder layer holds 3,152,384: attention $4d^2 + 4d$, the FFN
$2d\,d_{ff} + d_{ff} + d$, and two norms. A decoder layer, with a second attention and a third
norm, holds 4,204,032. Six of each make about 44.1 million.
:::

::: card
Each vocabulary matrix, $V \times d_{model}$ with $V = 50{,}000$, holds 25.6 million. With separate
source embeddings, target embeddings and output projection, the total is about 121 million. The
original paper shares one matrix for all three, with a vocabulary of about 37,000 tokens, and
reaches about 65 million.
:::

::: card
The pattern scales. A layer holds about $12\,d_{model}^2$ weights, so a decoder-only model holds
about $12\,N\,d_{model}^2$. GPT-3 has $d_{model} = 12{,}288$ and $N = 96$:
$12 \times 96 \times 12{,}288^2 \approx 174$ billion, plus 0.6 billion for its embeddings, close to
its 175 billion.

```plot
x: { var: d, label: "model dimension d", from: 256, to: 16384, scale: log, grid: true }
y: { label: "parameters, billions", from: 0.001, to: 1000, scale: log }

inputs:
  - { name: L, min: 6, max: 128, default: 96, step: 2, label: "number of layers N" }

draw:
  - curve: { is: 12 * L * d ^ 2 / 1e9, accent: true }
  - point: { at: [12288, 175], label: "GPT-3" }
  - point: { at: [768, 0.085], label: "GPT-2 small" }
```
:::

::: exercise q1
With $d = 1{,}024$, at what sequence length do the attention scores cost as much as the
projections and the FFN of a layer?

::: answer
6,144 tokens. The two sides meet at $n = 6d$.
:::

::: solution
$2n^2 d = 12 n d^2 \implies n = 6d = 6 \times 1{,}024 = 6{,}144$. ∎
:::
:::

::: exercise q2
Estimate the weights of a decoder-only model with $d_{model} = 4{,}096$ and $N = 32$, embeddings
left out.

::: answer
About 6.4 billion. $12 \times 32 \times 4{,}096^2 \approx 6.44 \times 10^9$.
:::
:::

::: exercise q3
A model with $h = 16$ heads and $N = 24$ layers reads $n = 2{,}048$ tokens. How many attention
weights does training keep for the backward pass?

::: answer
About 1.6 billion. $n^2 h N = 2{,}048^2 \times 16 \times 24 = 1{,}610{,}612{,}736$.
:::
:::

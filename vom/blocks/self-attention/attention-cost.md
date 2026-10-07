---
title: The quadratic cost
---

::: card
**Computational complexity** says how the work grows with the size of the input. Big-O notation
keeps only the growth: $O(n)$ grows in proportion to $n$, $O(n^2)$ with its square. Ten times
more tokens costs an $O(n)$ method ten times the work, and an $O(n^2)$ method a hundred times.
:::

::: card
Self-attention costs $O(n^2 \cdot d)$ for $n$ tokens of dimension $d$. The product
$\mathbf{Q}\mathbf{K}^T$ multiplies an $n \times d$ matrix by a $d \times n$ one: $n^2$ dot
products of length $d$. The softmax touches all $n^2$ scores once. The product $\mathbf{A}\mathbf{V}$
is again $n \times n$ times $n \times d$: another $n^2 \cdot d$.
[Self-attention cost](reference:self-attention-cost)
:::

::: card
The $n^2$ hurts. At $n = 1{,}000$ the model computes and stores $10^6$ scores. At $n = 10{,}000$
it is $10^8$. This is why early GPT models stopped at context windows of 1,024 or 2,048 tokens.
:::

::: card
Efficient attention methods attack the $n^2$. Linear attention approximates the softmax, sparse
transformers score only some pairs, and FlashAttention computes the exact result in a different
order so the full score matrix never sits in slow memory. The basic transformer pays the full
quadratic cost.
:::

::: card
Recurrence pays differently. An RNN does $n$ steps, each a $d \times d$ matrix times a vector:
$O(n \cdot d^2)$.[RNN update](reference:rnn-update)

$$ \begin{array}{lcc}
 & \text{self-attention} & \text{RNN} \\
\text{work per layer} & O(n^2 \cdot d) & O(n \cdot d^2) \\
\text{sequential steps} & O(1) & O(n) \\
\text{longest path} & O(1) & O(n)
\end{array} $$
:::

::: card
Which is cheaper depends on whether $n$ or $d$ is larger. The two costs meet where
$n^2 d = n d^2$, at $n = d$. Shorter sequences favour attention; longer ones favour the RNN.
Both axes are logarithmic: each tick is a power of ten.

```plot
x: { var: n, label: "sequence length $n$", from: 10, to: 100000, scale: log, ticks: 1, grid: true }
y: { label: "operations", from: 1000, to: 100000000000000, scale: log, ticks: 1 }

inputs:
  - { name: d, min: 64, max: 4096, default: 512, step: 64, label: "model dimension d" }

draw:
  - curve: { is: n^2 * d, accent: true, label: "self-attention" }
  - curve: { is: n * d^2, label: "RNN" }
  - point: { at: [d, d^3], label: "$n = d$" }
```
:::

::: card
The path length is the number of steps information takes between two positions. In an RNN, word
1 reaches word $n$ through $n - 1$ hidden states, each a transformation that can lose or distort
it. Gradients make the same trip backwards and can vanish or explode. In self-attention every
pair is one step apart.
:::

::: card
Parallelism is the other gain. An RNN cannot compute $\mathbf{h}_t$ before $\mathbf{h}_{t-1}$,
so 1,000 tokens take 1,000 steps in a row. Self-attention computes every position at once as a
few matrix products, the operation a GPU does fastest.
:::

::: exercise doubling-context
A model's context grows from 1,024 to 2,048 tokens. By what factor does the number of attention
scores grow?

::: answer
4. The score count is $n^2$.
:::

::: solution
$$ \frac{2048^2}{1024^2} = \left(\frac{2048}{1024}\right)^2 = 2^2 = 4 $$

∎
:::
:::

::: exercise score-count-4096
How many attention scores does one head compute for a sequence of 4,096 tokens?

::: answer
16,777,216. That is $4096^2$.
:::

::: solution
$$ 4096^2 = 2^{24} = 16{,}777{,}216 $$

∎
:::
:::

::: exercise attention-rnn-crossover
With $d = 512$, at what sequence length does $n^2 d$ equal $n d^2$?

::: answer
$n = 512$. Divide both sides by $n d$.
:::

::: solution
$$ n^2 d = n d^2 \implies n = d = 512 $$

∎
:::
:::

::: reference self-attention-cost
# Self-attention cost

The work of one self-attention layer grows with the square of the sequence length and linearly
with the width.

::: equation
\text{work} = O(n^2 \cdot d), \qquad \text{scores stored} = n^2
:::

::: legend
$n$: the number of tokens
$d$: the width of the queries, keys and values
:::

::: derivation
$\mathbf{Q}\mathbf{K}^T$ is $(n \times d)(d \times n)$: $n^2$ entries, each a dot product of length $d$, so $n^2 d$ multiplications.[Matrix product](reference:matrix-product)
Softmax visits each of the $n^2$ scores once: $O(n^2)$.
$\mathbf{A}\mathbf{V}$ is $(n \times n)(n \times d)$: $n d$ entries, each a sum of $n$ products, so $n^2 d$ multiplications.
Total: $2 n^2 d + O(n^2) = O(n^2 d)$. ∎
:::
:::

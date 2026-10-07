---
title: Why each head is narrower
---

::: card
Why not use 8 heads that are each 512 wide? Because each full-width head would cost as much as the
single head it replaces, and 8 of them would cost 8 times as much. Narrow heads keep two costs
flat: the compute and the number of parameters.
:::

::: card
Start with the scores. One head of width 512 scores a pair of tokens with one dot product of 512
terms. Eight heads of width 64 score the same pair with 8 dot products of 64 terms: $8 \times 64
= 512$ multiplications. The work is the same.
:::

::: card
Now count parameters, with $d_\text{model} = 512$. Each head holds three $512 \times 64$ matrices,
and $\mathbf{W}^O$ is $512 \times 512$.

$$ 8 \times 3 \times 512 \times 64 + 512^2 = 786{,}432 + 262{,}144 = 1{,}048{,}576 $$

That is $4 \times 512^2$, the count of one head of width 512 with its own output
matrix.[Parameter count](reference:multi-head-parameter-count)
:::

::: card
Make each head full width and the count grows with $h$. Each head then holds $3 \times 512^2$,
and $\mathbf{W}^O$ grows to $(512h) \times 512$, so the total is $4h \times 512^2$. Slide the
number of heads: narrow heads stay at $4\, d_\text{model}^2$, full-width heads climb.

```plot
x: { var: h, label: "number of heads $h$", from: 1, to: 16, ticks: 1, grid: true }
y: { label: "parameters in units of $d²$", from: 0, to: 68 }

draw:
  - curve: { is: 4, accent: true, label: "narrow heads" }
  - curve: { is: 4 * h, dash: true, label: "full-width heads" }
  - point: { at: [8, 32], label: "8 heads: 32" }
  - point: { at: [8, 4], label: "4" }
```
:::

::: card
So the extra heads come free. With the same compute and the same parameters as one wide head, the
layer computes $h$ separate attention patterns in place of one.
:::

::: exercise q1
A layer has $d_\text{model} = 768$ and $h = 12$ heads of width 64, with no biases. How many
parameters do its four kinds of matrix hold in total?

::: answer
2,359,296. The total is $4\, d_\text{model}^2$, whatever $h$ is.
:::

::: solution
$$ 12 \times 3 \times 768 \times 64 = 1{,}769{,}472 $$

$$ 768^2 = 589{,}824 $$

$$ 1{,}769{,}472 + 589{,}824 = 2{,}359{,}296 = 4 \times 768^2 $$

∎
:::
:::

::: exercise q2
A layer with $d_\text{model} = 512$ uses $h = 4$ heads that are each 512 wide. How many parameters
does it hold, with no biases?

::: answer
4,194,304. Full-width heads cost $4h\, d_\text{model}^2 = 16 \times 512^2$.
:::

::: solution
$$ \text{projections: } 4 \times 3 \times 512^2 = 12 \times 262{,}144 = 3{,}145{,}728 $$

$$ \mathbf{W}^O: (4 \times 512) \times 512 = 4 \times 262{,}144 = 1{,}048{,}576 $$

$$ 3{,}145{,}728 + 1{,}048{,}576 = 4{,}194{,}304 $$

∎
:::
:::

::: exercise q3
With $d_\text{model} = 512$ and $h = 8$ heads of width 64, how many multiplications do all the
heads spend on the scores of one pair of tokens?

::: answer
512. Each head spends 64, and $8 \times 64 = 512$.
:::
:::

::: reference multi-head-parameter-count
# Parameter count of multi-head attention

With heads of width $d_k = d_\text{model}/h$, the four kinds of matrix in a multi-head attention
layer hold $4\, d_\text{model}^2$ numbers, the same for every $h$. Biases add $4\, d_\text{model}$
more when the layer uses them.

::: equation
P = h \cdot 3\, d_\text{model}\, d_k + h d_k\, d_\text{model} = 4\, d_\text{model}^2
:::

::: legend
$P$: the number of weights, biases left out
$h$: the number of heads
$d_\text{model}$: the width of a token vector
$d_k$: the width of each head
:::

::: derivation
Each head holds $\mathbf{W}_i^Q$, $\mathbf{W}_i^K$, $\mathbf{W}_i^V$, each $d_\text{model} \times d_k$: $3\, d_\text{model}\, d_k$ numbers.[Multi-head attention](reference:multi-head-attention)
$h$ heads hold $3\, d_\text{model} \cdot h d_k = 3\, d_\text{model}^2$, since $h d_k = d_\text{model}$.
$\mathbf{W}^O$ is $h d_k \times d_\text{model} = d_\text{model} \times d_\text{model}$: $d_\text{model}^2$ numbers.
$P = 3\, d_\text{model}^2 + d_\text{model}^2 = 4\, d_\text{model}^2$. ∎
:::
:::

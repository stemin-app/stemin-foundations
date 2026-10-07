---
title: Putting the heads side by side
---

::: card
For one token, the heads give 8 vectors of 64 numbers each. The next layer expects one vector of
512, the same width that came in. To get it back, place the 8 vectors side by side:
**concatenate** them.

$$ \mathbf{c} = \big[\, \mathbf{v}_1 \mid \mathbf{v}_2 \mid \cdots \mid \mathbf{v}_8 \,\big] \qquad 8 \times 64 = 512 $$
:::

::: card
Over the whole sequence, the concatenation joins the heads column by column. Each head is
$n \times d_k$, so the result is $n \times h d_k$, which is $n \times d_\text{model}$.

$$ \text{Concat}(\text{head}_1, \dots, \text{head}_h) \in \mathbb{R}^{n \times h d_k} $$
:::

::: card
The joined vector is **segregated**. Coordinates 1 to 64 come only from head 1, coordinates 65 to
128 only from head 2, and so on. Coordinate $j$ belongs to head $\lceil j / d_k \rceil$. Change
the number of heads and the steps change width, but each coordinate still has one owner.

```plot
x: { var: j, label: "coordinate $j$", from: 1, to: 512, ticks: 64, grid: true }
y: { label: "head that owns $j$", from: 0, to: 17 }

inputs:
  - { name: k, min: 0, max: 4, default: 3, step: 1, label: "log₂ h, so 1, 2, 4, 8 or 16 heads" }

let:
  h: 2 ^ k
  dk: 512 / h

draw:
  - curve: { is: ceil(j / dk), accent: true }
  - hline: { at: h, dash: true, label: "$h$" }
```
:::

::: card
The blocks sit side by side, but they have not met. Head 1 may know that "banks" is a plural
noun. Head 2 may know the context is a river. No single coordinate yet holds "river banks". The
next step mixes them.
:::

::: exercise q1
A model has $d_k = 64$. Which head owns coordinate 200 of the concatenated vector?

::: answer
Head 4. It owns coordinates 193 to 256.
:::

::: solution
$$ \left\lceil \frac{200}{64} \right\rceil = \lceil 3.125 \rceil = 4 $$

Head 4 starts at $3 \times 64 + 1 = 193$ and ends at $4 \times 64 = 256$. ∎
:::
:::

::: exercise q2
A model has $d_\text{model} = 768$ and $h = 12$. Which coordinates of the concatenated vector
come from head 5?

::: answer
Coordinates 257 to 320. Each head fills a block of $d_k = 64$.
:::

::: solution
$$ d_k = \frac{768}{12} = 64 $$

Head 5 starts at $4 \times 64 + 1 = 257$ and ends at $5 \times 64 = 320$. ∎
:::
:::

::: exercise q3
A sequence has $n = 10$ tokens, with $h = 8$ heads of width $d_k = 64$. What is the shape of the
concatenation of all heads?

::: answer
$10 \times 512$. The heads join column by column, $8 \times 64 = 512$.
:::
:::

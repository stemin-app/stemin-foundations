---
title: Self-attention by hand
---

::: card
Take three tokens with 4-dimensional embeddings, one per row:

$$ \mathbf{X} = \begin{bmatrix} 1 & 0 & 1 & 0 \\ 0 & 1 & 0 & 1 \\ 1 & 1 & 0 & 0 \end{bmatrix} $$

Use $d_k = 2$, so every query, key and value has two numbers.
:::

::: card
Fix the three projections:

$$ \mathbf{W}^Q = \begin{bmatrix} 1 & 0 \\ 0 & 1 \\ 1 & 0 \\ 0 & 1 \end{bmatrix}, \quad
\mathbf{W}^K = \begin{bmatrix} 0 & 1 \\ 1 & 0 \\ 0 & 1 \\ 1 & 0 \end{bmatrix}, \quad
\mathbf{W}^V = \begin{bmatrix} 1 & 1 \\ 0 & 0 \\ 0 & 1 \\ 1 & 0 \end{bmatrix} $$

In a real model these are learned. Here they are chosen so the numbers stay small.
:::

::: card
**Step 1.** Multiply $\mathbf{X}$ by each projection. Token 1 is $[1, 0, 1, 0]$, so its query
adds rows 1 and 3 of $\mathbf{W}^Q$: $[2, 0]$. The other rows work the same way:

$$ \mathbf{Q} = \begin{bmatrix} 2 & 0 \\ 0 & 2 \\ 1 & 1 \end{bmatrix}, \quad
\mathbf{K} = \begin{bmatrix} 0 & 2 \\ 2 & 0 \\ 1 & 1 \end{bmatrix}, \quad
\mathbf{V} = \begin{bmatrix} 1 & 2 \\ 1 & 0 \\ 1 & 1 \end{bmatrix} $$
:::

::: card
**Step 2.** Dot every query with every key, then divide by $\sqrt{d_k} = \sqrt{2} \approx 1.414$:

$$ \mathbf{Q}\mathbf{K}^T = \begin{bmatrix} 0 & 4 & 2 \\ 4 & 0 & 2 \\ 2 & 2 & 2 \end{bmatrix}
\quad\Rightarrow\quad
\mathbf{S} \approx \begin{bmatrix} 0 & 2.83 & 1.41 \\ 2.83 & 0 & 1.41 \\ 1.41 & 1.41 & 1.41 \end{bmatrix} $$
:::

::: card
**Step 3.** Softmax row 1. The exponentials are $e^{0} = 1$, $e^{2.83} \approx 16.92$ and
$e^{1.41} \approx 4.11$, which sum to $22.03$:

$$ \operatorname{softmax}([0, 2.83, 1.41]) \approx [0.045,\ 0.768,\ 0.187] $$

Row 2 holds the same scores in another order, so its weights are $[0.768, 0.045, 0.187]$. Row 3
has three equal scores, so its weights are $[0.333, 0.333, 0.333]$.
:::

::: card
The scale $1/\sqrt{d_k}$ sets how sharp row 1 is. Its raw scores are $[0, 4, 2]$; multiply them
by a factor $c$ and watch the weights. At $c = 0.707$ you get the numbers above. At $c = 0$ the
weights are equal; as $c$ grows, token 2 takes almost everything.

```plot
x: { var: c, label: "the factor $c$ on the raw scores", from: 0, to: 1.5, ticks: 0.25, grid: true }
y: { label: "weight in row 1", from: 0, to: 1 }

inputs:
  - { name: k, min: 0, max: 1.5, default: 0.71, step: 0.01, label: "the factor c" }

let:
  z: 1 + exp(4 * c) + exp(2 * c)
  zk: 1 + exp(4 * k) + exp(2 * k)

draw:
  - curve: { is: exp(4 * c) / z, accent: true, label: "token 2" }
  - curve: { is: exp(2 * c) / z, label: "token 3" }
  - curve: { is: 1 / z, dash: true, label: "token 1" }
  - vline: { at: k, dash: true }
  - point: { at: [k, exp(4 * k) / zk] }
  - point: { at: [k, exp(2 * k) / zk] }
  - point: { at: [k, 1 / zk] }
```
:::

::: card
**Step 4.** Each output row blends the value rows with that row's weights:

$$ \mathbf{o}_1 = 0.045\,[1, 2] + 0.768\,[1, 0] + 0.187\,[1, 1] \approx [1.0,\ 0.28] $$

The same sum gives $\mathbf{o}_2 \approx [1.0, 1.72]$ and $\mathbf{o}_3 = [1.0, 1.0]$, so

$$ \mathbf{O} \approx \begin{bmatrix} 1.0 & 0.28 \\ 1.0 & 1.72 \\ 1.0 & 1.0 \end{bmatrix} $$
:::

::: card
Read the pattern. Token 1 looks mostly at token 2, with weight 0.768, and token 2 looks mostly at
token 1. Their own scores were 0: the query $[2, 0]$ of token 1 is perpendicular to its own key
$[0, 2]$. Token 3's query $[1, 1]$ matches every key equally, so its output is the plain average
of the three values.
:::

::: exercise unscaled-weight
Row 1 has raw scores $[0, 4, 2]$. Without the division by $\sqrt{d_k}$, what weight does
token 1 put on token 2?

::: answer
About 0.867. Softmax of the raw scores, with no scaling.
:::

::: solution
$$ e^0 = 1, \quad e^4 \approx 54.60, \quad e^2 \approx 7.39 $$

$$ 1 + 54.60 + 7.39 = 62.99 $$

$$ \frac{54.60}{62.99} \approx 0.867 $$

∎
:::
:::

::: exercise uniform-row-output
Keep the weights of row 3, $[1/3, 1/3, 1/3]$, but change the third value row to $[1, 4]$. What
is $\mathbf{o}_3$?

::: answer
$[1, 2]$. Equal weights give the plain average of the three value rows.
:::

::: solution
$$ \mathbf{o}_3 = \tfrac{1}{3}\left([1, 2] + [1, 0] + [1, 4]\right) = \tfrac{1}{3}[3, 6] = [1, 2] $$

∎
:::
:::

::: exercise zero-diagonal-score
In the example, why is the score of token 2 against its own key exactly 0?

::: answer
Its query $[0, 2]$ and its key $[2, 0]$ are perpendicular, so their dot product is 0.
:::

::: solution
$$ \mathbf{q}_2 \cdot \mathbf{k}_2 = 0 \cdot 2 + 2 \cdot 0 = 0 $$

∎
:::
:::

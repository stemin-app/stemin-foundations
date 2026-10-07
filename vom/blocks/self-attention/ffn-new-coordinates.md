---
title: New coordinates
---

::: card
Going back down to $d$ dimensions does not undo the lift. The output is not the old space again.
It is a **new** space, whose coordinates are combinations of the thresholded answers from the wide
layer. The decisions made up there survive in the new coordinates.
:::

::: card
Build an FFN that finds the factory's defects. The input is $\mathbf{x} = [s, w]$, size and weight.
Four hidden units:

$$ h_1 = \mathrm{ReLU}(s), \quad h_2 = \mathrm{ReLU}(w), \quad h_3 = \mathrm{ReLU}(w - s), \quad h_4 = \mathrm{ReLU}(s - w) $$

Since $s$ and $w$ are positive, $h_1 = s$ and $h_2 = w$. And $h_3 + h_4 = |w - s|$, the gap: one of
the two is always zero.
:::

::: card
Two outputs, the new coordinates:

$$ A = 0.5\,h_1 + 0.5\,h_2, \qquad B = 1.5 - 2\,(h_3 + h_4) $$

$A$ is the average of size and weight. $B$ is high when the gap is small. So the defects, close to
the diagonal, get a high $B$, and the good parts a low one. A defect at $(2, 1.8)$ gets
$B = 1.5 - 0.4 = 1.1$; a good part at $(1, 2.5)$ gets $B = 1.5 - 3 = -1.5$.
:::

::: card
Slide the points through the FFN. At 0 they sit at their inputs, size and weight: the blue defects
lie along the diagonal, with good parts on both sides. At 1 they sit at their outputs $(A, B)$:
the defects float up, the good parts sink, and the dashed line $B = 0.5$ splits them.

```plot
x: { var: t, label: "first coordinate", from: 0, to: 3.5, ticks: 0.5, grid: true }
y: { label: "second coordinate", from: -2, to: 3.5 }

inputs:
  - { name: m, min: 0, max: 1, default: 0, step: 0.05, label: "how far through the FFN" }

draw:
  - hline: { at: 0.5, dash: true }
  - point: { at: [1 + m * 0.1, 1.2 - m * 0.1], accent: true }
  - point: { at: [2 - m * 0.1, 1.8 - m * 0.7], accent: true }
  - point: { at: [3 + m * 0.05, 3.1 - m * 1.8], accent: true }
  - point: { at: [1 + m * 0.75, 2.5 - m * 4] }
  - point: { at: [2.5 - m * 0.75, 1 - m * 2.5] }
  - point: { at: [3 - m * 0.6, 1.8 - m * 2.7] }
  - point: { at: [1.2 + m * 0.6, 2.4 - m * 3.3] }
```
:::

::: card
$A$ and $B$ are linear in $h_1, \ldots, h_4$. But the $h_i$ are thresholded, so the whole map is
not linear in $s$ and $w$: one linear rule holds where $w > s$, another where $w < s$. Without the
ReLU, the FFN would collapse to one matrix, and no matrix can fold the plane along its diagonal.
:::

::: card
Real FFNs do this at scale. With $d_{ff} = 2048$, the wide layer asks 2048 questions, ReLU keeps
the ones that fire, and $\mathbf{W}_2$ writes "feature 17 fired, feature 203 fired" into $d$ numbers.
Attention moves information between positions. The FFN computes at each position, on its own.
:::

::: exercise q1
In the defect FFN, compute $(A, B)$ for a part with $s = 2$ and $w = 2.1$. Is it a defect by the
rule $B > 0.5$?

::: answer
$(2.05, 1.3)$, so yes. The gap is 0.1, and $B = 1.5 - 0.2$.
:::
:::

::: exercise q2
Does the FFN let position 3 read anything from position 5?

::: answer
No. The FFN acts on each position on its own, with the same weights. Moving information between
positions is the job of attention.
:::
:::

::: reference feed-forward-network
# Feed-forward network

The position-wise feed-forward network lifts each position's vector to a wider space, applies
ReLU, and projects back. The same weights act on every position, separately.

::: equation
\mathrm{FFN}(\mathbf{x}) = \mathbf{W}_2\,\mathrm{ReLU}(\mathbf{W}_1\mathbf{x} + \mathbf{b}_1) + \mathbf{b}_2
:::

::: legend
$\mathbf{x}$: one position's vector, in $\mathbb{R}^d$
$\mathbf{W}_1, \mathbf{b}_1$: the lift, $d_{ff} \times d$ and $d_{ff}$, usually $d_{ff} = 4d$
$\mathbf{W}_2, \mathbf{b}_2$: the projection back, $d \times d_{ff}$ and $d$
:::

::: derivation
Count the parameters: $d_{ff}\,d + d_{ff}$ for the lift and $d\,d_{ff} + d$ for the projection.
Total: $2\,d\,d_{ff} + d_{ff} + d$; with $d_{ff} = 4d$ this is $8d^2 + 5d$.
Without the ReLU, $\mathbf{W}_2(\mathbf{W}_1\mathbf{x} + \mathbf{b}_1) + \mathbf{b}_2$ is one affine map, so the bend is what gives the FFN its power.[linear layers collapse](reference:linear-layers-collapse) ∎
:::
:::

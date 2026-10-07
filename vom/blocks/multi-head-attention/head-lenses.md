---
title: Each head has its own lens
---

::: card
Each head $i$ owns three matrices: $\mathbf{W}_i^Q$, $\mathbf{W}_i^K$ and $\mathbf{W}_i^V$. Each
one is $d_\text{model} \times d_k$, so $512 \times 64$. Eight heads hold 24 such matrices, and
no two heads share one.
:::

::: card
Take "banks" in "The river banks overflowed." Its input vector carries many possible meanings at
once: a place that holds money, the side of a river, an aircraft that tilts to turn, a noun, a
verb.
:::

::: card
Head 1 may learn a $\mathbf{W}_1^Q$ that keeps the grammar and drops the meaning. Its 64 numbers
for "banks" say "plural noun". Head 2 may learn a $\mathbf{W}_2^Q$ that drops the grammar and keeps
the meaning: "a physical thing next to water".
:::

::: card
Each head then attends inside its own small space. Head 1 links "overflowed" to its subject,
"banks". Head 2 links "banks" to "river". Neither link blurs the other, because each head
computes its own weights.
:::

::: card
A projection picks out directions. Shrink the idea to two dimensions and a head of width 1: the
head turns $\mathbf{x}$ into the single number $\mathbf{x} \cdot \mathbf{u}$, for a unit vector
$\mathbf{u}$ it learned.[Dot product](reference:dot-product) Turn $\mathbf{u}$ and the same
$\mathbf{x} = (2, 1)$ gives a different number. Two heads with two directions read two different
things from one input.

```plot
x: { var: t, label: "first coordinate", from: -3, to: 3, ticks: 1, grid: true }
y: { label: "second coordinate", from: -3, to: 3 }

inputs:
  - { name: a, min: 0, max: 3.14, default: 0.3, step: 0.02, label: "angle of u in radians" }

let:
  cu: cos(a)
  su: sin(a)
  p: 2 * cu + su

draw:
  - param: { var: r, over: [-3, 3], x: r * cu, y: r * su, dash: true }
  - param: { var: r, over: [0, 1], x: 2 * r, y: r, label: "$x$" }
  - param: { var: r, over: [0, 1], x: 2 + r * (p * cu - 2), y: 1 + r * (p * su - 1), dash: true }
  - param: { var: r, over: [0, 1], x: r * p * cu, y: r * p * su, accent: true }
  - point: { at: [p * cu, p * su], label: "projection" }
```
:::

::: card
Nobody writes these matrices. Training sets them, starting from random numbers. Grammar in one
head and meaning in another is a pattern that trained heads often show, not a rule the model is
given.
:::

::: exercise q1
In two dimensions, two heads of width 1 use the unit vectors $\mathbf{u}_1 = (1, 0)$ and
$\mathbf{u}_2 = (0, 1)$. What does each head output for $\mathbf{x} = (3, 4)$?

::: answer
Head 1 gives 3 and head 2 gives 4. Each head takes the dot product of $\mathbf{x}$ with its own
direction.
:::

::: solution
$$ \mathbf{x} \cdot \mathbf{u}_1 = 3 \times 1 + 4 \times 0 = 3 $$

$$ \mathbf{x} \cdot \mathbf{u}_2 = 3 \times 0 + 4 \times 1 = 4 $$

∎
:::
:::

::: exercise q2
A head of width 1 uses the unit vector $\mathbf{u} = (1/\sqrt{2}, 1/\sqrt{2})$. What does it output
for $\mathbf{x} = (2, 1)$?

::: answer
$3/\sqrt{2} \approx 2.12$. Take the dot product $\mathbf{x} \cdot \mathbf{u}$.
:::

::: solution
$$ \mathbf{x} \cdot \mathbf{u} = \frac{2}{\sqrt{2}} + \frac{1}{\sqrt{2}} = \frac{3}{\sqrt{2}} = 2.121 $$

∎
:::
:::

::: exercise q3
How many numbers does one query matrix $\mathbf{W}_i^Q$ hold when $d_\text{model} = 512$ and
$d_k = 64$?

::: answer
32,768. The matrix is $512 \times 64$.
:::

::: solution
$$ 512 \times 64 = 32{,}768 $$

∎
:::
:::

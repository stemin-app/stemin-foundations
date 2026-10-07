---
title: A weighted average of values
---

::: card
Take a sequence of vectors $\mathbf{v}_1, \ldots, \mathbf{v}_n$, one per token of "The cat sat on
the mat". Call them the **values**. You want one output vector that combines them. The plainest
way is the average:

$$ \mathbf{o} = \frac{1}{n}\sum_{i=1}^{n} \mathbf{v}_i $$

It treats every position alike. To build the meaning of "sat", "cat", its subject, matters more
than "the".
:::

::: card
So weigh them. **Attention** takes a weighted average whose weights measure relevance:

$$ \mathbf{o} = \sum_{i=1}^{n} \alpha_i \mathbf{v}_i, \qquad \alpha_i \ge 0, \quad \sum_{i=1}^{n} \alpha_i = 1 $$

The $\alpha_i$ are the **attention weights**. If $\alpha_3 = 0.7$ and the rest are small, the
output is mostly $\mathbf{v}_3$: the model pays attention to position 3.
:::

::: card
Weights that are positive and add to 1 come from a [softmax](reference:softmax) over scores.
Below, three values sit at the corners of a triangle. Raise one score and the output slides
toward its value. With equal scores it sits at the centre, the plain average. The output can
never leave the triangle.

```plot
x: { var: t, label: "first component", from: 0, to: 6, ticks: 1, grid: true }
y: { label: "second component", from: 0, to: 5 }

inputs:
  - { name: s1, min: -3, max: 3, default: 0, step: 0.1, label: "score of v1" }
  - { name: s2, min: -3, max: 3, default: 0, step: 0.1, label: "score of v2" }
  - { name: s3, min: -3, max: 3, default: 1.5, step: 0.1, label: "score of v3" }

let:
  z: exp(s1) + exp(s2) + exp(s3)
  a1: exp(s1) / z
  a2: exp(s2) / z
  a3: exp(s3) / z

draw:
  - param: { var: u, over: [0, 1], x: 1 + 4 * u, y: 1, dash: true }
  - param: { var: u, over: [0, 1], x: 5 - 2 * u, y: 1 + 3 * u, dash: true }
  - param: { var: u, over: [0, 1], x: 3 - 2 * u, y: 4 - 3 * u, dash: true }
  - point: { at: [1, 1], label: "v₁" }
  - point: { at: [5, 1], label: "v₂" }
  - point: { at: [3, 4], label: "v₃" }
  - point: { at: [a1 + 5 * a2 + 3 * a3, a1 + a2 + 4 * a3], label: "o", accent: true }
```
:::

::: exercise q1
Three values are $[1, 0]$, $[0, 1]$ and $[1, 1]$, with weights $0.5, 0.25, 0.25$. What is the
output?

::: answer
$[0.75, 0.5]$. Add the values, each times its weight.
:::
:::

::: exercise q2
Can the weights $[0.6, 0.6, -0.2]$ be attention weights?

::: answer
No. They add to 1, but one is negative. Attention weights come from a softmax, which never
gives a negative number.
:::
:::

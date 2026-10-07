---
title: The scaled dot product
---

::: card
Transformers score a query against a key with the **scaled dot product**:

$$ s(\mathbf{q}, \mathbf{k}) = \frac{\mathbf{q}^T\mathbf{k}}{\sqrt{d}} $$

where $d$ is the size of the query and key vectors. The dot product is large when the two point
the same way ([dot product](reference:dot-product)): a query for "what I need" scores high against
a key for "what I offer" when the two align.
:::

::: card
Why divide by $\sqrt{d}$? Let the entries of $\mathbf{q}$ and $\mathbf{k}$ be independent, with
mean 0 and variance 1. Each product $q_i k_i$ then has mean 0 and variance 1, and the dot product
adds $d$ of them:

$$ \operatorname{Var}\left(\mathbf{q}^T\mathbf{k}\right) = \sum_{i=1}^{d} \operatorname{Var}(q_i k_i) = d $$

([variance of a sum](reference:variance-of-independent-sum)). With $d = 512$, typical scores are
about $\pm 22$.
:::

::: card
Large scores **saturate** the softmax. One score far above the rest takes almost all the weight,
and the slope of the softmax falls toward zero ([softmax Jacobian](reference:softmax-jacobian)).
Little gradient flows back, and the attention stops learning. Dividing by $\sqrt{d}$ brings the
variance back to 1 at every size.
:::

::: card
Two keys, whose scores differ by a typical amount, one standard deviation. Unscaled, that gap is
$\sqrt{d}$; scaled, it is 1. The curves show the softmax slope $p(1 - p)$ of the winning key. The
unscaled slope collapses as $d$ grows. The scaled one stays at 0.197 for every $d$.

```plot
x: { var: d, label: "the dimension d", from: 1, to: 1024, scale: log, grid: true }
y: { label: "slope of the softmax", from: 0, to: 0.26 }

let:
  pu: 1 / (1 + exp(-sqrt(d)))
  ps: 1 / (1 + exp(-1))

draw:
  - curve: { is: pu * (1 - pu), dash: true, label: "unscaled" }
  - curve: { is: ps * (1 - ps), accent: true, label: "scaled" }
  - point: { at: [64, 0.000335], label: "d = 64" }
```
:::

::: exercise q1
Queries and keys have size 64, with independent entries of variance 1. What is the standard
deviation of $\mathbf{q}^T\mathbf{k}$, and of the scaled score?

::: answer
8 and 1. The variance of the dot product is 64; dividing by $\sqrt{64} = 8$ brings it to 1.
:::
:::

::: exercise q2
$\mathbf{q} = [2, 0, 1, 1]$ and $\mathbf{k} = [1, 3, 0, 2]$. What is the scaled score?

::: answer
2. The dot product is $2 + 0 + 0 + 2 = 4$, and $\sqrt{4} = 2$.
:::
:::

::: exercise q3
Two scores are 24 and 0. What weight does the softmax give the second? Is the gradient through
it useful?

::: answer
About $3.8 \times 10^{-11}$, so no. The softmax is saturated, and its slope is almost zero.
:::
:::

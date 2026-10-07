---
title: Why one head is not enough
---

::: card
Read the sentence "The pilot flew the plane to Paris." To understand "flew", you make three
lookups at once. Who flew? The pilot. What was flown? The plane. Where to? Paris.
:::

::: card
One self-attention head gives each token one set of weights over the sequence: a probability
distribution, so the weights sum to 1.[Self-attention](reference:self-attention) For "flew",
every bit of weight the head gives to "pilot" is weight it cannot give to "plane" or "Paris".
:::

::: card
Take only two candidates, "pilot" and "Paris", with scores $s_1$ and $s_2$. The softmax turns
their gap $\Delta = s_1 - s_2$ into the weight on "pilot".[Softmax](reference:softmax)

$$ w_\text{pilot} = \frac{e^{s_1}}{e^{s_1} + e^{s_2}} = \frac{1}{1 + e^{-\Delta}} \qquad w_\text{Paris} = 1 - w_\text{pilot} $$
:::

::: card
Move along the score gap. The two curves cross and never rise together. A gap of $\ln 4 \approx 1.39$ gives 80% to
"pilot" and 20% to "Paris".

```plot
x: { var: g, label: "score gap $Δ$", from: -5, to: 5, ticks: 1, grid: true }
y: { label: "weight", from: 0, to: 1.05 }

draw:
  - curve: { is: 1 / (1 + exp(-g)), accent: true, label: "pilot" }
  - curve: { is: 1 - 1 / (1 + exp(-g)), dash: true, label: "Paris" }
  - point: { at: [1.386, 0.8], label: "80%" }
  - point: { at: [1.386, 0.2], label: "20%" }
```
:::

::: card
With 80% on "pilot" and 20% on "Paris", the output for "flew" is $0.8\,\mathbf{v}_\text{pilot}
+ 0.2\,\mathbf{v}_\text{Paris}$. It is mostly "who acted" and a little "where". Neither relation
comes through clean.
:::

::: card
Think of a recording that mixes the bass, the vocals and the drums onto one track. Once they
share the track, you cannot pull one of them out again. One head mixes its lookups the same way.
:::

::: card
**Multi-head attention** runs several attention mechanisms, called **heads**, side by side on the
same input. Each head computes its own distribution. One head can put all its weight on "pilot"
while another puts all its weight on "Paris".
:::

::: exercise q1
One head attends from "flew" with weight 0.8 on "pilot" and 0.2 on "Paris". The value vectors
are $\mathbf{v}_\text{pilot} = (1, 0)$ and $\mathbf{v}_\text{Paris} = (0, 1)$. What is the output
for "flew"?

::: answer
$(0.8, 0.2)$. The output is the weighted sum of the value vectors.
:::

::: solution
$$ 0.8\,(1, 0) + 0.2\,(0, 1) = (0.8, 0) + (0, 0.2) = (0.8, 0.2) $$

∎
:::
:::

::: exercise q2
A head compares two tokens with scores $s_1$ and $s_2$. What score gap $s_1 - s_2$ puts a weight
of 0.9 on the first token?

::: answer
$\ln 9 \approx 2.20$. Set $1/(1 + e^{-\Delta}) = 0.9$ and solve for $\Delta$.
:::

::: solution
$$ \frac{1}{1 + e^{-\Delta}} = 0.9 $$

$$ 1 + e^{-\Delta} = \frac{1}{0.9} = 1.111 $$

$$ e^{-\Delta} = \frac{1}{9} $$

$$ \Delta = \ln 9 = 2.197 $$

∎
:::
:::

::: exercise q3
Can one attention head give a weight of 0.7 to "pilot" and a weight of 0.7 to "Paris" from the
same token?

::: answer
No. The weights of one head form a distribution, so they sum to 1, and $0.7 + 0.7 = 1.4$.
:::
:::

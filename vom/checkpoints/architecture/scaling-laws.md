---
title: Scaling laws
---

Answer every question without notes. Losses are in nats.

::: exercise q1
On log-log axes, a loss falls 0.1 decades for each decade of $N$. What is the exponent, and by what
factor does doubling $N$ multiply the loss?

::: answer
$-0.1$, and $2^{-0.1} \approx 0.933$.
:::
:::

::: exercise q2
Under $L(N) = (8.8 \times 10^{13}/N)^{0.076}$, what loss does a model of $8.8 \times 10^{9}$
parameters reach?

::: answer
About 2.01. The ratio is $10^4$, and $10^{4 \times 0.076} = 10^{0.304}$.
:::
:::

::: exercise q3
A run trains on $5.4 \times 10^{12}$ tokens. What floor does the data put under the loss, by the
unified law with $D_c = 5.4 \times 10^{13}$ and $\alpha_D = 0.095$?

::: answer
About 1.24. $10^{0.095} \approx 1.245$.
:::
:::

::: exercise q4
A model has loss 2.4 and the floor is 1.6. The reducible part follows $N^{-0.076}$. What is the
loss after the model grows 100 times?

::: answer
About 2.16.
:::

::: solution
Reducible part: $2.4 - 1.6 = 0.8$.

$0.8 \times 100^{-0.076} = 0.8 \times 10^{-0.152} = 0.8 \times 0.705 = 0.564$.

Loss: $1.6 + 0.564 \approx 2.16$. ∎
:::
:::

::: exercise q5
What does training a 13 billion parameter model on 260 billion tokens cost in FLOPs? Is the split
compute-optimal by the 20 tokens per parameter rule?

::: answer
About $2.0 \times 10^{22}$ FLOPs, and yes: $260 / 13 = 20$.
:::
:::

::: exercise q6
Under Chinchilla's rule, the budget grows 16 times. By what factor do the best model size and the
best token count grow?

::: answer
Both grow 4 times. Each is proportional to $C^{0.5}$.
:::
:::

::: exercise q7
A task needs 10 correct steps in a row. What is its success rate at 90% per step, and at 99%?

::: answer
About 0.35 and about 0.90. $0.9^{10} \approx 0.349$ and $0.99^{10} \approx 0.904$.
:::
:::

::: exercise q8
Words follow Zipf's law with $s = 1.2$. By what factor does the tail mass beyond the cutoff shrink
when the cutoff grows 10 times?

::: answer
It is multiplied by $10^{-0.2} \approx 0.631$. The tail is proportional to $k^{1 - s}$.
:::
:::

::: exercise q9
Why can a 2× efficiency gain not change how many doublings of compute a given loss improvement
needs?

::: answer
It shifts the log-log line down without changing its slope. The slope, the exponent, fixes how much
each doubling buys.
:::
:::

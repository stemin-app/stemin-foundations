---
title: KL divergence
---

::: card
Cross-entropy measures how wrong a model is, but part of it is not the model's fault. Let the truth
be a fair coin, $p = [0.5,\ 0.5]$, and let the model predict exactly that. The cross-entropy is
still

$$ H(p, p) = -\left[0.5 \log 0.5 + 0.5 \log 0.5\right] = 0.69 $$
:::

::: card
That 0.69 is the entropy of the truth, $H(p)$: the uncertainty built into the problem. No model can
beat it, because no model can predict a fair coin. The entropy of $p$ is the smallest cross-entropy
any model can reach:

- $[1,\ 0,\ 0,\ 0]$: $H(p) = 0$, one right answer.
- $[0.7,\ 0.1,\ 0.1,\ 0.1]$: $H(p) = 0.94$.
- $[0.25,\ 0.25,\ 0.25,\ 0.25]$: $H(p) = 1.39$, the most uncertain.
:::

::: card
The **KL divergence** strips that floor away. It is the cross-entropy minus the entropy of the
truth:[KL divergence](reference:kl-divergence)

$$ D_{KL}(p \,\|\, q) = H(p, q) - H(p) = \sum_x p(x) \log \frac{p(x)}{q(x)} $$

It measures the extra surprise that comes from using $q$ when the truth is $p$.
:::

::: card
Let the truth be $p = [0.7,\ 0.3]$, 70% "cat" and 30% "dog", and let the model say $q = [0.5,\ 0.5]$.
First the entropy of the truth:

$$ H(p) = -\left[0.7 \times (-0.357) + 0.3 \times (-1.204)\right] = 0.250 + 0.361 = 0.61 $$
:::

::: card
Then the cross-entropy, and the gap between them:

$$ H(p, q) = -\left[0.7 \times (-0.693) + 0.3 \times (-0.693)\right] = 0.69 $$

$$ D_{KL}(p \,\|\, q) = 0.69 - 0.61 = 0.08 $$

The model meets 0.69 nats of surprise. Of that, 0.61 is unavoidable and 0.08 comes from the model
being wrong.
:::

::: card
Two coins. The truth lands heads with probability $p$; the model says $q$. As $q$ moves, the
cross-entropy curve never drops below the floor $H(p)$, and it touches the floor only at $q = p$.
The accent curve is their gap, the KL divergence, which is 0 at $q = p$.

```plot
x: { var: q, label: "the model's probability of heads $q$", from: 0, to: 1, ticks: 0.1, grid: true }
y: { label: "nats", from: 0, to: 2, ticks: 0.5 }

inputs:
  - { name: p, min: 0.05, max: 0.95, default: 0.7, step: 0.05, label: "true probability of heads p" }

let:
  hp: -p * log(e(), p) - (1 - p) * log(e(), 1 - p)
  hpq: -p * log(e(), q) - (1 - p) * log(e(), 1 - q)

draw:
  - hline: { at: hp, dash: true, label: "$H(p)$" }
  - curve: { is: hpq, over: [0.01, 0.99], label: "$H(p, q)$" }
  - curve: { is: hpq - hp, over: [0.01, 0.99], accent: true, label: "KL" }
  - vline: { at: p, dash: true }
```
:::

::: card
During training the truth $p$ does not change, so neither does $H(p)$. Whatever $q$ lowers the
cross-entropy lowers the KL divergence by the same amount:

$$ \arg\min_q H(p, q) = \arg\min_q D_{KL}(p \,\|\, q) $$

You minimize the cross-entropy because it is simpler: it never needs $H(p)$, which you do not know.
:::

::: card
The KL divergence is never negative, and it is 0 only when $p = q$. It is not a true distance,
because it is not symmetric. With $p = [0.7,\ 0.3]$ and $q = [0.5,\ 0.5]$:

$$ D_{KL}(p \,\|\, q) = 0.082 \qquad D_{KL}(q \,\|\, p) = 0.087 $$

Swap the two distributions and the number changes.
:::

::: exercise q1
What is $D_{KL}(p \,\|\, p)$ for any distribution $p$?

::: answer
$0$. Every ratio $p(x)/p(x)$ is 1, and $\log 1 = 0$.
:::
:::

::: exercise q2
The truth is $p = [1,\ 0]$ and the model is $q = [0.8,\ 0.2]$. What is $D_{KL}(p \,\|\, q)$?

::: answer
$\log 1.25 \approx 0.223$. Only the first term survives.
:::

::: solution
$$ D_{KL}(p \,\|\, q) = 1 \cdot \log \frac{1}{0.8} + 0 = \log 1.25 \approx 0.223 $$

∎
:::
:::

::: exercise q3
The truth is $p = [0.5,\ 0.5]$ and the model is $q = [0.9,\ 0.1]$. What is $D_{KL}(p \,\|\, q)$?

::: answer
About $0.511$. Add $p(x) \log \frac{p(x)}{q(x)}$ over both outcomes.
:::

::: solution
$$ 0.5 \log \frac{0.5}{0.9} = 0.5 \times (-0.588) = -0.294 $$

$$ 0.5 \log \frac{0.5}{0.1} = 0.5 \times 1.609 = 0.805 $$

$$ D_{KL}(p \,\|\, q) = -0.294 + 0.805 = 0.511 $$

∎
:::
:::

::: exercise q4
A model has cross-entropy 1.2 nats against a truth whose entropy is 0.9 nats. What is the KL
divergence?

::: answer
$0.3$ nats. Subtract the entropy from the cross-entropy.
:::
:::

::: reference kl-divergence
# KL divergence

The KL divergence of $q$ from $p$, named for Kullback and Leibler, is the extra surprise of using $q$ when the truth is
$p$. It is never negative, it is 0 only when $q = p$, and it is not symmetric.

::: equation
D_{KL}(p \,\|\, q) = \sum_x p(x) \log \frac{p(x)}{q(x)} = H(p, q) - H(p) \geq 0
:::

::: legend
$p$: the true distribution
$q$: the distribution of the model
$H(p, q)$: the cross-entropy of $q$ against $p$
$H(p)$: the entropy of $p$
:::

::: derivation
$H(p, q) = -\sum_x p(x) \log q(x)$.[Cross-entropy](reference:cross-entropy)
$H(p) = -\sum_x p(x) \log p(x)$.[Entropy](reference:entropy)
Subtract: $H(p, q) - H(p) = \sum_x p(x)\left(\log p(x) - \log q(x)\right) = \sum_x p(x) \log \frac{p(x)}{q(x)}$.
Negate it: $-D_{KL} = \sum_x p(x) \log \frac{q(x)}{p(x)}$, the mean of $\log \frac{q(X)}{p(X)}$ under $p$.
The logarithm is concave, so that mean is at most the log of the mean of $\frac{q(X)}{p(X)}$ (Jensen's inequality).
That mean is $\sum_x p(x) \frac{q(x)}{p(x)} = \sum_x q(x) \leq 1$, so $-D_{KL} \leq \log 1 = 0$.
Equality in Jensen's inequality needs $\frac{q(x)}{p(x)}$ constant, which with both summing to 1 means $q = p$. ∎
:::
:::

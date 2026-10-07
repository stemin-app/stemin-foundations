---
title: Softer targets
---

::: card
A one-hot target asks for $q_y = 1$. The softmax reaches 1 only when the correct logit runs off
to infinity, so the loss keeps pushing $z_y$ up long after the answer is right. The model grows
overconfident, and an overconfident model is badly calibrated.
:::

::: card
**Label smoothing** moves a little of the target's mass off the correct token and spreads it
evenly over the whole vocabulary of $V$ tokens. With smoothing parameter $\epsilon$:
[Label smoothing](reference:label-smoothing)

$$ p_j = \begin{cases} 1 - \epsilon + \dfrac{\epsilon}{V} & j = y \\[4pt] \dfrac{\epsilon}{V} & j \neq y \end{cases} $$

The entries still sum to 1: $1 - \epsilon + \frac{\epsilon}{V} + (V - 1)\frac{\epsilon}{V} = 1$.
:::

::: card
Slide $\epsilon$ from 0 to 1. At 0 the target is one-hot. At 1 every token gets $\frac{1}{V}$ and the
target says nothing about the answer. A small $\epsilon$, typically $0.1$, sits near the one-hot
end and only takes the edge off.

```plot
x: { var: eps, label: "$ε$", from: 0, to: 1, ticks: 0.1, grid: true }
y: { label: "target probability", from: 0, to: 1.05 }

inputs:
  - { name: V, min: 2, max: 20, default: 5, step: 1, label: "the vocabulary size V" }

draw:
  - curve: { is: 1 - eps + eps / V, accent: true, label: "correct token" }
  - curve: { is: eps / V, dash: true, label: "each other token" }
  - hline: { at: 1 / V, dash: true }
```
:::

::: card
The loss is the [cross-entropy](reference:cross-entropy) against the smoothed target. Split it by
where the mass of $p$ sits:

$$ -\sum_{j} p_j \log q_j = (1 - \epsilon)\left(-\log q_y\right) + \epsilon \cdot \frac{1}{V}\sum_{j=1}^{V} \left(-\log q_j\right) $$

It mixes the ordinary loss with a second loss that wants the prediction to be uniform. The
second term blows up if any $q_j$ goes to zero.
:::

::: card
The loss is now smallest at a finite prediction: the target itself. Spread the rest of the mass
evenly and drag $\epsilon$. The minimum sits at $q_y = 1 - \epsilon + \frac{\epsilon}{V}$, marked by the
dashed line, and moves off the right edge only when $\epsilon = 0$.

```plot
x: { var: q, label: "$qᵧ$", from: 0, to: 1, ticks: 0.1, grid: true }
y: { label: "loss", from: 0, to: 5 }

inputs:
  - { name: eps, min: 0, max: 0.5, default: 0.2, step: 0.05, label: "the smoothing ε" }
  - { name: V, min: 2, max: 20, default: 10, step: 1, label: "the vocabulary size V" }

let:
  py: 1 - eps + eps / V

draw:
  - curve: { is: "-py * log(e(), q) - (1 - py) * log(e(), (1 - q) / (V - 1))", over: [0.01, 0.99], accent: true }
  - vline: { at: py, dash: true }
```
:::

::: card
At the minimum the logits stay finite. The gap between the correct logit and any other settles
at $z_y - z_j = \ln \frac{p_y}{p_j}$.

With $\epsilon = 0.1$ and $V = 50{,}000$, the correct token's target is $0.900002$ and every other
token's is $2 \times 10^{-6}$. The gap settles at $\ln 450{,}001 \approx 13.0$, not at infinity.
:::

::: exercise ls-four-tokens
A vocabulary has $V = 4$ tokens and $\epsilon = 0.1$. What is the smoothed target?

::: answer
$0.925$ on the correct token and $0.025$ on each of the other three.
:::

::: solution
$\frac{\epsilon}{V} = \frac{0.1}{4} = 0.025$

$p_y = 1 - 0.1 + 0.025 = 0.925$

Check: $0.925 + 3 \times 0.025 = 1$. ∎
:::
:::

::: exercise ls-logit-gap
With $V = 4$ and $\epsilon = 0.2$, the loss is minimal at the smoothed target. What gap
$z_y - z_j$ between the correct logit and another logit does that require?

::: answer
$\ln 17 \approx 2.833$. The gap is $\ln \frac{p_y}{p_j}$.
:::

::: solution
$p_j = \frac{0.2}{4} = 0.05$

$p_y = 1 - 0.2 + 0.05 = 0.85$

The softmax gives $\frac{q_y}{q_j} = e^{z_y - z_j}$, so $z_y - z_j = \ln \frac{0.85}{0.05} = \ln 17 = 2.833$ ∎
:::
:::

::: exercise ls-full-smoothing
What is the smoothed target when $\epsilon = 1$?

::: answer
The uniform distribution, $\frac{1}{V}$ on every token. It no longer depends on the answer.
:::
:::

::: reference label-smoothing
# Label smoothing

Label smoothing replaces the one-hot target with a mixture of the one-hot target and the uniform
distribution. The cross-entropy against it is smallest when the prediction equals the smoothed
target.

::: equation
p_j = (1 - \epsilon)\,\mathbb{1}_{j=y} + \frac{\epsilon}{V}
:::

::: legend
$p_j$: the target probability of token $j$
$y$: the correct token
$\epsilon$: the smoothing parameter, between 0 and 1
$V$: the size of the vocabulary
$\mathbb{1}_{j=y}$: 1 when $j = y$, else 0
:::

::: derivation
Goal: the sum of the target, and the prediction that minimizes the loss.
Sum: $\sum_j p_j = (1 - \epsilon) + V \cdot \frac{\epsilon}{V} = 1$.
Loss: $-\sum_j p_j \log q_j = (1 - \epsilon)(-\log q_y) + \frac{\epsilon}{V}\sum_j (-\log q_j)$.
Write the loss as $H(p, q) = H(p) + D_{KL}(p \,\|\, q)$.[Entropy](reference:entropy)
$H(p)$ does not depend on $q$, and $D_{KL}(p \,\|\, q) \ge 0$ with equality only at $q = p$.[KL divergence](reference:kl-divergence)
So the loss is smallest at $q = p$, where $q_y = 1 - \epsilon + \frac{\epsilon}{V} < 1$ for $\epsilon > 0$. ∎
:::
:::

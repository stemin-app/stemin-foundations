---
title: Uncertainty
---

::: card
Three models predict the next word among cat, dog, bird and fish:

- Model A: 95%, 3%, 1%, 1%. It is nearly sure of "cat".
- Model B: 25% each. It has no idea.
- Model C: 70%, 10%, 10%, 10%. It leans toward "cat".

You want one number for how uncertain each model is: low when confident, high when lost. That
number is the **entropy**, $H$.
:::

::: card
Start from surprise. "The sun rose this morning" surprises nobody: it always does. "A meteor hit
your car" surprises everyone: it almost never happens. Rare outcomes are surprising, so measure
the **surprise** of an outcome by[Surprisal](reference:surprisal)

$$ \text{surprise} = -\log p $$

Here $\log$ is the natural logarithm, and the unit of surprise is the nat.
:::

::: card
A certain outcome, $p = 1$, has surprise 0. A coin flip, $p = 0.5$, has surprise 0.69. An outcome
with $p = 0.1$ has 2.30, and one with $p = 0.01$ has 4.61. The curve climbs without bound as $p$
falls toward 0.

```plot
x: { var: p, label: "probability $p$", from: 0, to: 1, ticks: 0.1, grid: true }
y: { label: "surprise $-log p$", from: 0, to: 5.5, ticks: 1 }

inputs:
  - { name: p0, min: 0.01, max: 1, default: 0.1, step: 0.01, label: "probability of the outcome" }

let:
  s: -log(e(), p)
  s0: -log(e(), p0)

draw:
  - curve: { is: s, over: [0.004, 1], accent: true }
  - point: { at: [p0, s0], label: "surprise" }
```
:::

::: card
**Entropy is the expected surprise.** Multiply the surprise of each outcome by its probability,
and add:[Entropy](reference:entropy)

$$ H(p) = \sum_x p(x) \left(-\log p(x)\right) = -\sum_x p(x) \log p(x) $$

An outcome with $p(x) = 0$ adds nothing: $0 \log 0$ counts as 0.
:::

::: card
Model A. The likely word has little surprise. The unlikely words have large surprise but small
probability, so they add little:

$$ H = 0.95 \times 0.051 + 0.03 \times 3.51 + 2 \times (0.01 \times 4.61) = 0.049 + 0.105 + 0.092 \approx 0.25 $$

Low entropy: the model is confident.
:::

::: card
Model B. Every word has the same surprise, $-\log 0.25 = 1.386$:

$$ H = 4 \times (0.25 \times 1.386) = 1.386 = \log 4 $$

This is the largest entropy any distribution over 4 outcomes can have.

Model C sits between them:

$$ H = 0.70 \times 0.357 + 3 \times (0.10 \times 2.303) = 0.250 + 0.691 \approx 0.94 $$
:::

::: card
A coin that lands heads with probability $p$ has entropy $-p \log p - (1 - p)\log(1 - p)$. It is 0
when the coin is certain, at $p = 0$ or $p = 1$, and largest at $p = 0.5$, where it reaches
$\log 2 \approx 0.693$.

```plot
x: { var: p, label: "probability of heads $p$", from: 0, to: 1, ticks: 0.1, grid: true }
y: { label: "entropy $H$", from: 0, to: 0.8 }

inputs:
  - { name: p0, min: 0.01, max: 0.99, default: 0.2, step: 0.01, label: "probability of heads" }

let:
  h: -p * log(e(), p) - (1 - p) * log(e(), 1 - p)
  h0: -p0 * log(e(), p0) - (1 - p0) * log(e(), 1 - p0)

draw:
  - hline: { at: 0.693, dash: true, label: "$log 2$" }
  - curve: { is: h, over: [0.001, 0.999], accent: true }
  - point: { at: [p0, h0] }
```
:::

::: card
Ordered by entropy, model A has 0.25, model C has 0.94, and model B has 1.39. During training you
want the model to grow confident about the right answers, so its entropy falls. Watching the
entropy is one way to see a model learn.
:::

::: exercise q1
What is the entropy of a fair coin?

::: answer
$\log 2 \approx 0.693$ nats. Two outcomes, each with surprise $\log 2$.
:::

::: solution
$$ H = -0.5 \log 0.5 - 0.5 \log 0.5 = -\log 0.5 = \log 2 \approx 0.693 $$

∎
:::
:::

::: exercise q2
What is the entropy of a uniform distribution over 8 outcomes?

::: answer
$\log 8 \approx 2.079$ nats. Every outcome has surprise $\log 8$.
:::

::: solution
$$ H = -8 \times \tfrac{1}{8} \log \tfrac{1}{8} = \log 8 \approx 2.079 $$

∎
:::
:::

::: exercise q3
What is the entropy of the distribution $[1,\ 0,\ 0]$?

::: answer
$0$. The sure outcome has surprise $-\log 1 = 0$, and the others have probability 0.
:::
:::

::: exercise q4
An outcome has probability 0.25. What is its surprise?

::: answer
$-\log 0.25 = \log 4 \approx 1.386$ nats.
:::
:::

::: reference surprisal
# Surprisal

The surprise of an outcome is the negative logarithm of its probability. A sure outcome has
surprise 0, and the surprise grows without bound as the probability falls to 0.

::: equation
s(x) = -\log p(x)
:::

::: legend
$s(x)$: the surprise of outcome $x$, in nats with the natural logarithm
$p(x)$: the probability of $x$
:::
:::

::: reference entropy
# Entropy

The entropy of a distribution is its expected surprise. It is 0 for a certain outcome and at most
$\log n$ over $n$ outcomes, reached by the uniform distribution.

::: equation
H(p) = -\sum_x p(x) \log p(x) \qquad 0 \leq H(p) \leq \log n
:::

::: legend
$H(p)$: the entropy of the distribution $p$, in nats
$p(x)$: the probability of outcome $x$; a term with $p(x) = 0$ counts as 0
$n$: the number of possible outcomes
:::

::: derivation
The surprise of $x$ is $-\log p(x)$.[Surprisal](reference:surprisal)
Weight each surprise by its probability and add: $H(p) = \sum_x p(x)\left(-\log p(x)\right)$.[Expectation](reference:expectation)
Each $p(x) \leq 1$, so each $-\log p(x) \geq 0$, and $H(p) \geq 0$.
The logarithm is concave, so the mean of $\log \frac{1}{p(X)}$ is at most the log of the mean of $\frac{1}{p(X)}$ (Jensen's inequality).
That mean is $\sum_x p(x) \cdot \frac{1}{p(x)}$, summed over the outcomes with $p(x) > 0$: their count, at most $n$.
So $H(p) \leq \log n$, with equality when every $p(x) = \frac{1}{n}$. ∎
:::
:::

---
title: Mass and density
---

::: card
A **discrete** random variable takes values you can count off one by one, like the words of a
vocabulary or the faces of a die. Each value has its own probability, given by the
**probability mass function**:

$$ p(x) = P(X = x) $$

A fair die has $p(1) = p(2) = \cdots = p(6) = \frac{1}{6}$.
:::

::: card
A **continuous** random variable takes values in a continuum, such as any real number between 0
and 1. There are infinitely many such values, so no single one can carry a share of the
probability: each exact value has probability zero.

Probability then lives on intervals, and a **probability density function** $f(x)$ describes how
it spreads.
:::

::: card
The probability that $X$ lands between $a$ and $b$ is the area under the density across that
interval.[Probability from a density](reference:probability-density)

$$ P(a \leq X \leq b) = \int_a^b f(x)\, dx $$
:::

::: card
Take the density $f(x) = e^{-x}$ for $x \geq 0$. The shaded area is $P(a \leq X \leq b) = e^{-a} -
e^{-b}$. With $a = 0.5$ and $b = 1.5$ it is $0.607 - 0.223 = 0.383$. Slide the interval right and
the same width holds less probability, because the density is lower there.

```plot
x: { var: x, label: "$x$", from: 0, to: 4, ticks: 0.5, grid: true }
y: { label: "$f(x)$", from: 0, to: 1.1 }

inputs:
  - { name: a, min: 0, max: 3, default: 0.5, step: 0.1, label: "start a" }
  - { name: w, min: 0.1, max: 1, default: 1, step: 0.1, label: "width b - a" }

draw:
  - area: { under: exp(-x), over: [a, a + w], accent: true }
  - curve: exp(-x)
  - point: { at: [a + w / 2, exp(-a - w / 2) / 2], label: "$P$" }
```
:::

::: card
A density says how concentrated the probability is near $x$. Where $f(x)$ is high, outcomes near
$x$ are likely; where it is low, they are rare. The whole area under $f$ is 1.

A density is not a probability, and it can exceed 1. The uniform density on $[0, 0.5]$ has
height $f(x) = 2$ across that interval: the area is $2 \times 0.5 = 1$, as it must be.
:::

::: card
Transformers work mostly with discrete distributions, over the tokens of a vocabulary.
Continuous distributions appear when you set the initial weights of a network and when you
analyse how signals grow or shrink inside it.
:::

::: exercise q1
$X$ is uniform on $[0, 4]$, so $f(x) = \frac{1}{4}$ there. What is $P(1 \leq X \leq 2)$?

::: answer
$0.25$. The area is height times width, $\frac{1}{4} \times 1$.
:::
:::

::: exercise q2
$X$ has the density $f(x) = e^{-x}$ for $x \geq 0$. What is $P(0 \leq X \leq 1)$?

::: answer
$1 - e^{-1} \approx 0.632$. Integrate the density from 0 to 1.
:::

::: solution
$$ P(0 \leq X \leq 1) = \int_0^1 e^{-x}\, dx = \left[-e^{-x}\right]_0^1 = 1 - e^{-1} $$

$$ 1 - 0.368 = 0.632 $$

∎
:::
:::

::: exercise q3
$X$ is a continuous random variable with density $f(x) = 2x$ on $[0, 1]$. What is $P(X = 0.5)$?

::: answer
$0$. A single point of a continuous variable carries no area, so no probability.
:::

::: solution
$$ P(X = 0.5) = \int_{0.5}^{0.5} 2x\, dx = 0 $$

∎
:::
:::

::: reference probability-density
# Probability from a density

For a continuous random variable, the probability of an interval is the area under the density
across that interval. The total area is 1.

::: equation
P(a \leq X \leq b) = \int_a^b f(x)\, dx \qquad \int_{-\infty}^{\infty} f(x)\, dx = 1
:::

::: legend
$X$: a continuous random variable
$f(x)$: its probability density function, never negative
$a, b$: the ends of the interval, with $a \leq b$
:::
:::

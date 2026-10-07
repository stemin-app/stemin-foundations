---
title: Spread
---

::: card
The mean says where a distribution sits. It does not say how widely it scatters. Two
distributions with mean 5:

- A always returns 5.
- B returns 0 or 10, each with probability 0.5.

A never moves; B always lands far from 5. You need a number for this **spread**: how far
outcomes typically fall from the mean.
:::

::: card
First try: average the signed distance from the mean. For B, the distances are $0 - 5 = -5$ and
$10 - 5 = +5$:

$$ \frac{(-5) + (+5)}{2} = 0 $$

B is clearly spread out, yet the answer is zero. The positive and negative distances cancel. They
always do: by linearity, $\mathbb{E}[X - \mathbb{E}[X]] = \mathbb{E}[X] - \mathbb{E}[X] = 0$.[Linearity of expectation](reference:expectation-linearity)
:::

::: card
Second try: average the absolute distances. For B, $|0 - 5| = 5$ and $|10 - 5| = 5$, so the
average is 5. This works, but $|u|$ has a sharp corner at $u = 0$, where it has no derivative.
Gradient descent needs derivatives, so the corner is a problem. The square $u^2$ has none.

```plot
x: { var: u, label: "distance from the mean $u$", from: -3, to: 3, ticks: 1, grid: true }
y: { label: "penalty", from: 0, to: 4 }

draw:
  - curve: { is: abs(u), label: "$|u|$" }
  - curve: { is: u^2, accent: true, label: "$u²$" }
```
:::

::: card
Third try: average the squared distances. For B, $(0 - 5)^2 = 25$ and $(10 - 5)^2 = 25$, so the
average is 25. This is the **variance**:[Variance](reference:variance)

$$ \operatorname{Var}(X) = \mathbb{E}\left[(X - \mathbb{E}[X])^2\right] $$

Read it as the expected value of the squared distance from the expected value.
:::

::: card
Squares earn their place four ways. A square is never negative, so nothing cancels. The function
$u^2$ is smooth and has a derivative everywhere. It punishes large distances: 10 away adds 100,
while 2 away adds only 4. And for independent random variables, variances add.
:::

::: card
Three distributions, all with mean 5:

- A, always 5: $(5 - 5)^2 \times 1 = 0$.
- C, 4, 5 or 6 with $\frac{1}{3}$ each: $(1 + 0 + 1) \times \frac{1}{3} = \frac{2}{3} \approx 0.67$.
- B, 0 or 10 with 0.5 each: $25 \times 0.5 + 25 \times 0.5 = 25$.

Move the two outcomes of B to $5 \pm d$. The mean stays at 5, and the variance is $d^2$.

```plot
x: { var: k, label: "outcome", from: 0, to: 10, ticks: 1, grid: true }
y: { label: "$p$", from: 0, to: 0.6 }

inputs:
  - { name: d, min: 0, max: 5, default: 5, step: 0.5, label: "distance d" }

draw:
  - area: { under: 0.5, over: [5 - d - 0.15, 5 - d + 0.15] }
  - area: { under: 0.5, over: [5 + d - 0.15, 5 + d + 0.15] }
  - vline: { at: 5, accent: true, dash: true }
  - point: { at: [5, 0.55], label: "mean" }
```
:::

::: card
A shortcut gives the same number with less work: the mean of the squares minus the square of the
mean.

$$ \operatorname{Var}(X) = \mathbb{E}[X^2] - \left(\mathbb{E}[X]\right)^2 $$

For B: $\mathbb{E}[X^2] = 0^2 \times 0.5 + 10^2 \times 0.5 = 50$, so $\operatorname{Var}(X) = 50 - 5^2 = 25$.
:::

::: card
Variance is in squared units: if $X$ is in metres, its variance is in square metres. The
**standard deviation** returns to the original units:[Standard deviation](reference:standard-deviation)

$$ \sigma = \sqrt{\operatorname{Var}(X)} $$

For B, $\sigma = \sqrt{25} = 5$.
:::

::: card
The standard deviation is a typical distance from the mean, not a typical value. B never returns 5.
It returns 0 or 10, and each time it lands exactly 5 away from the mean. That distance is what
$\sigma = 5$ reports.
:::

::: card
Spread matters the moment you build a network. You choose the initial weights at random, and their
spread decides what happens next. Too large and the activations explode; too small and the
gradients vanish. The Xavier and He initialization schemes exist to set that spread.
:::

::: exercise q1
$X$ is 2 or 8, each with probability 0.5. What are its variance and its standard deviation?

::: answer
$\operatorname{Var}(X) = 9$ and $\sigma = 3$. The mean is 5, and each outcome is 3 away.
:::

::: solution
$$ \mathbb{E}[X] = 2 \times 0.5 + 8 \times 0.5 = 5 $$

$$ \operatorname{Var}(X) = (2 - 5)^2 \times 0.5 + (8 - 5)^2 \times 0.5 = 4.5 + 4.5 = 9 $$

$$ \sigma = \sqrt{9} = 3 $$

∎
:::
:::

::: exercise q2
$X$ is the roll of a fair die. What is $\operatorname{Var}(X)$?

::: answer
$\frac{35}{12} \approx 2.92$. Use the shortcut: $\mathbb{E}[X^2] - (\mathbb{E}[X])^2$.
:::

::: solution
$$ \mathbb{E}[X^2] = \frac{1 + 4 + 9 + 16 + 25 + 36}{6} = \frac{91}{6} $$

$$ (\mathbb{E}[X])^2 = 3.5^2 = \frac{49}{4} $$

$$ \operatorname{Var}(X) = \frac{91}{6} - \frac{49}{4} = \frac{182 - 147}{12} = \frac{35}{12} \approx 2.92 $$

∎
:::
:::

::: exercise q3
$\operatorname{Var}(X) = 2$. What is $\operatorname{Var}(3X)$?

::: answer
$18$. Every distance from the mean triples, so every squared distance grows nine times.
:::

::: solution
$$ \mathbb{E}[3X] = 3\,\mathbb{E}[X] $$

$$ \operatorname{Var}(3X) = \mathbb{E}\left[(3X - 3\,\mathbb{E}[X])^2\right] = 9\,\mathbb{E}\left[(X - \mathbb{E}[X])^2\right] = 9 \times 2 = 18 $$

∎
:::
:::

::: reference variance
# Variance

The variance of a random variable is the expected squared distance from its mean. It equals the
mean of the squares minus the square of the mean.

::: equation
\operatorname{Var}(X) = \mathbb{E}\left[(X - \mu)^2\right] = \mathbb{E}[X^2] - \mu^2 \qquad \mu = \mathbb{E}[X]
:::

::: legend
$\operatorname{Var}(X)$: the variance of $X$, in the units of $X$ squared
$\mu$: the mean of $X$
:::

::: derivation
Expand the square: $(X - \mu)^2 = X^2 - 2\mu X + \mu^2$.
Take the mean of each term, with $\mu$ a constant: $\mathbb{E}[X^2] - 2\mu\,\mathbb{E}[X] + \mu^2$.[Linearity of expectation](reference:expectation-linearity)
Put $\mathbb{E}[X] = \mu$: $\mathbb{E}[X^2] - 2\mu^2 + \mu^2 = \mathbb{E}[X^2] - \mu^2$. ∎
:::
:::

::: reference standard-deviation
# Standard deviation

The standard deviation is the square root of the variance. It measures spread in the units of
the variable itself.

::: equation
\sigma = \sqrt{\operatorname{Var}(X)}
:::

::: legend
$\sigma$: the standard deviation of $X$, in the units of $X$
$\operatorname{Var}(X)$: the variance of $X$
:::
:::

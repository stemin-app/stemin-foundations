---
title: The mean
---

::: card
Roll a die six times and get 2, 5, 3, 6, 1, 4. The average is their sum over their count:

$$ \text{average} = \frac{2 + 5 + 3 + 6 + 1 + 4}{6} = \frac{21}{6} = 3.5 $$
:::

::: card
You can find an average before you roll at all: the value you would get on average over many
rolls. Each face of a fair die has probability $\frac{1}{6}$, so each face adds its value times
$\frac{1}{6}$:

$$ 1 \cdot \tfrac{1}{6} + 2 \cdot \tfrac{1}{6} + 3 \cdot \tfrac{1}{6} + 4 \cdot \tfrac{1}{6} + 5 \cdot \tfrac{1}{6} + 6 \cdot \tfrac{1}{6} = 3.5 $$

This is a **weighted average**, and the weights are the probabilities.
:::

::: card
This theoretical average is the **expectation**, or **expected value**, of $X$. You write it
$\mathbb{E}[X]$, and it is what "the mean" means for a random variable.[Expectation](reference:expectation)

$$ \mathbb{E}[X] = \sum_x x \, p(x) $$
:::

::: card
A loaded die lands on 6 half the time and on each of 1 to 5 a tenth of the time:

$$ \mathbb{E}[X] = (1 + 2 + 3 + 4 + 5) \times 0.1 + 6 \times 0.5 = 1.5 + 3.0 = 4.5 $$

The mean rises above the fair 3.5 because a high outcome is now more likely. Move the slider:
with $P(6) = s$ and the rest shared equally, the mean is $3 + 3s$.

```plot
x: { var: k, label: "face", from: 0, to: 7, ticks: 1 }
y: { label: "$p$", from: 0, to: 1, ticks: 0.25, grid: true }

inputs:
  - { name: s, min: 0, max: 1, default: 0.5, step: 0.05, label: "P(6)" }

let:
  r: (1 - s) / 5

draw:
  - area: { under: r, over: [0.75, 1.25] }
  - area: { under: r, over: [1.75, 2.25] }
  - area: { under: r, over: [2.75, 3.25] }
  - area: { under: r, over: [3.75, 4.25] }
  - area: { under: r, over: [4.75, 5.25] }
  - area: { under: s, over: [5.75, 6.25] }
  - vline: { at: 3 + 3 * s, accent: true, dash: true }
  - point: { at: [3 + 3 * s, 0.9], label: "mean" }
```
:::

::: card
The outcomes need not be numbers on a die. Give each next word of a model a score: cat 10, dog 8,
bird 6, fish 4, with probabilities 0.4, 0.3, 0.2 and 0.1. The expected score is

$$ 10 \times 0.4 + 8 \times 0.3 + 6 \times 0.2 + 4 \times 0.1 = 4 + 2.4 + 1.2 + 0.4 = 8.0 $$
:::

::: card
The mean is **linear**. Scale and add two random variables, and the mean of the result is the same
mix of their means:[Linearity of expectation](reference:expectation-linearity)

$$ \mathbb{E}[aX + bY] = a\,\mathbb{E}[X] + b\,\mathbb{E}[Y] $$

This holds even when $X$ and $Y$ depend on each other, which is why a mean is so easy to work with.
:::

::: exercise q1
A game pays 1 when a coin lands heads and 0 when it lands tails. The coin lands heads with
probability 0.3. What is the expected payout?

::: answer
$0.3$. Only heads adds to the sum: $1 \times 0.3 + 0 \times 0.7$.
:::
:::

::: exercise q2
$X$ takes the values 0, 1 and 2 with probabilities 0.2, 0.5 and 0.3. What is $\mathbb{E}[X]$?

::: answer
$1.1$. Add each value times its probability.
:::

::: solution
$$ \mathbb{E}[X] = 0 \times 0.2 + 1 \times 0.5 + 2 \times 0.3 = 0 + 0.5 + 0.6 = 1.1 $$

∎
:::
:::

::: exercise q3
$X$ is the roll of a fair die. What is $\mathbb{E}[2X + 1]$?

::: answer
$8$. Use linearity: $2 \times 3.5 + 1$.
:::

::: solution
$$ \mathbb{E}[X] = 3.5 $$

$$ \mathbb{E}[2X + 1] = 2\,\mathbb{E}[X] + 1 = 7 + 1 = 8 $$

∎
:::
:::

::: reference expectation
# Expectation

The expectation of a random variable is the average of its values, each weighted by its
probability. For a continuous variable the sum becomes an integral over the density.

::: equation
\mathbb{E}[X] = \sum_x x\, p(x) \qquad \mathbb{E}[X] = \int_{-\infty}^{\infty} x\, f(x)\, dx
:::

::: legend
$\mathbb{E}[X]$: the expectation, or mean, of $X$
$x$: one possible value of $X$
$p(x)$: the probability of $x$, for a discrete variable
$f(x)$: the density at $x$, for a continuous variable
:::
:::

::: reference expectation-linearity
# Linearity of expectation

The mean of a weighted sum of random variables is the weighted sum of their means, whether or not
the variables depend on each other.

::: equation
\mathbb{E}[aX + bY] = a\,\mathbb{E}[X] + b\,\mathbb{E}[Y]
:::

::: legend
$X, Y$: random variables
$a, b$: constant numbers
:::

::: derivation
Let $p(x, y)$ be the probability that $X = x$ and $Y = y$ together.
$\mathbb{E}[aX + bY] = \sum_{x, y} (a x + b y)\, p(x, y)$.[Expectation](reference:expectation)
Split the sum: $a \sum_{x, y} x\, p(x, y) + b \sum_{x, y} y\, p(x, y)$.
Sum out the other variable: $\sum_y p(x, y) = p(x)$ and $\sum_x p(x, y) = p(y)$.
So the sum is $a \sum_x x\, p(x) + b \sum_y y\, p(y) = a\,\mathbb{E}[X] + b\,\mathbb{E}[Y]$.
No step assumes $X$ and $Y$ are independent. ∎
:::
:::

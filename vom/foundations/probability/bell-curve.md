---
title: The bell curve
---

::: card
The **normal distribution**, also called the Gaussian or the bell curve, is the most important
continuous distribution. Most values cluster near the middle, and fewer and fewer appear as you
move away. Its density is[The normal distribution](reference:normal-distribution)

$$ f(x) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right) $$
:::

::: card
Two numbers set the whole curve. The mean $\mu$ places the centre of the bell. The standard
deviation $\sigma$ sets its width. The shaded band, from $\mu - \sigma$ to $\mu + \sigma$, always
holds about 68% of the probability. Widen the bell and it must also flatten: the area stays 1, so
the peak height is $1/(\sigma\sqrt{2\pi})$, about 0.399 when $\sigma = 1$.

```plot
x: { var: x, label: "$x$", from: -5, to: 5, ticks: 1, grid: true }
y: { label: "$f(x)$", from: 0, to: 0.85 }

inputs:
  - { name: mu, min: -2, max: 2, default: 0, step: 0.1, label: "mean μ" }
  - { name: s, min: 0.5, max: 2, default: 1, step: 0.1, label: "standard deviation σ" }

let:
  f: exp(-((x - mu)^2) / (2 * s^2)) / (s * sqrt(2 * pi()))

draw:
  - area: { under: f, over: [mu - s, mu + s], accent: true }
  - curve: f
  - vline: { at: mu, dash: true }
  - point: { at: [mu, 1 / (s * sqrt(2 * pi()))], label: "peak" }
```
:::

::: card
You write $X \sim \mathcal{N}(\mu, \sigma^2)$: "$X$ follows a normal distribution with mean $\mu$
and variance $\sigma^2$". The second slot holds the variance, not the standard deviation. So
$\mathcal{N}(0, 4)$ has $\sigma = 2$.
:::

::: card
The bell appears everywhere because of the **central limit theorem**: add up many independent
random quantities, and the sum takes the shape of a bell, whatever the shape of each part.

One die is uniform: each face equally likely. The sum of 100 dice is a bell centred on
$100 \times 3.5 = 350$.
:::

::: card
The average of $n$ dice tells the same story. It stays centred on 3.5, and its bell narrows as
$n$ grows: its standard deviation is $\sqrt{35/12}\,/\sqrt{n} \approx 1.71/\sqrt{n}$. With 100
dice it is about 0.17.

```plot
x: { var: x, label: "average of $n$ dice", from: 1, to: 6, ticks: 0.5, grid: true }
y: { label: "density", from: 0, to: 2.5 }

inputs:
  - { name: n, min: 4, max: 100, default: 10, step: 1, label: "number of dice n" }

let:
  s: 1.7078 / sqrt(n)
  f: exp(-((x - 3.5)^2) / (2 * s^2)) / (s * sqrt(2 * pi()))

draw:
  - curve: { is: f, accent: true }
  - vline: { at: 3.5, dash: true }
```
:::

::: card
Each neuron of a network computes a sum:

$$ \text{output} = w_1 x_1 + w_2 x_2 + \cdots + w_n x_n $$

Even when the single weights and inputs follow odd distributions, a sum of many of them tends
toward a bell. This is why the normal distribution runs through the analysis of networks.
:::

::: card
The **standard normal** $\mathcal{N}(0, 1)$ has mean 0 and standard deviation 1. Any normal
variable becomes standard when you subtract its mean and divide by its standard deviation:[Standardizing a variable](reference:standard-score)

$$ Z = \frac{X - \mu}{\sigma} $$

If $X \sim \mathcal{N}(\mu, \sigma^2)$, then $Z \sim \mathcal{N}(0, 1)$.
:::

::: exercise q1
$X \sim \mathcal{N}(10, 4)$. What is the standard score $z$ of the value $x = 14$?

::: answer
$z = 2$. The variance is 4, so $\sigma = 2$, and 14 is two of those above 10.
:::

::: solution
$$ \sigma = \sqrt{4} = 2 $$

$$ z = \frac{14 - 10}{2} = 2 $$

∎
:::
:::

::: exercise q2
What is the height of the standard normal density at its peak, $x = 0$?

::: answer
$\frac{1}{\sqrt{2\pi}} \approx 0.399$. At $x = \mu$ the exponential is 1.
:::

::: solution
$$ f(0) = \frac{1}{\sqrt{2\pi \cdot 1}} \exp(0) = \frac{1}{\sqrt{2\pi}} = \frac{1}{2.507} \approx 0.399 $$

∎
:::
:::

::: exercise q3
$X \sim \mathcal{N}(0, 9)$. What is the standard deviation of $X$?

::: answer
$3$. The second slot is the variance, $\sigma^2 = 9$.
:::
:::

::: reference normal-distribution
# The normal distribution

A normal random variable with mean $\mu$ and variance $\sigma^2$ has a bell shaped density,
symmetric about $\mu$. About 68% of its probability lies within one standard deviation of the
mean.

::: equation
f(x) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right) \qquad X \sim \mathcal{N}(\mu, \sigma^2)
:::

::: legend
$f(x)$: the density at $x$
$\mu$: the mean, the centre of the bell
$\sigma$: the standard deviation, the width of the bell
$\sigma^2$: the variance
:::
:::

::: reference standard-score
# Standardizing a variable

Subtract the mean and divide by the standard deviation, and the result has mean 0 and variance 1.
A normal variable becomes the standard normal.

::: equation
Z = \frac{X - \mu}{\sigma} \qquad \mathbb{E}[Z] = 0 \qquad \operatorname{Var}(Z) = 1
:::

::: legend
$X$: a random variable with mean $\mu$ and standard deviation $\sigma$
$Z$: the standardized variable, or standard score
:::

::: derivation
$\mathbb{E}[Z] = \frac{1}{\sigma}\left(\mathbb{E}[X] - \mu\right) = \frac{1}{\sigma}(\mu - \mu) = 0$.[Linearity of expectation](reference:expectation-linearity)
$Z - \mathbb{E}[Z] = \frac{X - \mu}{\sigma}$, so $\operatorname{Var}(Z) = \frac{1}{\sigma^2}\mathbb{E}\left[(X - \mu)^2\right]$.[Variance](reference:variance)
$\mathbb{E}\left[(X - \mu)^2\right] = \sigma^2$, so $\operatorname{Var}(Z) = \frac{\sigma^2}{\sigma^2} = 1$. ∎
:::
:::

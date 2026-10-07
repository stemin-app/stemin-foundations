---
title: Fitting a scaling law
---

::: card
A scaling law is measured, so you fit it to measurements. Train models across at least two or
three orders of magnitude, such as 10M, 30M, 100M, 300M, 1B and 3B parameters. Measure each one's
loss on a held-out test set, large enough that noise is small, in nats throughout.
:::

::: card
Fit in log space. A power law $L = aN^{-\alpha}$ is a line there
([power law](reference:power-law)):

$$ \log L_i = \beta_0 + \beta_1 \log N_i + \epsilon_i $$

Ordinary least squares gives the slope $\hat{\beta}_1 = -\hat{\alpha}$, the exponent, and the
intercept $\hat{\beta}_0 = \log \hat{a}$, the prefactor
([least squares in log space](reference:log-log-least-squares)).
:::

::: card
Try it by eye. The six points are measured losses. The blue line pivots on their centre; set its
slope. A slope near 0.077 runs through all of them. Least squares on these points gives exactly
$\hat{\alpha} = 0.0774$.

```plot
x: { var: N, label: "parameters N", from: 3e6, to: 1e10, scale: log, ticks: 1, grid: true }
y: { label: "test loss in nats", from: 1.8, to: 4, scale: log, grid: true }

inputs:
  - { name: al, min: 0.02, max: 0.15, default: 0.04, step: 0.002, label: "the exponent α" }

draw:
  - points: [[1e7, 3.412], [3e7, 3.076], [1e8, 2.858], [3e8, 2.572], [1e9, 2.39], [3e9, 2.177]]
  - curve: { is: 2.716 * (N / 1.732e8) ^ (-al), accent: true }
```
:::

::: card
Then validate. Leave one or two points out of the fit, predict their loss from it, and compare.
Agreement within a few percent supports the law. Then estimate the uncertainty of the exponent,
for example by resampling the points and fitting again.
:::

::: card
Three traps. A range under two orders of magnitude gives unreliable exponents, because curvature
from the floor or from saturation can pass for a different slope. The smallest models may not train
to convergence: leave them out if they sit off the line. And a test set the largest model has
partly memorized makes its loss look too good.
:::

::: card
The uncertainty grows as you extrapolate. Fit $\alpha = 0.08 \pm 0.01$ with $N_c = 8.8 \times 10^{13}$,
and predict at $N = 10^{12}$: $88^{0.08} \approx 1.43$, but $88^{0.07} \approx 1.37$ and
$88^{0.09} \approx 1.50$. A common rule is to extrapolate at most one order of magnitude past the
largest run you trained. Two or more is speculation.

```plot
x: { var: N, label: "parameters N", from: 1e8, to: 1e13, scale: log, ticks: 1, grid: true }
y: { label: "predicted loss in nats", from: 1, to: 4, scale: log, grid: true }

inputs:
  - { name: u, min: 0, max: 0.03, default: 0.01, step: 0.005, label: "uncertainty in α" }

draw:
  - curve: { is: (8.8e13 / N) ^ 0.08, accent: true, label: "α = 0.08" }
  - curve: { is: (8.8e13 / N) ^ (0.08 + u), dash: true }
  - curve: { is: (8.8e13 / N) ^ (0.08 - u), dash: true }
  - vline: { at: 1e10, dash: true, label: "largest run" }
```
:::

::: exercise q1
A fit in log space gives $\log_{10} L = 1.07 - 0.077 \log_{10} N$. What are $a$ and $\alpha$ in
$L = aN^{-\alpha}$?

::: answer
$a = 10^{1.07} \approx 11.7$ and $\alpha = 0.077$.
:::
:::

::: exercise q2
With $L = (8.8 \times 10^{13}/N)^{\alpha}$, what loss do $\alpha = 0.07$ and $\alpha = 0.09$ predict at
$N = 8.8 \times 10^{10}$?

::: answer
About 1.62 and 1.86 nats. The ratio is 1000, and $1000^{0.07} = 10^{0.21}$, $1000^{0.09} = 10^{0.27}$.
:::
:::

::: reference log-log-least-squares
# Least squares in log space

To fit a power law $L = aN^{-\alpha}$, fit a straight line to $(\log N_i, \log L_i)$ by ordinary
least squares. The slope is $-\alpha$ and the intercept is $\log a$.

::: equation
\hat{\alpha} = -\frac{\sum_i (u_i - \bar{u})(v_i - \bar{v})}{\sum_i (u_i - \bar{u})^2}, \qquad \log \hat{a} = \bar{v} + \hat{\alpha}\,\bar{u}, \qquad u_i = \log N_i, \; v_i = \log L_i
:::

::: legend
$N_i, L_i$: the size and the measured loss of model $i$
$u_i, v_i$: their logarithms
$\bar{u}, \bar{v}$: the means of the $u_i$ and of the $v_i$
:::

::: derivation
Take logs of the power law: $v = \log a - \alpha u$, a line with slope $-\alpha$.[power law](reference:power-law)
Minimize $S = \sum_i (v_i - \beta_0 - \beta_1 u_i)^2$ over $\beta_0, \beta_1$.
$\frac{\partial S}{\partial \beta_0} = 0$ gives $\beta_0 = \bar{v} - \beta_1\bar{u}$.[partial derivative](reference:partial-derivative)
Put it in $\frac{\partial S}{\partial \beta_1} = 0$: $\sum_i (u_i - \bar{u})\big((v_i - \bar{v}) - \beta_1(u_i - \bar{u})\big) = 0$, so $\beta_1 = \frac{\sum (u_i - \bar{u})(v_i - \bar{v})}{\sum (u_i - \bar{u})^2}$.
Then $\hat{\alpha} = -\beta_1$ and $\log\hat{a} = \beta_0 = \bar{v} + \hat{\alpha}\bar{u}$. ∎
:::
:::

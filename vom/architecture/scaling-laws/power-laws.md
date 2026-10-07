---
title: What a power law is
---

::: card
A power law ties one quantity to a fixed power of another:

$$ y = a\,x^b $$

The constant $a$ sets the size and the exponent $b$ sets the shape. The loss of a language model
falls as it grows, so in scaling laws the exponent is negative. The usual way to write it is
$L = (N_c/N)^\alpha$, which is a power law with $a = N_c^\alpha$ and $b = -\alpha$.
[Power law](reference:power-law)
:::

::: card
Double $x$ and $y$ is multiplied by $2^b$, whatever $x$ you start from. The factor depends on
the exponent alone.

Compare the other two shapes you know. In a linear law $y = a x$, doubling $x$ doubles $y$. In
an exponential $y = a\,b^x$, each unit step in $x$ multiplies $y$ by $b$, so it grows or decays
very fast. A power law with $0 < b < 1$ sits between them: doubling $x$ multiplies $y$ by a
fixed factor between 1 and 2. You keep gaining, and each doubling gains less.
:::

::: card
Take $L = 20\,N^{-0.1}$ and step $N$ by factors of ten:

$$
\begin{array}{c|c}
N & L \\ \hline
10^6 & 5.02 \\
10^7 & 3.99 \\
10^8 & 3.17 \\
10^9 & 2.52 \\
10^{10} & 2.00
\end{array}
$$

Every step multiplies the loss by $10^{-0.1} = 0.794$, a drop of about 21%. The absolute drop
shrinks: 1.03 on the first step, 0.52 on the last. This is the diminishing return of a power law.
:::

::: card
On ordinary axes a falling power law drops steeply, then flattens into a long tail. Drag the
exponent: a larger $\alpha$ bends the curve down faster, a smaller one leaves it almost flat.

```plot
x: { var: N, label: "$N$", from: 1, to: 100, ticks: 10, grid: true }
y: { label: "$L$", from: 0, to: 10, ticks: 2, grid: true }

inputs:
  - { name: alpha, min: 0.1, max: 1, default: 0.4, step: 0.05, label: "the exponent α" }

draw:
  - curve: { is: 10 * N^(-alpha), accent: true, label: "L = 10 N^(−α)" }
```
:::

::: card
Take the logarithm of both sides and the curve becomes a line:

$$ \log y = \log a + b \log x $$

On log-log axes a power law is straight, and its slope is the exponent. Across one decade of $N$
the line below falls $\alpha$ decades of $L$. Drag $\alpha$: the line tilts and stays straight.

```plot
x: { var: N, label: "$N$", from: 1, to: 1000, scale: log, ticks: 1, grid: true }
y: { label: "$L$", from: 0.01, to: 20, scale: log, ticks: 1, grid: true }

inputs:
  - { name: alpha, min: 0.1, max: 1, default: 0.4, step: 0.05, label: "the exponent α" }

draw:
  - curve: { is: 10 * N^(-alpha), accent: true }
  - curve: { is: 10 * 10^(-alpha), over: [10, 100], dash: true }
  - param: { var: s, over: [0, 1], x: 100, y: "10 * 10^(-alpha * (1 + s))", dash: true, label: "$α$ decades" }
  - point: { at: [10, "10 * 10^(-alpha)"], label: "one decade across" }
```
:::

::: card
A power law has no scale of its own. Multiply $x$ by any $k$ and $y$ is multiplied by $k^b$, the
same factor wherever you are. Zoom into any stretch of the log-log line and it looks like every
other stretch.

An exponential $y = e^{-x/x_0}$ is different. Its behaviour changes around $x = x_0$, its
characteristic scale. On log-log axes it bends and dives, while the power law $y = x^{-1}$ stays
straight. Drag $x_0$ and the bend moves with it.

```plot
x: { var: x, label: "$x$", from: 0.01, to: 100, scale: log, ticks: 1, grid: true }
y: { label: "$y$", from: 0.001, to: 100, scale: log, ticks: 1, grid: true }

inputs:
  - { name: x0, min: 0.1, max: 10, default: 1, step: 0.1, label: "the scale x₀" }

draw:
  - curve: { is: 1 / x, label: "$x⁻¹$" }
  - curve: { is: exp(-x / x0), accent: true, label: "exp(−x/x₀)" }
  - vline: { at: x0, dash: true }
```
:::

::: card
This is why every scaling result is drawn on log-log axes. A straight line confirms a power law,
and its slope reads off the exponent. Any other shape curves. The axes also hold many orders of
magnitude on one page.

Small slopes matter. A slope of $-0.076$ means doubling $N$ multiplies $L$ by
$2^{-0.076} = 0.949$, a 5.1% drop. A slope of $-0.1$ gives $2^{-0.1} = 0.933$, a 6.7% drop. Over
many doublings the gap compounds.
:::

::: exercise power-law-hundredfold
The loss follows $L = 20\,N^{-0.1}$. By what factor does the loss change when $N$ grows from
$10^8$ to $10^{10}$?

::: answer
It is multiplied by $10^{-0.2} \approx 0.631$, from 3.17 to 2.00. A factor of 100 in $N$ gives $100^{-0.1}$.
:::

::: solution
$$ \frac{L(10^{10})}{L(10^8)} = \left(\frac{10^{10}}{10^8}\right)^{-0.1} = 100^{-0.1} = 10^{-0.2} = 0.631 $$

$$ L(10^8) = 20 \times 10^{-0.8} = 3.17 \qquad L(10^{10}) = 20 \times 10^{-1} = 2.00 $$

∎
:::
:::

::: exercise power-law-slope
On log-log axes a straight line falls 0.3 decades of $y$ for every decade of $x$. What is the
exponent, and by what factor does doubling $x$ multiply $y$?

::: answer
$b = -0.3$, and doubling multiplies $y$ by $2^{-0.3} \approx 0.812$. The slope on log-log axes is the exponent.
:::

::: solution
Slope $= \Delta \log y / \Delta \log x = -0.3 / 1 = -0.3$, so $b = -0.3$.

$$ 2^{-0.3} = e^{-0.3 \times 0.693} = e^{-0.208} = 0.812 $$

∎
:::
:::

::: exercise power-law-square-root
The quantity $y = 4\sqrt{x}$ is a power law. What is its exponent, and by what factor does $y$
grow when $x$ doubles?

::: answer
$b = 0.5$, and $y$ grows by $2^{0.5} \approx 1.414$. The constant 4 does not enter the factor.
:::
:::

::: reference power-law
# Power law

A power law makes one quantity proportional to a fixed power of another. On log-log axes it is a
straight line whose slope is the exponent, and doubling $x$ multiplies $y$ by $2^b$ everywhere.

::: equation
y = a\,x^b \qquad \log y = \log a + b \log x \qquad \frac{y(2x)}{y(x)} = 2^b
:::

::: legend
$y$: the dependent quantity, such as the loss
$x$: the independent quantity, such as the parameter count
$a$: the prefactor, a positive constant
$b$: the exponent, negative for a loss that falls with scale
:::

::: derivation
Start from $y = a\,x^b$ with $a > 0$ and $x > 0$.
Take the logarithm of both sides: $\log y = \log\left(a\,x^b\right)$.
The log of a product is the sum of the logs: $\log y = \log a + \log x^b$.
The log of a power brings the exponent down: $\log y = \log a + b \log x$.
This is a line in $(\log x, \log y)$ with slope $b$ and intercept $\log a$.
For the doubling factor: $y(2x) / y(x) = a\,(2x)^b / (a\,x^b) = 2^b$, free of $x$. ∎
:::
:::

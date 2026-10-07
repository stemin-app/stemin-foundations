---
title: Activation functions and their slopes
---

::: card
A neural network bends its signals with a few fixed nonlinear functions, the **activation
functions**. Training needs the derivative of each one, because every gradient passes through
them. Four of them come back again and again: the sigmoid, the hyperbolic tangent, ReLU and
GELU.
:::

::: card
The **sigmoid** squashes any input into $(0, 1)$:

$$ \sigma(x) = \frac{1}{1 + e^{-x}}, \qquad \frac{d\sigma}{dx} = \sigma(x)\,\big(1 - \sigma(x)\big) $$

The derivative is built from the output itself, so a network that has computed $\sigma(x)$ gets
its slope almost for free ([sigmoid](reference:sigmoid)).
:::

::: card
Move the point along the sigmoid. The dashed curve is the slope. It peaks at $1/4$ when $x = 0$,
where $\sigma = 1/2$. Far out on either side the sigmoid flattens, and the slope falls toward 0:
the function **saturates**. A gradient that passes through a saturated sigmoid nearly vanishes.

```plot
x: { var: x, label: "$x$", from: -8, to: 8, ticks: 2, grid: true }
y: { label: "$σ(x)$", from: -0.1, to: 1.1 }

inputs:
  - { name: x0, min: -7, max: 7, default: 1.5, step: 0.1, label: "the point x" }

let:
  s0: 1 / (1 + exp(-x0))

draw:
  - curve: { is: 1 / (1 + exp(-x)), accent: true }
  - curve: { is: (1 / (1 + exp(-x))) * (1 - 1 / (1 + exp(-x))), dash: true, label: "slope" }
  - curve: { is: s0 + s0 * (1 - s0) * (x - x0), over: [x0 - 2, x0 + 2] }
  - point: { at: [x0, s0], label: "tangent" }
```
:::

::: card
The **hyperbolic tangent** squashes into $(-1, 1)$, centred on zero:

$$ \tanh(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}, \qquad \frac{d\tanh}{dx} = 1 - \tanh^2(x) $$

Its slope peaks at 1, four times the sigmoid's. It saturates the same way: at $x = 3$ the slope is
already below $0.01$.

```plot
x: { var: x, label: "$x$", from: -5, to: 5, ticks: 1, grid: true }
y: { label: "$tanh(x)$", from: -1.2, to: 1.2 }

draw:
  - hline: { at: 0 }
  - curve: { is: tanh(x), accent: true }
  - curve: { is: 1 - tanh(x) ^ 2, dash: true, label: "slope" }
```
:::

::: card
**ReLU**, the rectified linear unit, keeps positive inputs and zeroes the rest:

$$ \mathrm{ReLU}(x) = \max(0, x), \qquad \frac{d\,\mathrm{ReLU}}{dx} = \begin{cases} 1 & x > 0 \\ 0 & x < 0 \end{cases} $$

At $x = 0$ the slope is undefined, and in practice it is set to 0 or 1. For positive inputs it
never saturates: the slope stays exactly 1 ([ReLU](reference:relu)).
:::

::: card
**GELU**, the Gaussian error linear unit, is a smooth ReLU used in GPT:

$$ \mathrm{GELU}(x) = x\,\Phi(x) $$

$\Phi$ is the cumulative distribution of the standard normal. It weighs each input by the
chance that a normal variable falls below it. Compare it with ReLU below: it dips slightly below
zero near $x = -0.75$, then joins ReLU for large $|x|$.

```plot
x: { var: x, label: "$x$", from: -4, to: 3, ticks: 1, grid: true }
y: { label: "output", from: -0.5, to: 3 }

let:
  c: sqrt(2 / pi())

draw:
  - curve: { is: "max(0, x)", dash: true, label: "ReLU" }
  - curve: { is: 0.5 * x * (1 + tanh(c * (x + 0.044715 * x ^ 3))), accent: true, label: "GELU" }
```
:::

::: exercise q1
What is $\frac{d\sigma}{dx}$ at $x = 0$?

::: answer
$1/4$. $\sigma(0) = 1/2$, and $\frac{1}{2}\left(1 - \frac{1}{2}\right) = \frac{1}{4}$.
:::
:::

::: exercise q2
$\sigma(x) = 0.9$ at some $x$. What is the slope there?

::: answer
$0.09$. The slope is $0.9 \times 0.1$.
:::
:::

::: exercise q3
$\tanh(x) = 0.8$ at some $x$. What is the slope there?

::: answer
$0.36$. The slope is $1 - 0.8^2 = 1 - 0.64$.
:::
:::

::: exercise q4
A gradient of 5 arrives at a ReLU whose input was $-2$. What gradient passes to the input?

::: answer
0. The slope of ReLU is 0 for negative inputs, so $5 \times 0 = 0$.
:::
:::

::: reference sigmoid
# Sigmoid

The sigmoid maps any real number into $(0, 1)$. Its derivative is a product of its own output
and one minus it, with a maximum of $1/4$ at $x = 0$.

::: equation
\sigma(x) = \frac{1}{1 + e^{-x}} \qquad \frac{d\sigma}{dx} = \sigma(x)\big(1 - \sigma(x)\big)
:::

::: legend
$x$: the input, any real number
$\sigma(x)$: the output, between 0 and 1
:::

::: derivation
Write $\sigma(x) = (1 + e^{-x})^{-1}$.
Differentiate the outer power and the inner exponential: $\frac{d\sigma}{dx} = -(1 + e^{-x})^{-2} \cdot (-e^{-x})$.[chain rule](reference:chain-rule)
Simplify: $\frac{d\sigma}{dx} = \frac{e^{-x}}{(1 + e^{-x})^2} = \frac{1}{1 + e^{-x}} \cdot \frac{e^{-x}}{1 + e^{-x}}$.
The second factor is $1 - \frac{1}{1 + e^{-x}} = 1 - \sigma(x)$.
So $\frac{d\sigma}{dx} = \sigma(x)(1 - \sigma(x))$. ∎
:::
:::

::: reference relu
# ReLU

The rectified linear unit passes positive inputs unchanged and sends the rest to zero. Its
derivative is 1 for positive inputs and 0 for negative ones; at zero it is set by convention.

::: equation
\mathrm{ReLU}(x) = \max(0, x) \qquad \frac{d\,\mathrm{ReLU}}{dx} = \begin{cases} 1 & x > 0 \\ 0 & x < 0 \end{cases}
:::

::: legend
$x$: the input, any real number
:::

::: derivation
For $x > 0$, $\mathrm{ReLU}(x) = x$ near $x$, whose slope is 1.
For $x < 0$, $\mathrm{ReLU}(x) = 0$ near $x$, whose slope is 0.
At $x = 0$, the slope from the left is 0 and from the right is 1, so no derivative exists. ∎
:::
:::

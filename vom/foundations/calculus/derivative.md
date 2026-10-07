---
title: The rate of change at a point
---

::: card
A function $f: \mathbb{R} \to \mathbb{R}$ maps a real number to a real number. To ask how fast it
changes, pick two inputs, $x$ and $x + h$, and divide the change in output by the change in input:

$$ \text{average rate of change} = \frac{f(x + h) - f(x)}{h} $$

This is the slope of the **secant line**, the straight line through $(x, f(x))$ and
$(x + h, f(x + h))$.
:::

::: card
The average rate depends on $h$. For $f(x) = x^2$ at $x = 3$, the secant slope is $6 + h$. Shrink
$h$ and the secant pivots on the point $(3, 9)$. It settles on the dashed line of slope 6, which
touches the curve at that one point.

```plot
x: { var: x, label: "$x$", from: 0, to: 6, ticks: 1, grid: true }
y: { label: "$f(x) = x²$", from: 0, to: 30 }

inputs:
  - { name: h, min: 0.05, max: 2.5, default: 2, step: 0.05, label: "the step h" }

draw:
  - curve: x^2
  - curve: { is: 9 + 6 * (x - 3), dash: true, label: tangent }
  - curve: { is: 9 + (6 + h) * (x - 3), accent: true, label: secant }
  - point: { at: [3, 9], label: "$(3, 9)$" }
  - point: { at: [3 + h, (3 + h)^2], label: "$(3 + h, (3 + h)²)$" }
```
:::

::: card
You cannot set $h = 0$: that gives $\frac{0}{0}$. Take the limit instead. The result is the
**derivative** of $f$ at $x$, the instantaneous rate of change.
[Derivative](reference:derivative)

$$ \frac{df}{dx} = \lim_{h \to 0} \frac{f(x + h) - f(x)}{h} $$
:::

::: card
Compute it for $f(x) = x^2$ at $x = 3$. Expand, cancel, then let $h$ go to zero:

$$ \frac{df}{dx}\bigg|_{x=3} = \lim_{h \to 0} \frac{(3+h)^2 - 9}{h} = \lim_{h \to 0} \frac{6h + h^2}{h} = \lim_{h \to 0} (6 + h) = 6 $$
:::

::: card
The numbers agree. With $h = 1$ the average rate is $\frac{16 - 9}{1} = 7$. With $h = 0.1$ it is
$\frac{9.61 - 9}{0.1} = 6.1$. With $h = 0.01$ it is $6.01$, and with $h = 0.001$ it is $6.001$.
At $x = 3$, $x^2$ grows at 6 units of output for each unit of input.
:::

::: card
The same steps work at any $x$:

$$ \frac{(x + h)^2 - x^2}{h} = \frac{2xh + h^2}{h} = 2x + h \;\longrightarrow\; 2x $$

So $\frac{d}{dx} x^2 = 2x$. At $x = 3$ this gives $6$, as before.
:::

::: card
The derivative is the slope of the **tangent line**, the line that touches the curve at the point
and runs in the same direction. Move the point along $x^2$. Left of zero the tangent falls, right
of zero it rises, and at $a = 0$ it lies flat, because its slope $2a$ is zero.

```plot
x: { var: x, label: "$x$", from: -3, to: 3, ticks: 1, grid: true }
y: { label: "$x²$", from: -2, to: 9 }

inputs:
  - { name: a, min: -2.5, max: 2.5, default: 1.5, step: 0.1, label: "the point a" }

draw:
  - curve: x^2
  - curve: { is: a^2 + 2 * a * (x - a), accent: true, label: "slope $2a$" }
  - point: { at: [a, a^2], label: "$(a, a²)$" }
```
:::

::: exercise derivative-square-negative
What is the derivative of $x^2$ at $x = -2$?

::: answer
$-4$. Use $\frac{d}{dx} x^2 = 2x$.
:::
:::

::: exercise secant-slope-half
What is the slope of the secant line of $f(x) = x^2$ from $x = 3$ to $x = 3.5$?

::: answer
$6.5$. The secant slope from $3$ with step $h$ is $6 + h$.
:::

::: solution
$$ \frac{f(3.5) - f(3)}{0.5} = \frac{12.25 - 9}{0.5} = \frac{3.25}{0.5} = 6.5 $$

∎
:::
:::

::: exercise derivative-cube-first-principles
Use the limit definition to find the derivative of $f(x) = x^3$ at $x = 2$.

::: answer
$12$. Expand $(2 + h)^3$, cancel $8$, divide by $h$.
:::

::: solution
$$ (2 + h)^3 = 8 + 12h + 6h^2 + h^3 $$

$$ \frac{(2 + h)^3 - 8}{h} = 12 + 6h + h^2 $$

$$ \lim_{h \to 0} (12 + 6h + h^2) = 12 $$

∎
:::
:::

::: reference derivative
# The derivative

The derivative of $f$ at $x$ is the limit of the secant slope as the step shrinks to zero. It is
the instantaneous rate of change of $f$, and the slope of the tangent line at $(x, f(x))$.

::: equation
\frac{df}{dx} = \lim_{h \to 0} \frac{f(x + h) - f(x)}{h}
:::

::: legend
$f$: a function from real numbers to real numbers
$x$: the point where the rate is measured
$h$: the step between the two inputs of the secant
:::
:::

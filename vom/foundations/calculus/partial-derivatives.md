---
title: Slopes along one axis
---

::: card
A neural network has millions of parameters, its weights. A loss function measures how wrong the
network's predictions are, and it depends on all of them at once:

$$ L = L(w_1, w_2, \ldots, w_{1000000}) $$

You want weights that make $L$ small.
:::

::: card
Trying every combination is hopeless. Even if each of a million weights could take only 10
values, there would be $10^{1000000}$ combinations, far more than the atoms in the universe.
Instead you start somewhere, find which direction is downhill, take a step that way, and repeat.
That plan needs one answer at every point: which direction lowers the loss fastest?
:::

::: card
Start with two variables. A function $f(x, y)$ is a landscape: above each point $(x, y)$ of the
ground, the surface stands at height $z = f(x, y)$. You stand on it at $(x_0, y_0)$ and want to go
downhill as fast as you can.
:::

::: card
Walk due east: $x$ increases, $y$ stays fixed. The slope you feel is the **partial derivative
with respect to $x$**, $\frac{\partial f}{\partial x}$. Walk due north: $y$ increases, $x$ stays
fixed. The slope is $\frac{\partial f}{\partial y}$.
[Partial derivative](reference:partial-derivative)
:::

::: card
To compute $\frac{\partial f}{\partial x}$, treat every other variable as a constant and
differentiate as usual. Take $f(x, y) = x^2 + y^2$, a bowl with its bottom at the origin:

$$ \frac{\partial f}{\partial x} = 2x, \qquad \frac{\partial f}{\partial y} = 2y $$
:::

::: card
Fix $y$ at $y_0$ and you cut a slice through the bowl along $x$: the parabola $x^2 + y_0^2$.
The partial derivative is the slope of that slice. Raise $y_0$ and the slice lifts, but its slope
at $x_0$ stays $2x_0$, because $y$ only adds a constant.

```plot
x: { var: x, label: "$x$, with $y = y₀$ held fixed", from: -5, to: 5, ticks: 1, grid: true }
y: { label: "$f(x, y₀)$", from: 0, to: 45 }

inputs:
  - { name: x0, min: -4, max: 4, default: 3, step: 0.1, label: "the point x₀" }
  - { name: y0, min: -4, max: 4, default: 4, step: 0.1, label: "the fixed y₀" }

let:
  h: x0^2 + y0^2

draw:
  - curve: { is: x^2, dash: true, label: "the slice $y₀ = 0$" }
  - curve: x^2 + y0^2
  - curve: { is: h + 2 * x0 * (x - x0), over: [x0 - 1.5, x0 + 1.5], accent: true, label: "slope $2x₀$" }
  - point: { at: [x0, h], label: "$(x₀, y₀)$" }
```
:::

::: card
At the point $(3, 4)$, $\frac{\partial f}{\partial x} = 6$: walking east, you climb at slope 6.
And $\frac{\partial f}{\partial y} = 8$: walking north, you climb at slope 8. At the origin both
are $0$. The bowl is flat at its bottom and grows steeper as you move away.
:::

::: card
The formal definition is the ordinary derivative along a slice. Only $x$ moves; $y$ stays as it
is:

$$ \frac{\partial f}{\partial x} = \lim_{h \to 0} \frac{f(x + h, y) - f(x, y)}{h} $$
:::

::: exercise partial-product
For $f(x, y) = x^2 y + 3y$, find $\frac{\partial f}{\partial x}$ and $\frac{\partial f}{\partial y}$
at $(2, 1)$.

::: answer
$\frac{\partial f}{\partial x} = 4$ and $\frac{\partial f}{\partial y} = 7$. Hold the other
variable constant.
:::

::: solution
$$ \frac{\partial f}{\partial x} = 2xy = 2 \cdot 2 \cdot 1 = 4 $$

$$ \frac{\partial f}{\partial y} = x^2 + 3 = 4 + 3 = 7 $$

∎
:::
:::

::: exercise partial-exponential
For $f(x, y) = e^{xy}$, what is $\frac{\partial f}{\partial x}$ at $(0, 2)$?

::: answer
$2$. Treat $y$ as a constant: $\frac{\partial f}{\partial x} = y e^{xy}$.
:::

::: solution
$$ \frac{\partial f}{\partial x} = y e^{xy} = 2 e^{0} = 2 $$

∎
:::
:::

::: exercise partial-bowl-negative
For $f(x, y) = x^2 + y^2$ at $(-1, 2)$, which of the two walks climbs: due east or due north?

::: answer
Due north. $\frac{\partial f}{\partial x} = -2$, so east goes down; $\frac{\partial f}{\partial y} = 4$,
so north goes up.
:::
:::

::: reference partial-derivative
# Partial derivative

The partial derivative of $f$ with respect to $x$ is the derivative along $x$ with every other
variable held fixed: the slope of the slice through the surface in the $x$ direction.

::: equation
\frac{\partial f}{\partial x} = \lim_{h \to 0} \frac{f(x + h, y) - f(x, y)}{h}
:::

::: legend
$f$: a function of several variables
$x$: the variable that moves
$y$: every other variable, held fixed
$h$: the step in x
:::

::: derivation
Fix $y$ and define the one variable function $g(x) = f(x, y)$.
The partial derivative is the ordinary derivative of $g$: $\frac{\partial f}{\partial x} = \frac{dg}{dx}$.[Derivative](reference:derivative)
So every rule for one variable applies, with the other variables treated as constants. ∎
:::
:::

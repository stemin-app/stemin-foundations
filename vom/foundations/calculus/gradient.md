---
title: The gradient points uphill
---

::: card
The **gradient** collects every partial derivative into one vector:

$$ \nabla f = \begin{bmatrix} \frac{\partial f}{\partial x} \\ \frac{\partial f}{\partial y} \end{bmatrix} $$

For $f(x, y) = x^2 + y^2$ it is $\nabla f = [2x, 2y]^T$. At the point $(3, 4)$ it is
$[6, 8]^T$.
[Gradient](reference:gradient)
:::

::: card
The gradient lives on the ground, in the input plane, not in the 3D space of the surface. It
points the way to walk to climb most steeply, and its length is that steepest slope. On the bowl
$x^2 + y^2$ it points straight away from the bottom, across the rings of equal height. Move the
point around and watch it.

```plot
x: { var: t, label: "$x$", from: -8, to: 8, ticks: 2, grid: true }
y: { label: "$y$", from: -8, to: 8 }

inputs:
  - { name: r, min: 1, max: 5, default: 5, step: 0.5, label: "the distance from the bottom" }
  - { name: phi, min: 0, max: 6.28, default: 0.93, step: 0.01, label: "the angle around the bowl" }

let:
  px: r * cos(phi)
  py: r * sin(phi)

draw:
  - param: { var: s, over: [0, 2 * pi()], x: 2 * cos(s), y: 2 * sin(s), dash: true }
  - param: { var: s, over: [0, 2 * pi()], x: 4 * cos(s), y: 4 * sin(s), dash: true }
  - param: { var: s, over: [0, 2 * pi()], x: r * cos(s), y: r * sin(s) }
  - param: { var: s, over: [0, 1], x: px * (1 + 0.5 * s), y: py * (1 + 0.5 * s), accent: true, label: "$∇ f$" }
  - param: { var: s, over: [0, 1], x: px * (1 - 0.5 * s), y: py * (1 - 0.5 * s), dash: true, label: "$-∇ f$" }
  - point: { at: [px, py] }
  - point: { at: [0, 0], label: "minimum" }
```

Each arrow is drawn at a quarter of its length. At $(3, 4)$ the gradient is $[6, 8]^T$, of
length $10$.
:::

::: card
At $(3, 4)$, north has slope 8 and east has slope 6. North looks best, so why would a mix of
the two climb faster? Because a diagonal step does not dilute the good direction with the worse
one. It collects height from both at once.
:::

::: card
Compare unit steps, each of length 1. The height gained is each slope times the distance moved
along it:

- north $[0, 1]$: $0 \times 6 + 1 \times 8 = 8$;
- east $[1, 0]$: $1 \times 6 + 0 \times 8 = 6$;
- along the gradient $[0.6, 0.8]$: $0.6 \times 6 + 0.8 \times 8 = 3.6 + 6.4 = 10$.

The diagonal step takes 3.6 from $x$ and 6.4 from $y$, more than either axis alone.
:::

::: card
The budget is a circle, $u_1^2 + u_2^2 = 1$, not a line $u_1 + u_2 = 1$. Tilt from north
$[0, 1]$ to $[0.6, 0.8]$: you give up only 0.2 of $y$ movement and gain 0.6 of $x$ movement. Near
an axis, a small sacrifice in one direction buys a large gain in the other.
:::

::: card
The best direction is proportional to the payoffs $[6, 8]$ themselves. Divide by their length:
$[6, 8] / \sqrt{36 + 64} = [6, 8] / 10 = [0.6, 0.8]$. A larger partial derivative earns a larger
share of the step, in exact proportion. The gradient encodes this balance on its own.
:::

::: card
What you computed is the **directional derivative**, the slope in the direction of a unit vector
$\mathbf{u}$:

$$ D_\mathbf{u} f = \nabla f \cdot \mathbf{u} = \frac{\partial f}{\partial x} u_1 + \frac{\partial f}{\partial y} u_2 $$

Weight each slope by how far you move along it, then add.
[Directional derivative](reference:directional-derivative)
:::

::: card
Turn the unit step through every angle $\theta$, with $\mathbf{u} = [\cos\theta, \sin\theta]$.
The slope traces a wave. East ($\theta = 0$) gives 6 and north ($\theta = \frac{\pi}{2}$) gives
8. The peak is $\|\nabla f\| = 10$, at $\theta \approx 0.93$ rad, the direction of the gradient.
Change the gradient and the peak moves with it.

```plot
x: { var: t, label: "the angle $θ$ of the step, in radians", from: 0, to: 6.283, ticks: 0.785, grid: true }
y: { label: "the slope $Dᵤ f$", from: -15, to: 15 }

inputs:
  - { name: gx, min: 0.5, max: 10, default: 6, step: 0.5, label: "∂f / ∂x" }
  - { name: gy, min: -10, max: 10, default: 8, step: 0.5, label: "∂f / ∂y" }

let:
  m: sqrt(gx^2 + gy^2)
  top: atan(gy / gx) + if(gy < 0, 2 * pi(), 0)

draw:
  - hline: { at: 0 }
  - curve: { is: gx * cos(t) + gy * sin(t), accent: true }
  - point: { at: [0, gx], label: east }
  - point: { at: [pi() / 2, gy], label: north }
  - point: { at: [top, m], label: "steepest, $‖∇ f‖$" }
```
:::

::: card
Because $D_\mathbf{u} f$ is a dot product, it equals
$\|\nabla f\| \, \|\mathbf{u}\| \cos\theta = \|\nabla f\| \cos\theta$, where $\theta$ is the
angle between $\nabla f$ and $\mathbf{u}$. It is largest at $\theta = 0$, when $\mathbf{u}$ points
along the gradient, and the largest slope is $\|\nabla f\| = \sqrt{6^2 + 8^2} = 10$.
[Dot product](reference:dot-product)
:::

::: card
To go downhill, walk the other way. At $(3, 4)$, $-\nabla f = [-6, -8]^T$ points at the origin,
the bottom of the bowl. A loss $L(w_1, \ldots, w_n)$ is a landscape in $n$ dimensions you cannot
picture, but the rule is the same. Step against the gradient:

$$ \mathbf{w}_{\text{new}} = \mathbf{w}_{\text{old}} - \eta \nabla L $$

The learning rate $\eta$ sets the step size. This is gradient descent.
[Gradient descent](reference:gradient-descent)
:::

::: exercise gradient-at-point
For $f(x, y) = 3x^2 + y$, what is $\nabla f$ at $(1, 2)$?

::: answer
$[6, 1]^T$. The partials are $6x$ and $1$.
:::
:::

::: exercise gradient-steepest-slope
At a point where $\nabla f = [3, 4]^T$, what is the steepest slope, and in which unit direction?

::: answer
Slope $5$, in the direction $[0.6, 0.8]$. Divide the gradient by its length.
:::

::: solution
$$ \|\nabla f\| = \sqrt{9 + 16} = 5 $$

$$ \mathbf{u} = \frac{[3, 4]}{5} = [0.6, 0.8] $$

∎
:::
:::

::: exercise directional-slope
At a point where $\nabla f = [2, -1]^T$, what is the slope in the unit direction
$\mathbf{u} = [0.6, 0.8]$?

::: answer
$0.4$. Take the dot product $\nabla f \cdot \mathbf{u}$.
:::

::: solution
$$ D_\mathbf{u} f = 2 \times 0.6 + (-1) \times 0.8 = 1.2 - 0.8 = 0.4 $$

∎
:::
:::

::: exercise descent-step
The weights are $\mathbf{w} = [1, 2]^T$, the gradient is $\nabla L = [4, -2]^T$, and $\eta = 0.1$.
What are the weights after one step of gradient descent?

::: answer
$[0.6, 2.2]^T$. Subtract $\eta \nabla L = [0.4, -0.2]^T$.
:::

::: solution
$$ \mathbf{w}_{\text{new}} = \begin{bmatrix} 1 \\ 2 \end{bmatrix} - 0.1 \begin{bmatrix} 4 \\ -2 \end{bmatrix} = \begin{bmatrix} 1 - 0.4 \\ 2 + 0.2 \end{bmatrix} = \begin{bmatrix} 0.6 \\ 2.2 \end{bmatrix} $$

∎
:::
:::

::: reference gradient
# Gradient

The gradient of a scalar function of $n$ variables is the column vector of its partial
derivatives. It points in the direction of steepest ascent, and its length is the steepest slope.

::: equation
\nabla f = \begin{bmatrix} \frac{\partial f}{\partial x_1} \\ \vdots \\ \frac{\partial f}{\partial x_n} \end{bmatrix}
:::

::: legend
$f$: a scalar function of n variables
$x_i$: the i-th input variable
$\nabla f$: the gradient, a vector with n entries
:::

::: derivation
Each entry is a partial derivative.[Partial derivative](reference:partial-derivative)
The slope in a unit direction is $\nabla f \cdot \mathbf{u}$.[Directional derivative](reference:directional-derivative)
That dot product is $\|\nabla f\| \cos\theta$, largest at $\theta = 0$.[Dot product](reference:dot-product)
So $\nabla f$ points the way of steepest ascent, and $\|\nabla f\|$ is the steepest slope. ∎
:::
:::

::: reference directional-derivative
# Directional derivative

The slope of $f$ in the direction of a unit vector $\mathbf{u}$ is the dot product of the gradient
with $\mathbf{u}$.

::: equation
D_\mathbf{u} f = \nabla f \cdot \mathbf{u} = \sum_{i=1}^{n} \frac{\partial f}{\partial x_i} u_i
:::

::: legend
$\mathbf{u}$: a unit vector, the direction of the step
$u_i$: the i-th entry of the direction
$\nabla f$: the gradient of f at the point
:::

::: derivation
Step a small distance $t$ along $\mathbf{u}$: each $x_i$ moves by $t u_i$.
Each move changes $f$ by about $\frac{\partial f}{\partial x_i} t u_i$.[Partial derivative](reference:partial-derivative)
To first order the changes add: $\Delta f \approx t \sum_i \frac{\partial f}{\partial x_i} u_i$.[Multivariable chain rule](reference:multivariable-chain-rule)
Divide by $t$ and let $t \to 0$: $D_\mathbf{u} f = \sum_i \frac{\partial f}{\partial x_i} u_i = \nabla f \cdot \mathbf{u}$. ∎
:::
:::

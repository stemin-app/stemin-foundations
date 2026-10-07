---
title: Why attention needs a bend
---

::: card
Self-attention outputs weighted sums of value vectors, $\sum_j a_{ij}\mathbf{v}_j$. The softmax
that makes the weights is nonlinear, but the mixing itself is linear in the values. Stack only
linear steps and you gain nothing: two linear maps in a row are one linear map,

$$ \mathbf{W}_2(\mathbf{W}_1\mathbf{x}) = (\mathbf{W}_2\mathbf{W}_1)\mathbf{x} $$

([linear layers collapse](reference:linear-layers-collapse)). A hundred linear layers have the
power of one.
:::

::: card
A linear function cannot express "this and that", "this or that", or "above a threshold". Take a
hallway light with a switch at each end. It is on when exactly one switch is up. The four cases
are $(0, 0)$ off, $(0, 1)$ on, $(1, 0)$ on and $(1, 1)$ off: the **XOR** pattern.
:::

::: card
A linear rule draws one straight line and puts "on" on one side. Turn and shift the line below.
The blue points (on) sit on one diagonal and the black points (off) on the other. No position of
the line puts both blue points on one side and both black points on the other.

```plot
x: { var: t, label: "switch A", from: -0.5, to: 1.5, ticks: 0.5, grid: true }
y: { label: "switch B", from: -0.5, to: 1.5 }

inputs:
  - { name: deg, min: 0, max: 180, default: 135, step: 5, label: "angle of the line, in degrees" }
  - { name: c, min: -1, max: 2, default: 0.5, step: 0.05, label: "where the line crosses x = y" }

let:
  th: deg * pi() / 180

draw:
  - param: { var: s, over: [-3, 3], x: c + s * cos(th), y: c + s * sin(th), dash: true }
  - points: { at: [[0, 1], [1, 0]], accent: true, label: "on" }
  - points: { at: [[0, 0], [1, 1]], label: "off" }
```
:::

::: card
Why no line works: a line $w_1 a + w_2 b + c = 0$ would need $c < 0$ for $(0, 0)$, $w_1 + c > 0$
and $w_2 + c > 0$ for the two "on" points, and $w_1 + w_2 + c < 0$ for $(1, 1)$. Add the two
middle conditions: $w_1 + w_2 + 2c > 0$, so $w_1 + w_2 + c > -c > 0$. That contradicts the last
one. A curved boundary, around the two "on" points, does the job. To draw curves, a network needs
a nonlinearity.
:::

::: exercise q1
$\mathbf{W}_1 = \begin{bmatrix} 1 & 1 \\ 0 & 2 \end{bmatrix}$ and $\mathbf{W}_2 = \begin{bmatrix} 2 & 0 \\ 1 & 1 \end{bmatrix}$
act one after the other with no bend. Which single matrix does the same?

::: answer
$\begin{bmatrix} 2 & 2 \\ 1 & 3 \end{bmatrix}$, the product $\mathbf{W}_2\mathbf{W}_1$.
:::
:::

::: exercise q2
AND is on only at $(1, 1)$. Can one straight line separate it? Give one if so.

::: answer
Yes. The line $a + b = 1.5$ puts $(1, 1)$ above and the other three points below.
:::
:::

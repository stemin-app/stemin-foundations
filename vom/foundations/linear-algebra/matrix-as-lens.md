---
title: A weight matrix and its shape
---

::: card
In a neural network, a product $\mathbf{y} = \mathbf{W}\mathbf{x}$ is an action on the vector
$\mathbf{x}$. What kind of action depends on the shape of $\mathbf{W}$: on how the number of
outputs compares to the number of inputs. There are three cases. Learn to read them at a
glance.
:::

::: card
**Fewer outputs than inputs: a compressor.** Take $\mathbf{x} \in \mathbb{R}^{512}$ and
$\mathbf{W} \in \mathbb{R}^{64 \times 512}$, with rows $\mathbf{r}_1, \ldots, \mathbf{r}_{64}$:

$$ \mathbf{W}\mathbf{x} = \begin{bmatrix} \mathbf{r}_1 \cdot \mathbf{x} \\ \vdots \\ \mathbf{r}_{64} \cdot \mathbf{x} \end{bmatrix} $$

Each output is a dot product, a test of how far $\mathbf{x}$ points along one row. Each row is
a detector for one feature.
:::

::: card
A part of $\mathbf{x}$ perpendicular to all 64 rows gives zero in every test. The matrix cannot
see it. The set of inputs sent to zero is the **null space**, and a $64 \times 512$ matrix has
one of at least 448 dimensions. The compressor must choose what to keep, like a ten second
description of a busy street over a bad phone line: "red car, speeding, police behind it."
:::

::: card
**More outputs than inputs: an expander.** $\mathbf{W} \in \mathbb{R}^{2048 \times 512}$ maps
$\mathbb{R}^{512}$ into $\mathbb{R}^{2048}$. It creates no information: the outputs fill only a
512 dimensional flat sheet inside the larger space, the span of the columns. What it creates is
room. It reads the input along 2048 directions at once, so a later step can pick apart
patterns that overlapped.
:::

::: card
A small case shows why room helps. Points on a line, labelled blue, black, blue, cannot be split
by one cut. Lift each $x$ to $[x, x^2]$ and they land on a parabola. Now one horizontal line
splits them. Move it until the blue points (outside) sit above and the black ones (inside)
below.

```plot
x: { var: x, label: "$x$", from: -3, to: 3, ticks: 1, grid: true }
y: { label: "$x²$", from: -0.5, to: 6 }

inputs:
  - { name: h, min: 0, max: 5, default: 2.5, step: 0.25, label: "height of the cut" }

draw:
  - curve: { is: x ^ 2, dash: true }
  - points: { at: [[-2.4, 0], [-2, 0], [2, 0], [2.3, 0]], accent: true }
  - points: [[-0.8, 0], [-0.3, 0], [0.4, 0], [0.9, 0]]
  - points: { at: [[-2.4, 5.76], [-2, 4], [2, 4], [2.3, 5.29]], accent: true }
  - points: [[-0.8, 0.64], [-0.3, 0.09], [0.4, 0.16], [0.9, 0.81]]
  - hline: { at: h, label: "cut" }
```
:::

::: card
**As many outputs as inputs: a mixer.** A full rank $\mathbf{W} \in \mathbb{R}^{512 \times 512}$
is a change of basis. The output carries exactly the information of the input, reorganized:
$\mathbf{y} = x_1\mathbf{w}_1 + \cdots + x_n\mathbf{w}_n$. Its null space holds only zero, so
nothing is lost. It is a map turned the right way up: all there, but now facing the direction
the next layer reads.
:::

::: card
A mixer combines channels. It does not read feature 1 and feature 2 alone; it reads their sum
and their difference. It routes information from where it was computed to where it is needed.
Inside a transformer, "the subject is John" and "the verb is run" come in as separate reports,
and a square matrix mixes them into one meaning.
:::

::: card
So read a weight matrix by its shape. Narrower output: it summarizes, and decides what matters.
Wider output: it analyzes, and spreads the input out to untangle it. Same size: it translates,
and reorganizes for the next step.
:::

::: exercise q1
$\mathbf{W} \in \mathbb{R}^{2048 \times 512}$. Which of the three kinds is it, and what is the
largest possible rank?

::: answer
An expander, with rank at most 512. The rank is never above the smaller size.
:::
:::

::: exercise q2
$\mathbf{W} \in \mathbb{R}^{64 \times 512}$ has rank 64. What is the dimension of its null space?

::: answer
448. The input has 512 dimensions, and 64 of them reach the output, so $512 - 64$ are sent to
zero.
:::
:::

::: exercise q3
The points $-1$, $0$ and $1$ on a line are labelled A, B, A. Lift $x$ to $[x, x^2]$. Give a
horizontal line that splits the two labels.

::: answer
$x^2 = 0.5$. The A points land at height 1, the B point at height 0.
:::
:::

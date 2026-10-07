---
title: Offsets are rotations
---

::: card
The most useful property of the sinusoidal encoding: moving $k$ positions forward acts the same way
at every position. For each pair, the step from $t$ to $t + k$ is a **rotation** by the angle
$\omega_i k$, and that angle does not depend on $t$.
:::

::: card
Expand the sine and the cosine of a sum:

$$ \begin{bmatrix} \sin\omega(t + k) \\ \cos\omega(t + k) \end{bmatrix} = \begin{bmatrix} \cos\omega k & \sin\omega k \\ -\sin\omega k & \cos\omega k \end{bmatrix} \begin{bmatrix} \sin\omega t \\ \cos\omega t \end{bmatrix} $$

The matrix holds only $k$. So "three tokens back" is one fixed linear map, the same at position 10
and at position 1,000 ([offset as a rotation](reference:encoding-offset-rotation)). A model can
learn it once.
:::

::: card
Dot two encodings together and the positions drop out. Each pair gives
$\sin\omega t \sin\omega(t + k) + \cos\omega t \cos\omega(t + k) = \cos\omega k$, so

$$ \mathbf{p}_t \cdot \mathbf{p}_{t+k} = \sum_{i=0}^{d/2-1} \cos(\omega_i k) $$

The similarity of two positions depends only on how far apart they are.
:::

::: card
Here is that sum for $d = 16$, against the offset $k$. At $k = 0$ it is $d/2 = 8$, the squared
length. It falls, with ripples, as the positions move apart: near positions look alike, far ones
less so. Move
$t$: the point slides along the same curve, because $t$ is not in the formula.

```plot
x: { var: k, label: "offset k", from: 0, to: 60, ticks: 5, grid: true }
y: { label: "pₜ · pₜ₊ₖ", from: -1, to: 8.5 }

inputs:
  - { name: k0, min: 0, max: 60, default: 5, step: 1, label: "offset of the marked pair" }
  - { name: t, min: 0, max: 500, default: 100, step: 10, label: "position t, which does not matter" }

let:
  f: cos(k) + cos(0.316228 * k) + cos(0.1 * k) + cos(0.0316228 * k) + cos(0.01 * k) + cos(0.00316228 * k) + cos(0.001 * k) + cos(0.000316228 * k)
  f0: cos(k0) + cos(0.316228 * k0) + cos(0.1 * k0) + cos(0.0316228 * k0) + cos(0.01 * k0) + cos(0.00316228 * k0) + cos(0.001 * k0) + cos(0.000316228 * k0)

draw:
  - curve: { is: f, accent: true }
  - point: { at: [k0, f0 + 0 * t], label: "same for every t" }
```
:::

::: exercise q1
For one pair with $\omega = 0.5$, what rotation angle takes position $t$ to position $t + 4$?

::: answer
2 radians. The angle is $\omega k = 0.5 \times 4$.
:::
:::

::: exercise q2
With $d = 4$, $\omega_0 = 1$ and $\omega_1 = 0.01$, compute $\mathbf{p}_t \cdot \mathbf{p}_{t+3}$.

::: answer
About $0.01$. $\cos 3 + \cos 0.03 \approx -0.990 + 1.000$.
:::

::: solution
$\cos(1 \times 3) = \cos 3 \approx -0.990$.

$\cos(0.01 \times 3) = \cos 0.03 \approx 0.9996$.

Sum: $\approx 0.0096$. ∎
:::
:::

::: reference encoding-offset-rotation
# An offset is a rotation

For each pair of a sinusoidal encoding, moving from position $t$ to $t + k$ rotates the pair by
the angle $\omega_i k$. The rotation does not depend on $t$, and neither does the dot product of
the two encodings.

::: equation
\begin{bmatrix} p_{t+k,2i} \\ p_{t+k,2i+1} \end{bmatrix} = \begin{bmatrix} \cos\omega_i k & \sin\omega_i k \\ -\sin\omega_i k & \cos\omega_i k \end{bmatrix} \begin{bmatrix} p_{t,2i} \\ p_{t,2i+1} \end{bmatrix} \qquad \mathbf{p}_t \cdot \mathbf{p}_{t+k} = \sum_{i} \cos\omega_i k
:::

::: legend
$p_{t,j}$: dimension $j$ of the encoding of position $t$
$k$: the offset between the two positions
$\omega_i$: the frequency of pair $i$
:::

::: derivation
$\sin(a + b) = \sin a \cos b + \cos a \sin b$ and $\cos(a + b) = \cos a \cos b - \sin a \sin b$.
With $a = \omega_i t$ and $b = \omega_i k$, the two lines are the rows of the matrix product.[Sinusoidal encoding](reference:sinusoidal-encoding)
The matrix is orthogonal, a rotation.[Matrix-vector product](reference:matrix-vector-product)
For the dot product, each pair gives $\sin a \sin(a + b) + \cos a \cos(a + b) = \cos(a + b - a) = \cos b$. ∎
:::
:::

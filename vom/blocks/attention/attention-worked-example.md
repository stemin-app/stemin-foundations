---
title: Attention by hand
---

::: card
Compute one attention step with $d = 3$. The query, the three keys and the three values are

$$ \mathbf{q} = \begin{bmatrix} 1 \\ 0 \\ 1 \end{bmatrix}; \quad \mathbf{k}_1 = \begin{bmatrix} 1 \\ 1 \\ 0 \end{bmatrix}, \; \mathbf{k}_2 = \begin{bmatrix} 0 \\ 1 \\ 1 \end{bmatrix}, \; \mathbf{k}_3 = \begin{bmatrix} 1 \\ 0 \\ 1 \end{bmatrix}; \quad \mathbf{v}_1 = \begin{bmatrix} 1 \\ 2 \end{bmatrix}, \; \mathbf{v}_2 = \begin{bmatrix} 3 \\ 4 \end{bmatrix}, \; \mathbf{v}_3 = \begin{bmatrix} 5 \\ 6 \end{bmatrix} $$
:::

::: card
**Step 1, the scores.** Dot the query with each key and divide by $\sqrt{3} \approx 1.732$:

$$ s_1 = \frac{1}{\sqrt{3}} \approx 0.577, \qquad s_2 = \frac{1}{\sqrt{3}} \approx 0.577, \qquad s_3 = \frac{2}{\sqrt{3}} \approx 1.155 $$

The third key equals the query, so it scores highest.
:::

::: card
**Step 2, the weights.** Exponentiate: $e^{0.577} \approx 1.781$ and $e^{1.155} \approx 3.174$.
The sum is $1.781 + 1.781 + 3.174 = 6.736$, so

$$ \alpha_1 = \alpha_2 = \frac{1.781}{6.736} \approx 0.264, \qquad \alpha_3 = \frac{3.174}{6.736} \approx 0.471 $$
:::

::: card
**Step 3, the output.** Mix the values:

$$ \mathbf{o} = 0.264\begin{bmatrix} 1 \\ 2 \end{bmatrix} + 0.264\begin{bmatrix} 3 \\ 4 \end{bmatrix} + 0.471\begin{bmatrix} 5 \\ 6 \end{bmatrix} = \begin{bmatrix} 0.264 + 0.792 + 2.355 \\ 0.528 + 1.056 + 2.826 \end{bmatrix} = \begin{bmatrix} 3.41 \\ 4.41 \end{bmatrix} $$

It leans toward $\mathbf{v}_3 = [5, 6]$, because position 3 matched best.
:::

::: card
The output becomes the new representation of the position that asked. Its old vector knew only
itself. The new one has gathered content from every position, weighed by relevance: the token now
"knows about" the others.
:::

::: card
Now turn the query in a plane. Three keys point at $0°$, $90°$ and $180°$, all of length 2, and
the query has length 2 too. The bars are the three weights, with $d = 2$. The weights follow the
key the query points at, and they never all vanish.

```plot
x: { var: t, label: "key", from: 0.4, to: 3.6, ticks: 1 }
y: { label: "weight α", from: 0, to: 1, ticks: 0.25, grid: true }

inputs:
  - { name: deg, min: 0, max: 180, default: 30, step: 5, label: "direction of the query, in degrees" }

let:
  th: deg * pi() / 180
  e1: exp(4 * cos(th) / sqrt(2))
  e2: exp(4 * sin(th) / sqrt(2))
  e3: exp(-4 * cos(th) / sqrt(2))
  z: e1 + e2 + e3

draw:
  - area: { under: e1 / z, over: [0.7, 1.3], accent: true }
  - area: { under: e2 / z, over: [1.7, 2.3] }
  - area: { under: e3 / z, over: [2.7, 3.3] }
  - point: { at: [1, 0.95], label: "0°" }
  - point: { at: [2, 0.95], label: "90°" }
  - point: { at: [3, 0.95], label: "180°" }
```
:::

::: exercise q1
In the example, change the query to $\mathbf{q} = [0, 1, 1]^T$. What are the three unscaled dot
products?

::: answer
1, 2 and 1. Now the second key matches the query exactly.
:::
:::

::: exercise q2
Scores $[0, 0, \ln 2]$ meet the values $[1, 2]$, $[3, 4]$, $[5, 6]$. What is the output?

::: answer
$[3.5, 4.5]$. The weights are $[0.25, 0.25, 0.5]$.
:::

::: solution
$e^0 = 1$, $e^0 = 1$, $e^{\ln 2} = 2$; the sum is 4.

Weights: $[0.25, 0.25, 0.5]$.

$\mathbf{o} = 0.25[1, 2] + 0.25[3, 4] + 0.5[5, 6] = [0.25 + 0.75 + 2.5, \; 0.5 + 1 + 3] = [3.5, 4.5]$. ∎
:::
:::

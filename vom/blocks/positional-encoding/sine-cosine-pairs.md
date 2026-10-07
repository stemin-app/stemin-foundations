---
title: Why sine and cosine come in pairs
---

::: card
Why not use sines alone? Because a sine cannot be read back. If $\sin\theta = 0.5$, the angle
$\theta$ could be $30°$ or $150°$. Two different positions can land on the same sine value, and a
model reading that number alone could not tell them apart.
:::

::: card
Add the cosine and the doubt goes. $\sin\theta = 0.5$ with $\cos\theta = 0.866$ means
$\theta = 30°$, and nothing else between $0°$ and $360°$. At $150°$ the cosine is $-0.866$. The two
functions are $90°$ out of step, so each fills in what the other leaves open.
:::

::: card
Seen as a point, the pair $(\sin\omega t, \cos\omega t)$ sits on the unit circle. As the position
grows, the point travels round the circle at speed $\omega$. The dashed line marks every point
with sine 0.5: it crosses the circle twice, and only the cosine says which crossing you are at.

```plot
x: { var: u, label: "$sin ω t$", from: -1.3, to: 1.3, ticks: 0.5, grid: true }
y: { label: "$cos ω t$", from: -1.3, to: 1.3 }

inputs:
  - { name: t, min: 0, max: 30, default: 3, step: 1, label: "position t" }
  - { name: w, min: 0.1, max: 1, default: 0.5, step: 0.05, label: "frequency ω" }

draw:
  - param: { var: s, over: [0, 2 * pi()], x: sin(s), y: cos(s) }
  - vline: { at: 0.5, dash: true, label: "sine 0.5" }
  - param: { var: s, over: [0, 1], x: s * sin(w * t), y: s * cos(w * t), accent: true }
  - point: { at: [sin(w * t), cos(w * t)], label: "$t$" }
```
:::

::: card
Each pair is one such circle, turning at its own speed. A $d$-wide encoding is $d/2$ circles, fast
ones and slow ones, and the positions of all the points together form the fingerprint of a
position.
:::

::: card
Every pair adds $\sin^2 + \cos^2 = 1$ to the squared length, so every encoding has the same
length, whatever the position:

$$ \|\mathbf{p}_t\|^2 = \sum_{i=0}^{d/2-1} \left(\sin^2 \omega_i t + \cos^2 \omega_i t\right) = \frac{d}{2} $$
:::

::: exercise q1
An angle has $\sin\theta = -0.6$ and $\cos\theta = 0.8$. What is $\theta$ between $0°$ and
$360°$?

::: answer
About $323.1°$. A negative sine and a positive cosine put the angle in the fourth quarter.
:::

::: solution
$$ \sin^{-1}(0.6) = 36.87° $$

Sine negative and cosine positive: the fourth quarter.

$$ \theta = 360° - 36.87° = 323.13° $$

∎
:::
:::

::: exercise q2
What is the length $\|\mathbf{p}_t\|$ of a sinusoidal encoding with $d = 512$?

::: answer
16. The squared length is $d/2 = 256$.
:::

::: solution
$$ \|\mathbf{p}_t\|^2 = \frac{512}{2} = 256 $$

$$ \|\mathbf{p}_t\| = \sqrt{256} = 16 $$

∎
:::
:::

::: exercise q3
A sine-only pair at $\omega = 1$ gives $\sin 1 = 0.841$ at $t = 1$. At what other angle between 0
and $\pi$ does the sine give 0.841 too?

::: answer
$\pi - 1 \approx 2.14$. The cosines differ: $\cos 1 = 0.540$ and $\cos 2.14 = -0.540$.
:::

::: solution
$$ \sin(\pi - \theta) = \sin\theta $$

$$ \pi - 1 = 2.142 $$

∎
:::
:::

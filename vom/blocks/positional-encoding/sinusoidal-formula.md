---
title: The sinusoidal encoding
---

::: card
Start with the numbers. With $d = 8$ dimensions, the first four positions get these vectors, to
two decimal places:

$$ \begin{aligned} \mathbf{p}_0 &= (0.00,\ 1.00,\ 0.00,\ 1.00,\ 0.00,\ 1.00,\ 0.00,\ 1.00) \\ \mathbf{p}_1 &= (0.84,\ 0.54,\ 0.10,\ 1.00,\ 0.01,\ 1.00,\ 0.00,\ 1.00) \\ \mathbf{p}_2 &= (0.91,\ {-0.42},\ 0.20,\ 0.98,\ 0.02,\ 1.00,\ 0.00,\ 1.00) \\ \mathbf{p}_3 &= (0.14,\ {-0.99},\ 0.30,\ 0.96,\ 0.03,\ 1.00,\ 0.00,\ 1.00) \end{aligned} $$
:::

::: card
Read down the columns. The first two numbers change a lot from one position to the next. The
middle ones change slowly. The last two barely move. Each pair of dimensions tracks position at
its own speed.
:::

::: card
The rule behind the numbers: dimension $2i$ holds a sine and dimension $2i + 1$ a cosine, both at
the frequency $\omega_i$ of pair $i$.[Sinusoidal encoding](reference:sinusoidal-encoding)

$$ p_{t,2i} = \sin(\omega_i t), \qquad p_{t,2i+1} = \cos(\omega_i t), \qquad \omega_i = \frac{1}{10000^{2i/d}} $$
:::

::: card
For $d = 8$ there are $d/2 = 4$ pairs, and each frequency is a tenth of the one before:

$$ \omega_0 = 1, \quad \omega_1 = 0.1, \quad \omega_2 = 0.01, \quad \omega_3 = 0.001 $$

Dimensions 0 and 1 use $\omega_0$, dimensions 2 and 3 use $\omega_1$, and so on.
:::

::: card
Here is one pair in a model with $d = 64$, plotted against the position: the sine in solid line,
the cosine dashed. Raise the pair index $i$ and the frequency falls, so the waves stretch out.

```plot
x: { var: t, label: "position $t$", from: 0, to: 100, ticks: 10, grid: true }
y: { label: "$pₜ,ⱼ$", from: -1.1, to: 1.1 }

inputs:
  - { name: i, min: 0, max: 31, default: 2, step: 1, label: "pair index i" }

let:
  w: 10 ^ (-i / 8)

draw:
  - hline: { at: 0 }
  - curve: { is: sin(w * t), accent: true, label: "$sin(ωᵢ t)$" }
  - curve: { is: cos(w * t), dash: true, label: "$cos(ωᵢ t)$" }
```
:::

::: card
Every value lies between $-1$ and $1$, whatever $t$ is. The rule is a formula, not a table, so
position 10,000 gets a vector as easily as position 3, even if training never reached it.
:::

::: exercise q1
With $d = 8$, compute the positional encoding $\mathbf{p}_5$ to three decimal places.

::: answer
$(-0.959,\ 0.284,\ 0.479,\ 0.878,\ 0.050,\ 0.999,\ 0.005,\ 1.000)$. Use $\omega_i = 1, 0.1, 0.01,
0.001$ and the sine, cosine rule.
:::

::: solution
The angles are $\omega_i t$ for $t = 5$:

$$ 5, \quad 0.5, \quad 0.05, \quad 0.005 $$

Pair 0: $\sin 5 = -0.959$, $\cos 5 = 0.284$.

Pair 1: $\sin 0.5 = 0.479$, $\cos 0.5 = 0.878$.

Pair 2: $\sin 0.05 = 0.050$, $\cos 0.05 = 0.999$.

Pair 3: $\sin 0.005 = 0.005$, $\cos 0.005 = 1.000$.

$$ \mathbf{p}_5 = (-0.959,\ 0.284,\ 0.479,\ 0.878,\ 0.050,\ 0.999,\ 0.005,\ 1.000) $$

∎
:::
:::

::: exercise q2
A model has $d = 16$. What is the frequency $\omega_2$ of pair 2?

::: answer
0.1. The exponent is $2i/d = 4/16 = 0.25$.
:::

::: solution
$$ \omega_2 = \frac{1}{10000^{4/16}} = \frac{1}{10000^{0.25}} = \frac{1}{10} = 0.1 $$

∎
:::
:::

::: exercise q3
With $d = 8$, what is dimension 5 of $\mathbf{p}_{100}$?

::: answer
0.540. Dimension 5 is the cosine of pair 2, with $\omega_2 = 0.01$.
:::

::: solution
$$ p_{100,5} = \cos(0.01 \times 100) = \cos 1 = 0.540 $$

∎
:::
:::

::: exercise q4
With $d = 8$, which dimension holds the sine at frequency $\omega_3$?

::: answer
Dimension 6. Pair $i$ puts its sine in dimension $2i$.
:::
:::

::: reference sinusoidal-encoding
# Sinusoidal positional encoding

Position $t$ gets a $d$-wide vector. Pair $i$ of its dimensions holds a sine and a cosine of $t$
at the frequency $\omega_i$, and the frequencies fall from 1 to near $1/10000$ across the pairs.

::: equation
p_{t,2i} = \sin(\omega_i t), \qquad p_{t,2i+1} = \cos(\omega_i t), \qquad \omega_i = 10000^{-2i/d}, \quad i = 0, 1, \dots, \tfrac{d}{2} - 1
:::

::: legend
$p_{t,j}$: dimension $j$ of the encoding of position $t$
$t$: the position, counted from 0
$d$: the width of the encoding, the same as the embedding
$i$: the index of the pair of dimensions
$\omega_i$: the frequency of pair $i$, in radians per position
:::
:::

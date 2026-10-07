---
title: Fast and slow dimensions
---

::: card
A pair with frequency $\omega_i$ repeats every $2\pi/\omega_i$ positions: its **wavelength**. With
$d = 8$ the four pairs have very different wavelengths.

$$ \lambda_0 = 2\pi \approx 6.3, \quad \lambda_1 = \frac{2\pi}{0.1} \approx 63, \quad \lambda_2 = \frac{2\pi}{0.01} \approx 628, \quad \lambda_3 = \frac{2\pi}{0.001} \approx 6283 $$
:::

::: card
Pair 0, with $\omega_0 = 1$, completes a cycle about every 6 positions. It tells position 5 from
position 6, but it comes back to nearly the same values every 6 positions. It carries fine,
local position.
:::

::: card
Pair 1 repeats every 63 positions: 5 and 6 look almost the same, but 5 and 50 differ. Pair 2
repeats every 628: 5 and 50 look almost the same, but 5 and 500 differ. Pair 3 repeats every
6283 and stays near constant over a normal sequence.
:::

::: card
Read one encoding across its dimensions. This plot shows all 16 values of $\mathbf{p}_t$ for
$d = 16$, one flat step per dimension. Slide the position: the steps on the left jump around,
while the steps on the right barely move.

```plot
x: { var: j, label: "dimension $j$", from: 0, to: 16, ticks: 1, grid: true }
y: { label: "$pₜ,ⱼ$", from: -1.1, to: 1.1 }

inputs:
  - { name: t, min: 0, max: 63, default: 5, step: 1, label: "position t" }

let:
  k: floor(j)
  w: 10 ^ (-floor(k / 2) / 2)

draw:
  - hline: { at: 0 }
  - curve: { is: "if(k % 2 < 0.5, sin(w * t), cos(w * t))", accent: true, over: [0, 15.999] }
```
:::

::: card
Stack those rows for 64 positions and you get the whole encoding at a glance ([Figure](figure:encoding-heatmap)).
The top rows, the fast pairs, form tight stripes. The bottom rows, the slow pairs, change so
slowly that they look flat. Each column, one position, has its own pattern.
:::

::: figure encoding-heatmap
![The sinusoidal encoding for 64 positions and 16 dimensions](assets/encoding-heatmap.svg)

Each cell is $p_{t,j}$ for $d = 16$: darker means nearer $+1$, lighter nearer $-1$. Columns are
positions 0 to 63; rows are dimensions 0 to 15.
:::

::: card
Compare positions 5 and 6 with $d = 8$, coordinate by coordinate. The size of each change, to two
decimal places:

$$ |\mathbf{p}_6 - \mathbf{p}_5| = (0.68,\ 0.68,\ 0.09,\ 0.05,\ 0.01,\ 0.00,\ 0.00,\ 0.00) $$

The fast pair moves a lot, the slow pairs hardly at all.
:::

::: card
So the fast dimensions tell neighbours apart, and the slow dimensions tell regions of the
sequence apart. Together they give each position a fingerprint of its own.
:::

::: exercise q1
With $d = 16$, what is the wavelength of pair 4?

::: answer
About 628 positions. $\omega_4 = 10000^{-8/16} = 0.01$.
:::

::: solution
$$ \omega_4 = 10000^{-8/16} = 10000^{-0.5} = 0.01 $$

$$ \lambda_4 = \frac{2\pi}{0.01} = 628.3 $$

∎
:::
:::

::: exercise q2
With $d = 8$, pair 0 encodes position 63 as $(\sin 63, \cos 63)$. Compute it to two decimal
places, and compare it with position 0.

::: answer
$(0.17, 0.99)$, close to $(0, 1)$ at position 0. 63 positions is just over 10 full cycles.
:::

::: solution
$$ \frac{63}{2\pi} = 10.03 \text{ cycles} $$

$$ 63 - 20\pi = 0.168 $$

$$ (\sin 63, \cos 63) = (\sin 0.168, \cos 0.168) = (0.17, 0.99) $$

∎
:::
:::

::: exercise q3
With $d = 8$, which is the fastest pair that clearly tells position 63 from position 0?

::: answer
Pair 2, dimensions 4 and 5: $(0.59, 0.81)$ against $(0, 1)$. Pairs 0 and 1 have both come back near $(0, 1)$.
:::

::: solution
Pair 0: $(\sin 63, \cos 63) = (0.17, 0.99)$.

Pair 1: $(\sin 6.3, \cos 6.3) = (0.02, 1.00)$, since $6.3$ is just over $2\pi$.

Pair 2: $(\sin 0.63, \cos 0.63) = (0.59, 0.81)$.

Pair 2 is the first one far from $(0, 1)$. ∎
:::
:::

::: reference encoding-wavelength
# Wavelength of an encoding pair

Pair $i$ of a sinusoidal encoding repeats every $\lambda_i$ positions. The wavelengths grow
geometrically, from $2\pi$ for pair 0 toward $2\pi \cdot 10000$ for the last pair.

::: equation
\lambda_i = \frac{2\pi}{\omega_i} = 2\pi \cdot 10000^{2i/d}
:::

::: legend
$\lambda_i$: the wavelength of pair $i$, in positions
$\omega_i$: the frequency of pair $i$, in radians per position
$i$: the index of the pair
$d$: the width of the encoding
:::

::: derivation
Pair $i$ holds $\sin(\omega_i t)$ and $\cos(\omega_i t)$.[Sinusoidal encoding](reference:sinusoidal-encoding)
Sine and cosine repeat when their angle grows by $2\pi$.
$\omega_i (t + \lambda_i) = \omega_i t + 2\pi$ gives $\lambda_i = 2\pi/\omega_i$.
$\omega_i = 10000^{-2i/d}$ gives $\lambda_i = 2\pi \cdot 10000^{2i/d}$. ∎
:::
:::

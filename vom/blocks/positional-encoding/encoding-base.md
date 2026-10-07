---
title: Why the base is 10000
---

::: card
The constant in $\omega_i = 10000^{-2i/d}$ sets the range of wavelengths. With the base 10000 and
$d = 512$, the fastest pair repeats every $2\pi \approx 6$ positions, and the slowest every

$$ \lambda_{255} = 2\pi \cdot 10000^{510/512} \approx 2\pi \cdot 9{,}647 \approx 60{,}600 \text{ positions} $$

([wavelength of a pair](reference:encoding-wavelength)).
:::

::: card
The wavelengths should span the sequences the model reads, 512 to 2,048 tokens in the original
models. The fast pairs tell neighbours apart. The slowest pairs must change so little over a
whole sequence that they act as a steady regional signal.
:::

::: card
Each dot below is the wavelength of one pair, for $d = 512$, on a log scale. The dashed line is a
sequence of 2,048 tokens. Lower the base to 100 and the slowest pair has a wavelength of only
about 620: it repeats more than three times in one sequence. Raise it far above 10000 and many
slow pairs barely move at all, which wastes their dimensions.

```plot
x: { var: i, label: "pair index i", from: 0, to: 256, ticks: 32, grid: true }
y: { label: "wavelength, in positions", from: 1, to: 10000000, scale: log }

inputs:
  - { name: lb, min: 2, max: 6, default: 4, step: 0.5, label: "base, as a power of 10" }

draw:
  - hline: { at: 2048, dash: true, label: "2048 tokens" }
  - curve: { is: 2 * pi() * 10 ^ (lb * 2 * i / 512), over: [0, 255], accent: true }
```
:::

::: card
So 10000 is a practical choice for sequences up to several thousand tokens. Models that read much
longer sequences raise the base, so that the slowest pairs still change slowly across their whole
context.
:::

::: exercise q1
With base 100 and $d = 512$, what is the wavelength of the slowest pair, $i = 255$?

::: answer
About 618 positions. $2\pi \cdot 100^{510/512} \approx 2\pi \cdot 98.2$.
:::
:::

::: exercise q2
A model reads sequences of 2,048 tokens with base 100. About how many times does its slowest pair
repeat in one sequence?

::: answer
About 3.3 times. $2{,}048 / 618 \approx 3.3$.
:::
:::

---
title: One law for size and data
---

::: card
Each single law assumes the other resource is plentiful. A real run has a finite model and a
finite dataset. Kaplan's combined law covers both at once:

$$ L(N, D) = \left[\left(\frac{N_c}{N}\right)^{\alpha_N / \alpha_D} + \frac{D_c}{D}\right]^{\alpha_D} $$

The constants are the same ones as in the single laws.
[Unified scaling law](reference:unified-scaling-law)
:::

::: card
Let one resource grow without bound and the other law comes back. As $N \to \infty$ the first
term vanishes, and as $D \to \infty$ the second one does:

$$ L(\infty, D) = \left(\frac{D_c}{D}\right)^{\alpha_D} \qquad L(N, \infty) = \left(\frac{N_c}{N}\right)^{\alpha_N} $$

In the second case the outer power $\alpha_D$ cancels the inner $1/\alpha_D$. Between the two
limits the formula moves smoothly from one regime to the other.
:::

::: card
Read the bracket as two deficits added in one currency. The second term $D_c/D$ is a shortage of
data. The first, $(N_c/N)^{\alpha_N/\alpha_D}$, is a shortage of parameters converted into the
same units, with the conversion power $\alpha_N / \alpha_D = 0.076 / 0.095 = 0.8$. Too few
parameters acts like too little data.
:::

::: card
Fix the dataset and grow the model. For small $N$ the loss follows the parameter law, the
dashed line. Then it bends and settles on a floor set by the data, $(D_c/D)^{\alpha_D}$. Drag the
dataset size: more tokens lower the floor and push the bend to larger models.

```plot
x: { var: N, label: "$N$", from: 1e6, to: 1e13, scale: log, ticks: 1, grid: true }
y: { label: "$L$ in nats", from: 1, to: 5, scale: log, ticks: 1, grid: true }

inputs:
  - { name: ld, min: 8, max: 13, default: 10, step: 0.5, label: "the data log₁₀ D" }

let:
  Dv: 10^ld
  dfloor: (5.4e13 / Dv)^0.095

draw:
  - curve: { is: (8.8e13 / N)^0.076, dash: true }
  - hline: { at: dfloor, dash: true, label: "data floor" }
  - curve: { is: ((8.8e13 / N)^0.8 + 5.4e13 / Dv)^0.095, accent: true, label: "$L(N, D)$" }
```
:::

::: card
This is the diminishing return of the joint law. Scale one resource alone and each step helps
less, because the loss can never fall below what the other resource allows. With $10^{10}$
tokens the floor is $(5400)^{0.095} \approx 2.26$ nats, and no model size gets under it. To keep
gaining, grow both.
:::

::: exercise unified-data-floor
A model trains on $5.4 \times 10^{11}$ tokens. Under the unified law, what is the lowest loss any
model size can reach?

::: answer
About 1.55 nats. Let $N \to \infty$: the floor is $(D_c/D)^{\alpha_D} = 100^{0.095}$.
:::

::: solution
$$ \frac{D_c}{D} = \frac{5.4 \times 10^{13}}{5.4 \times 10^{11}} = 100 $$

$$ L(\infty, D) = 100^{0.095} = 10^{0.19} = 1.55\ \text{nats} $$

∎
:::
:::

::: exercise unified-both-finite
A model has $N = 8.8 \times 10^{11}$ parameters and trains on $D = 5.4 \times 10^{11}$ tokens.
What loss does the unified law give?

::: answer
About 1.60 nats. Both ratios are 100, so the bracket is $100^{0.8} + 100$.
:::

::: solution
$$ \left(\frac{N_c}{N}\right)^{0.8} = 100^{0.8} = 10^{1.6} = 39.8 $$

$$ \frac{D_c}{D} = 100 $$

$$ L = (39.8 + 100)^{0.095} = 139.8^{0.095} = e^{0.095 \times 4.940} = e^{0.469} = 1.60\ \text{nats} $$

This is above both single laws, 1.42 nats and 1.55 nats. ∎
:::
:::

::: exercise unified-conversion
What is the conversion power $\alpha_N / \alpha_D$ in the unified law, with Kaplan's exponents?

::: answer
0.8. Divide 0.076 by 0.095.
:::
:::

::: reference unified-scaling-law
# Unified scaling law

When both the model and the dataset are finite, the test loss combines the two deficits inside
one power. It reduces to the parameter law and the data law at the two limits.

::: equation
L(N, D) = \left[\left(\frac{N_c}{N}\right)^{\alpha_N / \alpha_D} + \frac{D_c}{D}\right]^{\alpha_D}
:::

::: legend
$L$: the test loss, in nats per token
$N$: the number of parameters
$D$: the number of training tokens
$N_c$: the fitted constant $8.8 \times 10^{13}$
$D_c$: the fitted constant $5.4 \times 10^{13}$
$\alpha_N$: the parameter exponent 0.076
$\alpha_D$: the data exponent 0.095
:::

::: derivation
The form is a fit. Its two limits are the single laws.[Kaplan scaling laws](reference:kaplan-scaling-laws)
As $N \to \infty$: $(N_c/N)^{\alpha_N/\alpha_D} \to 0$, so $L \to (D_c/D)^{\alpha_D}$.
As $D \to \infty$: $D_c/D \to 0$, so $L \to \left[(N_c/N)^{\alpha_N/\alpha_D}\right]^{\alpha_D} = (N_c/N)^{\alpha_N}$.
Both terms are positive, so $L(N, D)$ lies above each limit. ∎
:::
:::

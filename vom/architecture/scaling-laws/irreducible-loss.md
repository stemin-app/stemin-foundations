---
title: The floor under the loss
---

::: card
The laws so far send the loss to zero as the resources grow without bound. That cannot be true.
The test loss splits into two parts:

$$ L = L_\infty + L_\text{red} $$

$L_\infty$ is the irreducible part, the uncertainty that lives in the text itself. $L_\text{red}$
is the reducible part, everything above that floor, and only this part shrinks with scale.
:::

::: card
Read "I flipped a coin and got" and guess the next word. No model, however large, predicts
"heads" or "tails" better than chance: the outcome is random given the context. Natural language
is full of such choices. Which synonym the author picked, a random number in the text, the name
of a person in a story. None of it can be learned away.
:::

::: card
Estimates put the floor near $L_\infty \approx 1.5$ nats per token for natural language. The
matching perplexity is

$$ e^{1.5} \approx 4.5 $$

so even a perfect model would choose, on average, as if among four or five equally likely tokens.
[Perplexity](reference:perplexity)
:::

::: card
Put the floor into the law and give each resource its own reducible term:

$$ L(N, D) = L_\infty + \left(\frac{N_c}{N}\right)^{\alpha_N} + \left(\frac{D_c}{D}\right)^{\alpha_D} $$

Each power term falls to zero as its resource grows, and the loss falls to $L_\infty$. This
additive form is the one the Chinchilla paper fits.
[Loss with a floor](reference:chinchilla-loss)
:::

::: card
Add a floor to Kaplan's parameter law and plot it on log-log axes. Far above the floor the curve
follows the straight power law, the dashed line. As the reducible part shrinks, the floor takes
over and the curve flattens onto it. Drag $L_\infty$ across the range of estimates.

```plot
x: { var: N, label: "$N$", from: 1e6, to: 1e16, scale: log, ticks: 1, grid: true }
y: { label: "$L$ in nats", from: 0.5, to: 7, scale: log, ticks: 1, grid: true }

inputs:
  - { name: linf, min: 0.5, max: 1.8, default: 1.5, step: 0.1, label: "the floor L∞" }

draw:
  - curve: { is: (8.8e13 / N)^0.076, dash: true, label: "power law" }
  - hline: { at: linf, dash: true, label: "L∞" }
  - curve: { is: linf + (8.8e13 / N)^0.076, accent: true }
```
:::

::: card
For today's models the reducible part is still large, so the floor is often left out. As models
improve, the floor becomes a larger share of the loss, and each new factor of scale buys less.
To see the power law again near the floor, plot $L - L_\infty$ instead of $L$: the reducible
part alone is the straight line.
:::

::: card
Measuring $L_\infty$ is hard. It means extrapolating today's trends far beyond any scale anyone
has trained. Different methods give values between 1.0 and 1.8 nats, perplexities between
$e^{1.0} \approx 2.7$ and $e^{1.8} \approx 6.0$. That spread is the uncertainty in how good a
language model can ever become.
:::

::: exercise irreducible-tenfold
A model has a loss of 2.1 nats, and the floor is $L_\infty = 1.5$ nats. The reducible part
follows $N^{-0.076}$. What is the loss after the model grows ten times?

::: answer
About 2.00 nats. Only the reducible 0.6 nats shrinks, by the factor $10^{-0.076}$.
:::

::: solution
$$ L_\text{red} = 2.1 - 1.5 = 0.6\ \text{nats} $$

$$ L_\text{red}' = 0.6 \times 10^{-0.076} = 0.6 \times 0.839 = 0.504\ \text{nats} $$

$$ L' = 1.5 + 0.504 = 2.00\ \text{nats} $$

∎
:::
:::

::: exercise irreducible-perplexity
If the floor is $L_\infty = 1.0$ nat, what is the perplexity of a perfect model?

::: answer
$e \approx 2.72$. Perplexity is $e$ raised to the loss in nats.
:::
:::

::: exercise irreducible-bend
On log-log axes, why does the curve of $L$ against $N$ bend near $L_\infty$, while the curve of
$L - L_\infty$ stays straight?

::: answer
Only $L - L_\infty$ is a power law in $N$. The constant $L_\infty$ does not shrink, so it dominates as the power term falls.
:::
:::

::: reference chinchilla-loss
# Loss with an irreducible floor

The test loss is a constant floor plus one reducible power term for each resource. Hoffmann and
colleagues fit this form in the Chinchilla paper, writing it as
$E + A/N^{\alpha} + B/D^{\beta}$.

::: equation
L(N, D) = L_\infty + \left(\frac{N_c}{N}\right)^{\alpha_N} + \left(\frac{D_c}{D}\right)^{\alpha_D}
:::

::: legend
$L$: the test loss, in nats per token
$L_\infty$: the irreducible loss, the entropy of the text itself
$N$: the number of parameters
$D$: the number of training tokens
$N_c, D_c$: fitted constants
$\alpha_N, \alpha_D$: fitted exponents, between 0 and 1
:::

::: derivation
Each term has a limit.
As $N \to \infty$: $(N_c/N)^{\alpha_N} \to 0$, because $\alpha_N > 0$.
As $D \to \infty$: $(D_c/D)^{\alpha_D} \to 0$, because $\alpha_D > 0$.
With both limits: $L \to L_\infty$.
Above the floor: $L - L_\infty$ is a sum of two power laws, each a straight line on log-log axes when the other term is small.[Power law](reference:power-law) ∎
:::
:::

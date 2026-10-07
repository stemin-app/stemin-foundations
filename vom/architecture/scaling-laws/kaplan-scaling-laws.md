---
title: Loss against parameters, data and compute
---

::: card
In January 2020 a team at OpenAI, Kaplan and colleagues, published "Scaling Laws for Neural
Language Models". They trained over 400 transformer language models, from 768 parameters to 1.5
billion, on datasets from 22 million to 23 billion tokens. They varied the width, the depth, the
batch size and the learning rate.
:::

::: card
They plotted the test loss against the model size on logarithmic axes, and the points fell on a
straight line. A straight line on log-log axes is a power law:

$$ L = a\,N^{-\alpha} \qquad \log L = \log a - \alpha \log N $$

No theory had predicted it. The line held across every size they trained, and the same shape
appeared for data and for compute.
[Power law](reference:power-law)
:::

::: card
For a model with $N$ parameters, trained on enough data:

$$ L(N) = \left(\frac{N_c}{N}\right)^{\alpha_N} \qquad N_c \approx 8.8 \times 10^{13}, \quad \alpha_N \approx 0.076 $$

$N_c$ is a fitted constant with no physical meaning: the formula gives a loss of 1 nat at
$N = N_c$. The exponent is what matters. Doubling $N$ divides the loss by
$2^{0.076} \approx 1.054$, about 5% better.
[Kaplan scaling laws](reference:kaplan-scaling-laws)
:::

::: card
For a model trained on $D$ tokens, with enough parameters:

$$ L(D) = \left(\frac{D_c}{D}\right)^{\alpha_D} \qquad D_c \approx 5.4 \times 10^{13}, \quad \alpha_D \approx 0.095 $$

The exponent is a little larger than $\alpha_N$, so data pays slightly better than parameters.
Doubling $D$ divides the loss by $2^{0.095} \approx 1.068$, about 6% better. The method was the
same: fix the model, vary the tokens, plot on log-log axes, fit a line.
:::

::: card
For a compute budget $C$ spent in the best way:

$$ L(C) = \left(\frac{C_c}{C}\right)^{\alpha_C} \qquad C_c \approx 3.1 \times 10^8\ \text{PF-days}, \quad \alpha_C \approx 0.050 $$

A PF-day is $10^{15}$ operations per second for a day, $8.64 \times 10^{19}$ FLOPs. This is the
smallest exponent, because compute must pay for both parameters and data. Ten times the compute
divides the loss by $10^{0.05} \approx 1.12$.
:::

::: card
Here are the parameter law and the data law on one set of log-log axes. Both are straight, and
the data line is a little steeper. At $10^9$ the parameter law gives 2.38 nats and the data law
gives 2.82 nats: a billion tokens is a small dataset, a billion parameters a fair model.

```plot
x: { var: n, label: "$N$ or $D$", from: 1e6, to: 1e13, scale: log, ticks: 1, grid: true }
y: { label: "$L$ in nats", from: 1, to: 6, scale: log, ticks: 1, grid: true }

draw:
  - curve: { is: (8.8e13 / n)^0.076, accent: true, label: "$L(N)$" }
  - curve: { is: (5.4e13 / n)^0.095, dash: true, label: "$L(D)$" }
  - point: { at: [1e9, 2.38], label: "2.38" }
  - point: { at: [1e9, 2.82], label: "2.82" }
```
:::

::: card
Nobody knows why the exponents take these values. No theory predicts $\alpha_N = 0.076$ rather
than 0.1 or 0.05. Two things constrain them. They are above zero, because bigger models do
better. They are small, because the returns diminish. All three lie between 0.05 and 0.1, which
may mean that the bottleneck is the same whichever resource you scale.
:::

::: card
A small change in the exponent is a large change in the end. Drag $\alpha_N$ and compare it with
the dashed line at 0.076. Both pass through 1 nat at $N_c$, off the right edge. A steeper line
sits higher, and the gap grows with every decade you move away from $N_c$.

```plot
x: { var: N, label: "$N$", from: 1e6, to: 1e13, scale: log, ticks: 1, grid: true }
y: { label: "$L$ in nats", from: 1, to: 10, scale: log, ticks: 1, grid: true }

inputs:
  - { name: an, min: 0.05, max: 0.12, default: 0.1, step: 0.005, label: "the exponent α of N" }

draw:
  - curve: { is: (8.8e13 / N)^0.076, dash: true, label: "0.076" }
  - curve: { is: (8.8e13 / N)^an, accent: true }
```
:::

::: card
The shape is universal, and the constants are not. GPT style decoders, BERT style encoders and
their variants all scale as power laws with similar exponents, so the laws describe learning from
text more than any one architecture. The numbers above come from OpenAI's own runs on WebText, a
curated web scrape. Other data, another tokenizer or another architecture gives other constants.
:::

::: card
In 2022 DeepMind's Chinchilla paper, by Hoffmann and colleagues, measured again with more careful
experiments. It found slightly different exponents and, more important, a different best trade
between parameters and data. Kaplan's work said to make models as large as possible. Chinchilla
showed that growing parameters and data together uses compute better. The power law survived;
the constants moved.
:::

::: exercise kaplan-tenfold-parameters
Under Kaplan's law $L(N) = (N_c/N)^{0.076}$, by what factor does the loss fall when the model
grows ten times?

::: answer
The loss is divided by $10^{0.076} \approx 1.19$, so it falls about 16%. The factor is $10^{\alpha_N}$.
:::

::: solution
$$ \frac{L(10N)}{L(N)} = 10^{-0.076} = e^{-0.076 \times 2.303} = e^{-0.175} = 0.839 $$

$1 / 0.839 = 1.19$, and $1 - 0.839 = 0.161$. ∎
:::
:::

::: exercise kaplan-loss-at-size
What loss does Kaplan's parameter law give for a model with $8.8 \times 10^{11}$ parameters?

::: answer
About 1.42 nats. The ratio $N_c / N$ is exactly 100.
:::

::: solution
$$ \frac{N_c}{N} = \frac{8.8 \times 10^{13}}{8.8 \times 10^{11}} = 100 $$

$$ L = 100^{0.076} = 10^{0.152} = 1.42\ \text{nats} $$

∎
:::
:::

::: exercise kaplan-halve-loss
Under $L(C) = (C_c/C)^{0.050}$, by what factor must the compute grow to halve the loss?

::: answer
By $2^{20} \approx 10^6$. Set $(C_1/C_2)^{0.05} = 1/2$ and solve for the ratio.
:::

::: solution
$$ \frac{L(C_2)}{L(C_1)} = \left(\frac{C_1}{C_2}\right)^{0.05} = \frac{1}{2} $$

$$ \frac{C_2}{C_1} = 2^{1/0.05} = 2^{20} = 1\,048\,576 \approx 10^6 $$

∎
:::
:::

::: reference kaplan-scaling-laws
# Kaplan scaling laws

The test loss of a transformer language model falls as a power law in each resource, when the
other resources are not the bottleneck. The constants are fits to OpenAI's 2020 runs on WebText,
with the loss in nats.

::: equation
L(N) = \left(\frac{N_c}{N}\right)^{\alpha_N} \qquad L(D) = \left(\frac{D_c}{D}\right)^{\alpha_D} \qquad L(C) = \left(\frac{C_c}{C}\right)^{\alpha_C}
:::

::: legend
$L$: the test loss, in nats per token
$N$: the number of parameters
$D$: the number of training tokens
$C$: the training compute, in PF-days
$N_c$: the fitted constant $8.8 \times 10^{13}$ parameters
$D_c$: the fitted constant $5.4 \times 10^{13}$ tokens
$C_c$: the fitted constant $3.1 \times 10^{8}$ PF-days
$\alpha_N$: the fitted exponent 0.076
$\alpha_D$: the fitted exponent 0.095
$\alpha_C$: the fitted exponent 0.050
:::
:::

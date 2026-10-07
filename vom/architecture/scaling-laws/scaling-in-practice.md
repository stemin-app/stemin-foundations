---
title: Using the laws
---

::: card
**To pick a model size**, start from the budget. With $C$ FLOPs and 20 tokens per parameter,
$N_\text{opt} = \sqrt{C/120}$ and $D_\text{opt} = 20N_\text{opt}$
([training compute](reference:training-compute)). If you have fewer than $20N$ tokens, you are
limited by data: take a smaller model, train the larger one anyway at some waste, or stretch the
data with repetition or synthesis.
:::

::: card
**To serve cheaply**, remember that training is paid once and inference at every query. An
application that answers millions of queries may do better with a smaller model trained on more
tokens than the compute-optimal split, accepting some training waste for a cheaper model to run.
Fine-tuning does not need its own scaling study: take the largest pretrained model you can afford
to run.
:::

::: card
**To do research**, fit before you commit. Train at 1%, 3% and 10% of the target scale, plot the
loss against compute on log-log axes, and extrapolate the line. To compare two methods, ask how
much compute the old one would need to match the new one: its **effective compute multiplier**. A
change that helps small models but flattens the slope will lose at scale.
:::

::: card
Most improvements shift the line, not its slope. A method worth a multiplier $m$ acts like $m$
times the compute: the line moves down in parallel. FlashAttention may be worth about 2, a mixture
of experts about 4 in training. A doubling of efficiency saves one doubling of compute, and leaves
the number of doublings needed for a given gain unchanged.

```plot
x: { var: C, label: "compute, PF-days", from: 1, to: 1e8, scale: log, ticks: 1, grid: true }
y: { label: "loss in nats", from: 1, to: 4, scale: log, grid: true }

inputs:
  - { name: m, min: 1, max: 100, default: 4, step: 1, label: "effective compute multiplier" }

draw:
  - curve: { is: (3.1e8 / C) ^ 0.05, dash: true, label: "baseline" }
  - curve: { is: (3.1e8 / (m * C)) ^ 0.05, accent: true, label: "improved" }
```
:::

::: card
Loss is not the goal; tasks are. Task accuracy often follows a sigmoid of the loss: flat near
chance, then steep, then saturated. So one gain in loss buys little on an easy task that is already
saturated, much on a task in its steep middle, and nothing on a task still below its threshold.
Knowledge tasks improve steadily with size; reasoning tasks sit lower and jump later.
:::

::: card
For policy, compute is the lever. It is measurable and controllable in a way that algorithms and
data are not, so scaling laws inform forecasts, export rules and agreements about training runs.
Emergence argues for scaling in steps, with an evaluation at each one, since a model just past the
frontier may do things no one has tested.
:::

::: exercise q1
You have $1.2 \times 10^{22}$ FLOPs. What model size and how many tokens does the 20 tokens per
parameter rule give?

::: answer
10 billion parameters and 200 billion tokens. $\sqrt{1.2 \times 10^{22} / 120} = 10^{10}$.
:::
:::

::: exercise q2
A new method has an effective compute multiplier of 4. With $\alpha_C = 0.05$, by what factor does
it lower the loss at a fixed budget?

::: answer
It divides the loss by $4^{0.05} \approx 1.07$, about 7% lower.
:::
:::

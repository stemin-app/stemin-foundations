---
title: The limits of scaling
---

::: card
The laws say the loss keeps falling as compute, data and parameters grow. Each resource has a
real limit. Data runs out first.
:::

::: card
The 20 tokens per parameter rule asks a lot. A model of a trillion parameters needs 20 trillion
tokens. The public web, as in Common Crawl, holds about 100 trillion tokens, mostly spam,
duplicates and boilerplate; after filtering, perhaps 10 to 20 trillion remain. Books add about
100 billion edited tokens, and code, papers and curated sites add more. Estimates of all good
digitized text run from 5 to 15 trillion tokens.
:::

::: card
The blue line is what the rule asks for, $20N$ tokens. The band is the estimated stock of good
text. A compute-optimal model larger than a few hundred billion parameters already asks for more
text than exists. Move the stock and watch where the two meet.

```plot
x: { var: N, label: "parameters N", from: 1e9, to: 1e13, scale: log, ticks: 1, grid: true }
y: { label: "tokens", from: 1e10, to: 1e15, scale: log, ticks: 1, grid: true }

inputs:
  - { name: st, min: 5, max: 30, default: 10, step: 1, label: "stock of good text, trillions of tokens" }

draw:
  - curve: { is: 20 * N, accent: true, label: "20 N needed" }
  - hline: { at: st * 1e12, dash: true, label: "stock" }
  - vline: { at: st * 1e12 / 20, dash: true }
```
:::

::: card
There are ways around the **data wall**, each with a cost. **Synthetic data** from existing models
works, but risks a slow drift away from natural text, called model collapse. **Repeating** the data
helps for 2 to 4 epochs, then the model memorizes. **Other modalities**, such as video, hold far
more data than text, but need other architectures. **Paying people** to write text gives quality
at a price too high for pretraining.
:::

::: card
Compute is limited too. One frontier run reportedly used about 25,000 GPUs for three months, over
$100 million in rented compute alone. Such a run can take a noticeable share of a year's output of
high-end chips, draw tens of megawatts for months, and spread one model over thousands of devices
whose communication slows the work. Chips no longer double their density every 18 months.
:::

::: card
And the returns shrink. With $\alpha_C \approx 0.05$, ten times the compute lowers the loss by only
about 11%, and halving the loss takes about a million times more
([Kaplan scaling laws](reference:kaplan-scaling-laws)). A larger model also costs more at every
query. At some point a dollar buys more as research, better data or a better method than as
scale.
:::

::: exercise q1
A compute-optimal model has 500 billion parameters. How many training tokens does it want, and is
that more than an estimated stock of 10 trillion?

::: answer
10 trillion, exactly the stock. Any larger model wants more than the stock holds.
:::
:::

::: exercise q2
With $\alpha_C = 0.05$, by what factor does the loss fall for 100 times more compute?

::: answer
It is divided by $100^{0.05} = 10^{0.1} \approx 1.26$, a drop of about 21%.
:::
:::

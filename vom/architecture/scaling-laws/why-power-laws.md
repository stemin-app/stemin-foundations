---
title: Where the power laws come from
---

::: card
Power laws turn up all through nature: the sizes of earthquakes, the populations of cities, the
frequencies of words, the abundance of species. A power law usually points to a process with no
scale of its own. Why training a network should produce one is an open question. Several
theories each explain a part, and none explains all of it.
:::

::: card
The first is the manifold hypothesis. Take every sequence of 1000 tokens from a vocabulary of
50,000. There are

$$ 50000^{1000} \approx 10^{4699} $$

of them, and almost all are gibberish. Coherent text fills a tiny part of this space, and the
hypothesis says that part is a smooth surface of much lower dimension.
:::

::: card
A network approximates that surface. A small network gets only a rough shape, like a plane laid
against a curved sheet. More parameters capture finer detail: curves, bumps, wrinkles. If the
surface has intrinsic dimension $d$, approximation theory says the error falls as $N^{-\alpha}$,
with $\alpha$ set by $d$ and by the smoothness. The weak point: the measured exponent barely
changes between datasets, and this theory says it should.
:::

::: card
The second view looks at random features. A freshly initialized network already computes a large
set of random nonlinear features of its input, about $N$ of them for $N$ parameters. Training
picks the useful ones. Suppose usefulness is heavy tailed: a few features, such as one that
tracks agreement between subject and verb, help a lot, and most help almost nothing.
:::

::: card
Then the number of useful features among $N$ random ones grows as $N^\alpha$ with $\alpha < 1$.
Doubling the parameters does not double the useful features, because useful features are rare.
This explains why the exponent is below one. It also suggests the exponent is universal, set by
the statistics of useful features and not by the task.
:::

::: card
The third view is the loss landscape. A small network has a rugged landscape, many local minima
behind high walls, and the optimizer gets stuck. In high dimensions most directions are neither
uphill nor downhill, and saddle points outnumber minima. A large network almost always has a
direction out of a bad region. This fits what you see in practice: larger models are easier to
train, and the learning rate and other settings carry over between sizes.
:::

::: card
The fourth view comes from statistical mechanics. A physical system at a critical point, such as
water at its boiling point, has power law correlations. Training may sit near a similar point,
balanced between underfitting and overfitting, near the threshold where the model can just fit
its data. Physics then predicts exponents fixed by a universality class, the same for every
system in it. Transformers of many sizes and shapes do share similar exponents.
:::

::: card
The fifth view puts the cause in the data. Word frequencies follow Zipf's law: the word of rank
$r$ appears with frequency proportional to $r^{-s}$, with $s \approx 1$ for English. A model with
more capacity learns rarer patterns, down to some cutoff rank $k$. The loss left is the mass of
the patterns beyond the cutoff, the shaded tail. Drag the cutoff and the exponent $s$.

```plot
x: { var: r, label: "rank $r$", from: 1, to: 1e6, scale: log, ticks: 1, grid: true }
y: { label: "frequency", from: 1e-8, to: 1, scale: log, ticks: 1, grid: true }

inputs:
  - { name: s, min: 0.8, max: 1.5, default: 1.1, step: 0.05, label: "the Zipf exponent s" }
  - { name: lk, min: 1, max: 5, default: 3, step: 0.25, label: "the cutoff log₁₀ k" }

draw:
  - area: { under: "r^(-s)", over: ["10^lk", 1e6], accent: true }
  - curve: "r^(-s)"
  - vline: { at: 10^lk, dash: true, label: "cutoff $k$" }
```
:::

::: card
For $s$ a little above 1, the tail beyond rank $k$ holds a mass proportional to $k^{1-s}$. That
is a power law in the cutoff with the small exponent $s - 1$. So if capacity pushes the cutoff
out in proportion to $N$, the loss inherits a power law in $N$. The exponents this predicts are
in the right range, though the details do not match.
[Zipf tail mass](reference:zipf-tail-mass)
:::

::: card
Each theory holds a piece. The manifold explains why larger models generalize better. Random
features explain why useful capacity grows slower than $N$. The landscape explains why large
models train easily. Statistical mechanics gives a frame for universality, and the data explains
why the exponent might be the same across tasks. Still, $\alpha_N \approx 0.076$ and
$\alpha_D \approx 0.095$ are measured, not predicted. No theory today derives them.
:::

::: exercise zipf-rank-ratio
Words follow Zipf's law with $s = 1$. How many times more often does the word of rank 1 appear
than the word of rank 10?

::: answer
10 times. The frequency ratio is $(10/1)^{s}$.
:::
:::

::: exercise manifold-count
How many distinct sequences of 1000 tokens can you build from a vocabulary of 50,000 tokens? Give
the power of ten.

::: answer
About $10^{4699}$. Take $1000 \times \log_{10} 50000$.
:::

::: solution
$$ \log_{10} 50000 = 4.699 $$

$$ \log_{10}\left(50000^{1000}\right) = 1000 \times 4.699 = 4699 $$

∎
:::
:::

::: exercise zipf-tail-doubling
The tail mass beyond rank $k$ is proportional to $k^{1-s}$, with $s = 1.1$. By what factor does
the tail shrink when the cutoff doubles?

::: answer
It is multiplied by $2^{-0.1} \approx 0.933$. The exponent is $1 - s = -0.1$.
:::
:::

::: reference zipf-tail-mass
# Zipf tail mass

When frequencies fall as a power of the rank with exponent $s > 1$, the total mass beyond a
cutoff rank $k$ is itself a power law in $k$.

::: equation
\int_k^\infty r^{-s}\,dr = \frac{k^{1-s}}{s - 1}
:::

::: legend
$r$: the rank of a word or pattern, 1 for the most frequent
$s$: the Zipf exponent, above 1
$k$: the cutoff rank, the rarest pattern the model has learned
:::

::: derivation
Treat the rank as continuous for large $k$.
An antiderivative of $r^{-s}$ is $\frac{r^{1-s}}{1-s}$, for $s \ne 1$.
Evaluate from $k$ to $\infty$: $\left[\frac{r^{1-s}}{1-s}\right]_k^\infty$.
With $s > 1$, $r^{1-s} \to 0$ as $r \to \infty$, so the upper end gives 0.
The lower end gives $-\frac{k^{1-s}}{1-s} = \frac{k^{1-s}}{s-1}$.
This is a power law in $k$ with exponent $1 - s$.[Power law](reference:power-law) ∎
:::
:::

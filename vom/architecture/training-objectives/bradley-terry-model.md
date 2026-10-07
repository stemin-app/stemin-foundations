---
title: From comparisons to scores
---

::: card
The reward model learns from comparisons through the **Bradley-Terry model**, built in the 1950s
to rank chess players and sports teams. Give each item a strength $p_i > 0$. The chance that
$i$ beats $j$ is its share of the two strengths:
[Bradley-Terry model](reference:bradley-terry)

$$ P(i \succ j) = \frac{p_i}{p_i + p_j} $$
:::

::: card
Check the formula on three cases. Equal strengths give $\frac{1}{2}$: an even match. With
$p_i = 2p_j$ the chance is $\frac{2p_j}{3p_j} = \frac{2}{3}$: twice as strong wins two games in three.
As $p_i / p_j$ grows without bound, the chance goes to 1.
:::

::: card
Write each strength as an exponential, $p_i = e^{r_i}$, so $r_i = \log p_i$. Divide top and bottom
by $e^{r_i}$ and the [sigmoid](reference:sigmoid) appears:

$$ P(i \succ j) = \frac{e^{r_i}}{e^{r_i} + e^{r_j}} = \frac{1}{1 + e^{-(r_i - r_j)}} = \sigma(r_i - r_j) $$

The chance depends on the difference of the log-strengths alone. RLHF calls these log-strengths
**rewards**.
:::

::: card
Drag the score difference $\Delta = r_i - r_j$. Equal scores give 50%. A difference of 2 gives 88%,
and $-2$ gives 12%. The curve is steepest at zero, where a small gap moves the chance most. Far out
it flattens: a lead of 10 or of 100 both mean a near-certain win.

```plot
x: { var: d, label: "score difference $Δ$", from: -6, to: 6, ticks: 1, grid: true }
y: { label: "$P(i ≻ j)$", from: 0, to: 1 }

inputs:
  - { name: D, min: -5, max: 5, default: 1, step: 0.1, label: "the difference Δ" }

draw:
  - hline: { at: 0.5, dash: true }
  - curve: { is: 1 / (1 + exp(-d)), accent: true }
  - point: { at: [0, 0.5], label: "50%" }
  - point: { at: [2, 0.881], label: "88%" }
  - point: { at: [-2, 0.119], label: "12%" }
  - vline: { at: D, dash: true }
  - point: { at: [D, 1 / (1 + exp(-D))], label: "$σ(Δ)$" }
```
:::

::: card
For one prompt, response A has reward $r_A = 1.5$ and response B has $r_B = -0.5$. The difference
is $2.0$:

$$ P(A \succ B) = \sigma(2.0) = \frac{1}{1 + e^{-2}} \approx 0.88 $$

Over 100 comparisons of these two responses, expect A to win about 88.
:::

::: card
Apply the model to responses. The reward model should give the winner the higher score, so that
$P(y_w \succ y_l \mid x) = \sigma\big(r_\phi(x, y_w) - r_\phi(x, y_l)\big)$. Maximize the
log-likelihood of the observed preferences:
[Reward model loss](reference:reward-model-loss)

$$ \mathcal{L}_{RM} = -\mathbb{E}_{(x, y_w, y_l) \sim \mathcal{D}}\Big[\log \sigma\big(r_\phi(x, y_w) - r_\phi(x, y_l)\big)\Big] $$
:::

::: card
Write $z$ for the margin $r_\phi(x, y_w) - r_\phi(x, y_l)$. Then $-\log \sigma(z) = \log(1 + e^{-z})$.
A large positive margin costs almost nothing. A negative one, the loser ahead, costs about $-z$.

The dashed curve is the size of the slope, $\sigma(-z)$. It is largest where the model ranks the
pair wrong, and it pushes $r_\phi(x, y_w)$ up and $r_\phi(x, y_l)$ down by the same amount.

```plot
x: { var: z, label: "margin $z$", from: -5, to: 5, ticks: 1, grid: true }
y: { label: "loss", from: 0, to: 5.5 }

draw:
  - curve: { is: "log(e(), 1 + exp(-z))", accent: true, label: "$-log σ(z)$" }
  - curve: { is: 1 / (1 + exp(z)), dash: true, label: "$σ(-z)$" }
  - point: { at: [0, "log(e(), 2)"], label: "$log 2$" }
```
:::

::: card
Only the difference of rewards enters the loss. Add the same constant to every reward and nothing
changes. The scale of the rewards is arbitrary, and that is fine: the reward model only has to
rank responses.
:::

::: card
The reward model compresses thousands of judgements into a function that scores prompts and
responses no annotator saw. If annotators prefer accurate answers, accuracy scores high.

It learns only what the comparisons say. Suppose the annotators preferred the shorter answer every
time. The model would learn $r_\phi(x, y) \approx -\text{len}(y)$ and reward brevity whatever the
content. That fits the data, and it is useless for alignment.
:::

::: exercise bt-triple-strength
Item $i$ is three times as strong as item $j$. What is $P(i \succ j)$?

::: answer
$0.75$. It is $\frac{3p_j}{3p_j + p_j}$.
:::
:::

::: exercise bt-margin-for-75
What reward difference makes a response win 75% of the time?

::: answer
$\Delta = \ln 3 \approx 1.099$. Solve $\sigma(\Delta) = 0.75$.
:::

::: solution
$\frac{1}{1 + e^{-\Delta}} = 0.75$

$1 + e^{-\Delta} = \frac{4}{3}$

$e^{-\Delta} = \frac{1}{3}$

$\Delta = \ln 3 = 1.0986$ ∎
:::
:::

::: exercise bt-rm-loss
The reward model gives $r_\phi(x, y_w) = 1.5$ and $r_\phi(x, y_l) = -0.5$. What is the loss on this
triple?

::: answer
$\approx 0.127$. The loss is $\log(1 + e^{-z})$ with $z = 2$.
:::

::: solution
$z = 1.5 - (-0.5) = 2$

$\log(1 + e^{-2}) = \ln(1 + 0.1353) = \ln 1.1353 = 0.1269$ ∎
:::
:::

::: exercise bt-gradient-at-zero
The reward model scores a pair equally, $z = 0$. What is $\frac{d}{dz}\left[-\log \sigma(z)\right]$
there?

::: answer
$-0.5$. The derivative is $-\sigma(-z)$, and $\sigma(0) = 0.5$.
:::
:::

::: reference bradley-terry
# Bradley-Terry model

The probability that item $i$ beats item $j$ is its share of their strengths. With rewards
$r = \log p$, it is the sigmoid of the reward difference.

::: equation
P(i \succ j) = \frac{p_i}{p_i + p_j} = \sigma(r_i - r_j)
:::

::: legend
$p_i, p_j$: the strengths, positive numbers
$r_i, r_j$: the rewards, $r = \log p$
$\sigma$: the sigmoid, $\sigma(z) = \frac{1}{1 + e^{-z}}$
:::

::: derivation
Goal: the sigmoid form.
Substitute $p_i = e^{r_i}$ and $p_j = e^{r_j}$: $P(i \succ j) = \frac{e^{r_i}}{e^{r_i} + e^{r_j}}$.
Divide numerator and denominator by $e^{r_i}$: $\frac{1}{1 + e^{r_j - r_i}} = \frac{1}{1 + e^{-(r_i - r_j)}}$.
This is the sigmoid of $r_i - r_j$.[Sigmoid](reference:sigmoid)
Adding a constant $c$ to both rewards multiplies both strengths by $e^c$ and leaves the share unchanged. ∎
:::
:::

::: reference reward-model-loss
# Reward model loss

A reward model is trained to maximize the Bradley-Terry likelihood of the observed preferences.

::: equation
\mathcal{L}_{RM} = -\mathbb{E}_{(x, y_w, y_l) \sim \mathcal{D}}\left[\log \sigma\big(r_\phi(x, y_w) - r_\phi(x, y_l)\big)\right]
:::

::: legend
$r_\phi(x, y)$: the reward model's score of response $y$ to prompt $x$
$\phi$: the reward model's parameters
$y_w, y_l$: the preferred and the rejected response
$\mathcal{D}$: the dataset of preference triples
:::

::: derivation
Goal: the loss, and its slope in the margin.
Each triple is a win of $y_w$ over $y_l$, with probability $\sigma(z)$ and $z = r_\phi(x, y_w) - r_\phi(x, y_l)$.[Bradley-Terry model](reference:bradley-terry)
The negative mean log-likelihood over the data is $-\mathbb{E}[\log \sigma(z)]$.[Expectation](reference:expectation)
Rewrite one term: $-\log \sigma(z) = \log(1 + e^{-z})$.
Differentiate: $\frac{d}{dz}\log(1 + e^{-z}) = \frac{-e^{-z}}{1 + e^{-z}} = -\sigma(-z)$.[Derivative](reference:derivative)
Since $\frac{\partial z}{\partial r_\phi(x, y_w)} = 1$ and $\frac{\partial z}{\partial r_\phi(x, y_l)} = -1$, a descent step raises the winner's score and lowers the loser's by equal amounts. ∎
:::
:::

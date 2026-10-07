---
title: Direct preference optimization
---

::: card
**Direct preference optimization**, DPO, skips the reward model and the reinforcement learning.
It starts from the closed form of the best KL-regularized policy
([KL-regularized objective](reference:kl-regularized-objective)):

$$ \pi^*(y \mid x) = \frac{1}{Z(x)}\,\pi_{ref}(y \mid x)\,\exp\left(\frac{r(x, y)}{\beta}\right) $$
:::

::: card
Solve it for the reward. Take the log and rearrange:

$$ r(x, y) = \beta \log \frac{\pi^*(y \mid x)}{\pi_{ref}(y \mid x)} + \beta \log Z(x) $$

A policy defines its own reward. The quantity $\beta \log(\pi / \pi_{ref})$ is the **implicit
reward** of a response.
:::

::: card
Put this reward into the [Bradley-Terry model](reference:bradley-terry). Only a difference of
rewards enters it, so the hard term $\beta \log Z(x)$, the same for both responses, cancels. What is
left depends on the policy alone, and it gives a loss on preference triples
([DPO loss](reference:dpo-loss)):

$$ \mathcal{L}_{DPO} = -\mathbb{E}\left[\log \sigma\left(\beta \log \frac{\pi_\theta(y_w \mid x)}{\pi_{ref}(y_w \mid x)} - \beta \log \frac{\pi_\theta(y_l \mid x)}{\pi_{ref}(y_l \mid x)}\right)\right] $$
:::

::: card
To train, compute four log probabilities: the winner and the loser, under the policy and under
the frozen reference. Combine them as in the formula and backpropagate. No reward model, no
sampling, no value network. DPO reaches results comparable to PPO with far simpler training.
:::

::: card
Write $h$ for the log-ratio margin, $\log\frac{\pi_\theta(y_w)}{\pi_{ref}(y_w)} - \log\frac{\pi_\theta(y_l)}{\pi_{ref}(y_l)}$.
The loss of a triple is $\log(1 + e^{-\beta h})$. At the start the policy equals the reference,
$h = 0$, and the loss is $\ln 2$. Raising the winner relative to the loser lowers it. $\beta$ sets
how far the margin must grow before the loss stops caring.

```plot
x: { var: h, label: "log-ratio margin h", from: -10, to: 20, ticks: 5, grid: true }
y: { label: "loss", from: 0, to: 4 }

inputs:
  - { name: b, min: 0.05, max: 1, default: 0.1, step: 0.05, label: "β" }

draw:
  - curve: { is: "log(e(), 1 + exp(-b * h))", accent: true }
  - point: { at: [0, 0.693], label: "ln 2" }
```
:::

::: card
Line up the objectives of this topic. Language modeling predicts the next token of raw text, and
builds generation and broad knowledge. Masked language modeling fills blanks on both sides, and
builds understanding. Instruction tuning trains on the responses of curated pairs, and teaches the
model to follow requests. RLHF and DPO learn from human comparisons, and align the model with
what people prefer.
:::

::: exercise q1
With $\beta = 0.1$, the policy raises the winner's log ratio to 2 and lowers the loser's to $-1$.
What is the DPO loss on this triple?

::: answer
About 0.554. The margin is $h = 3$ and $\beta h = 0.3$.
:::

::: solution
$h = 2 - (-1) = 3$, so $\beta h = 0.3$.

$\log(1 + e^{-0.3}) = \ln(1 + 0.741) = \ln 1.741 \approx 0.554$. ∎
:::
:::

::: exercise q2
Why does the normalizer $Z(x)$ not appear in the DPO loss?

::: answer
The Bradley-Terry model uses only the difference of the two rewards. Both rewards contain the same
$\beta \log Z(x)$, so it cancels.
:::
:::

::: exercise q3
At the first step of DPO the policy equals the reference. What is the loss of every triple?

::: answer
$\ln 2 \approx 0.693$. Both log ratios are 0, so $h = 0$ and the loss is $\log(1 + e^0)$.
:::
:::

::: reference dpo-loss
# DPO loss

Direct preference optimization trains the policy on preference triples with the Bradley-Terry
likelihood of its implicit rewards, $\beta \log(\pi_\theta / \pi_{ref})$.

::: equation
\mathcal{L}_{DPO} = -\mathbb{E}_{(x, y_w, y_l)}\left[\log \sigma\left(\beta \log \frac{\pi_\theta(y_w \mid x)}{\pi_{ref}(y_w \mid x)} - \beta \log \frac{\pi_\theta(y_l \mid x)}{\pi_{ref}(y_l \mid x)}\right)\right]
:::

::: legend
$\pi_\theta$: the policy being trained
$\pi_{ref}$: the frozen reference policy
$y_w, y_l$: the preferred and the rejected response to prompt $x$
$\beta$: the weight of the KL penalty
$\sigma$: the sigmoid
:::

::: derivation
The best KL-regularized policy is $\pi^* = \pi_{ref}\, e^{r/\beta} / Z$.[KL-regularized objective](reference:kl-regularized-objective)
Take logs: $r(x, y) = \beta \log \frac{\pi^*(y \mid x)}{\pi_{ref}(y \mid x)} + \beta \log Z(x)$.
The preference probability is $\sigma\big(r(x, y_w) - r(x, y_l)\big)$.[Bradley-Terry model](reference:bradley-terry)
In the difference, $\beta \log Z(x)$ cancels.
Replace $\pi^*$ by the trained $\pi_\theta$ and take the negative mean log-likelihood over the triples. ∎
:::
:::

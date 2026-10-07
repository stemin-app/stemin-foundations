---
title: Optimizing the policy
---

::: card
In the third stage of RLHF the reward model is frozen and the language model trains. Call the
language model the **policy**, $\pi_\theta$. For a prompt $x$ it samples a response $y$, token by
token, and the response earns the reward $r_\phi(x, y)$. The plain goal is the expected reward:

$$ J(\theta) = \mathbb{E}_{x \sim \mathcal{D},\; y \sim \pi_\theta(\cdot \mid x)}\big[r_\phi(x, y)\big] $$
:::

::: card
Chasing reward alone fails. The reward model is a proxy, and a strong optimizer finds its blind
spots: responses that score high and are bad. This is **reward hacking**. If the reward model
learned that confident text tends to win, the policy can learn to sound sure of everything,
right or wrong.
:::

::: card
The fix keeps the policy near a **reference** $\pi_{ref}$, the model before RLHF. Subtract a
[KL divergence](reference:kl-divergence) penalty, scaled by $\beta$
([KL-regularized objective](reference:kl-regularized-objective)):

$$ J(\theta) = \mathbb{E}\left[r_\phi(x, y) - \beta \log \frac{\pi_\theta(y \mid x)}{\pi_{ref}(y \mid x)}\right] $$

A response the reference finds unlikely, with $\pi_\theta \gg \pi_{ref}$, has a large log ratio and
loses reward. Typical values of $\beta$ lie between 0.01 and 0.1.
:::

::: card
The best policy under this objective has a closed form: the reference, tilted by the reward,
$\pi^*(y \mid x) \propto \pi_{ref}(y \mid x)\, e^{r(x, y)/\beta}$. Take two responses: a better one
with reward advantage $\Delta$, and the rest. Slide $\beta$. Large $\beta$ keeps the reference
probability. Small $\beta$ pushes the better response toward probability 1, whatever the proxy
says.

```plot
x: { var: b, label: "β", from: 0.01, to: 10, scale: log, grid: true }
y: { label: "π* of the better response", from: 0, to: 1.05 }

inputs:
  - { name: D, min: 0.1, max: 3, default: 1, step: 0.1, label: "reward advantage Δ" }
  - { name: p, min: 0.05, max: 0.9, default: 0.2, step: 0.05, label: "reference probability" }

draw:
  - hline: { at: p, dash: true, label: "reference" }
  - curve: { is: p * exp(D / b) / (p * exp(D / b) + 1 - p), accent: true }
```
:::

::: card
The standard optimizer is **proximal policy optimization**, PPO. Updating $\theta$ changes the very
distribution the samples come from, which makes training unstable. PPO limits each step. Let
$\rho_t = \pi_\theta(y_t \mid \cdot) / \pi_{\theta_{old}}(y_t \mid \cdot)$ be the ratio of new to old
probability of token $t$, and $\hat{A}_t$ the **advantage**, how much better the token did than
expected. PPO maximizes

$$ \mathbb{E}\Big[\min\big(\rho_t \hat{A}_t,\; \mathrm{clip}(\rho_t, 1 - \epsilon, 1 + \epsilon)\,\hat{A}_t\big)\Big], \qquad \epsilon \approx 0.2 $$
:::

::: card
The clip removes the reward for moving far. With a positive advantage, the objective stops rising
once $\rho_t$ passes $1 + \epsilon$, so there is no gain in pushing further. With a negative one,
it stops falling below $1 - \epsilon$. Slide the advantage and $\epsilon$.

```plot
x: { var: q, label: "probability ratio ρ", from: 0, to: 2, ticks: 0.25, grid: true }
y: { label: "objective", from: -2.5, to: 2.5 }

inputs:
  - { name: A, min: -1, max: 1, default: 1, step: 0.1, label: "advantage Â" }
  - { name: e, min: 0.05, max: 0.5, default: 0.2, step: 0.05, label: "clip range ε" }

let:
  c: "max(1 - e, min(q, 1 + e))"

draw:
  - curve: { is: q * A, dash: true, label: "unclipped" }
  - curve: { is: "min(q * A, c * A)", accent: true, label: "PPO" }
  - vline: { at: 1, dash: true }
```
:::

::: card
PPO needs more machinery: a value network to estimate the advantages, and a way to split the
reward of a whole response among its tokens. Implementations are intricate. The idea is not: take
small steps, clip large changes, and improve the policy gradually.
:::

::: exercise q1
With $\beta = 0.1$, the policy gives a response probability $0.4$ and the reference gives it
$0.1$. The reward is 2. What is the penalized reward $r - \beta \log(\pi_\theta / \pi_{ref})$?

::: answer
About 1.861. The log ratio is $\ln 4 \approx 1.386$.
:::

::: solution
$\ln(0.4 / 0.1) = \ln 4 = 1.386$.

$2 - 0.1 \times 1.386 = 2 - 0.139 = 1.861$. ∎
:::
:::

::: exercise q2
With $\epsilon = 0.2$ and $\hat{A}_t = 1$, what is the PPO objective for the ratios $\rho = 1.1$ and
$\rho = 1.5$?

::: answer
1.1 and 1.2. The second ratio is clipped to $1 + \epsilon = 1.2$.
:::
:::

::: exercise q3
In the two-response example, the reference gives the better response $0.2$, its reward advantage
is $\Delta = 1$, and $\beta = 1$. What probability does $\pi^*$ give it?

::: answer
About 0.405. $0.2e / (0.2e + 0.8)$.
:::

::: solution
$0.2 \times e^{1} = 0.2 \times 2.718 = 0.544$.

$0.544 / (0.544 + 0.8) = 0.544 / 1.344 \approx 0.405$. ∎
:::
:::

::: reference kl-regularized-objective
# KL-regularized objective

RLHF maximizes the expected reward minus a penalty for moving away from the reference policy. Its
best policy is the reference tilted by the exponential of the reward.

::: equation
J(\pi) = \mathbb{E}_{y \sim \pi}\big[r(x, y)\big] - \beta\, D_{KL}\big(\pi(\cdot \mid x) \,\|\, \pi_{ref}(\cdot \mid x)\big), \qquad \pi^*(y \mid x) = \frac{\pi_{ref}(y \mid x)\, e^{r(x, y)/\beta}}{Z(x)}
:::

::: legend
$\pi$: the policy, a distribution over responses $y$
$\pi_{ref}$: the reference policy, the model before RLHF
$r(x, y)$: the reward of response $y$ to prompt $x$
$\beta$: the weight of the penalty
$Z(x)$: the sum $\sum_y \pi_{ref}(y \mid x)\, e^{r(x, y)/\beta}$ that makes $\pi^*$ a distribution
:::

::: derivation
Write $J = \sum_y \pi(y)\left[r(y) - \beta \log \frac{\pi(y)}{\pi_{ref}(y)}\right]$.[KL divergence](reference:kl-divergence)
Divide by $-\beta$ and add $\log Z$: $-J/\beta + \log Z = \sum_y \pi(y) \log \frac{\pi(y)}{\pi_{ref}(y)\, e^{r(y)/\beta} / Z} = D_{KL}(\pi \,\|\, \pi^*)$.
$\log Z$ does not depend on $\pi$, so maximizing $J$ minimizes $D_{KL}(\pi \,\|\, \pi^*)$.
A KL divergence is zero only when the two distributions are equal, so the maximum is at $\pi = \pi^*$. ∎
:::
:::

---
title: A distribution over words
---

::: card
A transformer ends with a distribution over a vocabulary of $V$ words. This is a **categorical
distribution**: $V$ possible outcomes with probabilities $p_1, p_2, \ldots, p_V$, none negative,
and

$$ \sum_{i=1}^{V} p_i = 1 $$
:::

::: card
The network itself outputs $V$ real numbers, one per word, called **logits**. A logit can be any
real number: positive or negative, large or small. To become probabilities they must turn
non-negative and add up to 1.

The **softmax** does this:[Softmax](reference:softmax)

$$ \operatorname{softmax}(z_i) = \frac{\exp(z_i)}{\sum_{j=1}^{V} \exp(z_j)} $$
:::

::: card
Take $V = 4$ and the logits $\mathbf{z} = [2.0,\ 1.0,\ 0.1,\ -1.0]$. First exponentiate each one:

$$ [e^{2.0},\ e^{1.0},\ e^{0.1},\ e^{-1.0}] = [7.39,\ 2.72,\ 1.11,\ 0.37] $$

Every result is positive. Their sum is $7.39 + 2.72 + 1.11 + 0.37 = 11.58$.
:::

::: card
Then divide each by the sum:

$$ \operatorname{softmax}(\mathbf{z}) = [0.638,\ 0.235,\ 0.095,\ 0.032] $$

Check: $0.638 + 0.235 + 0.095 + 0.032 = 1.000$. The four numbers form a valid distribution.
:::

::: card
The softmax keeps the order of the logits: the largest logit, 2.0, gets the largest probability,
0.638. It also amplifies: the logits 2.0 and 1.0 differ by 1, yet their probabilities differ by a
factor of $e \approx 2.72$.

It is *soft* because every word keeps some probability, unlike a hard maximum that picks one word
and zeroes the rest. That smoothness lets gradients flow to every logit during learning.
:::

::: card
Only the differences between logits matter. Add the same constant $c$ to every logit, and
$e^{c}$ appears in every numerator and in the denominator, where it cancels:

$$ \frac{e^{z_i + c}}{\sum_j e^{z_j + c}} = \frac{e^{c}\, e^{z_i}}{e^{c} \sum_j e^{z_j}} = \frac{e^{z_i}}{\sum_j e^{z_j}} $$
:::

::: card
A **temperature** $T$ divides the logits before the softmax, $p_i \propto e^{z_i / T}$. These are
the four logits above. At $T = 1$ you get the distribution you computed. Lower $T$ and the
distribution sharpens toward the top word: at $T = 0.5$ it is $[0.862,\ 0.117,\ 0.019,\ 0.002]$.
Raise $T$ and it flattens toward $\frac{1}{4}$ each.[Softmax with temperature](reference:logit-temperature)

```plot
x: { var: k, label: "word $i$", from: 0, to: 5, ticks: 1 }
y: { label: "$pᵢ$", from: 0, to: 1, ticks: 0.25, grid: true }

inputs:
  - { name: T, min: 0.2, max: 5, default: 1, step: 0.1, label: "temperature T" }

let:
  u1: exp(2 / T)
  u2: exp(1 / T)
  u3: exp(0.1 / T)
  u4: exp(-1 / T)
  s: u1 + u2 + u3 + u4

draw:
  - hline: { at: 0.25, dash: true, label: "uniform" }
  - area: { under: u1 / s, over: [0.7, 1.3], accent: true }
  - area: { under: u2 / s, over: [1.7, 2.3] }
  - area: { under: u3 / s, over: [2.7, 3.3] }
  - area: { under: u4 / s, over: [3.7, 4.3] }
```
:::

::: exercise q1
A model outputs the logits $[0,\ 0,\ 0,\ 0]$. What is the softmax?

::: answer
$[0.25,\ 0.25,\ 0.25,\ 0.25]$. Equal logits give equal probabilities.
:::

::: solution
$$ e^0 = 1 \text{ for each logit, and } 1 + 1 + 1 + 1 = 4 $$

$$ \operatorname{softmax} = \left[\tfrac{1}{4},\ \tfrac{1}{4},\ \tfrac{1}{4},\ \tfrac{1}{4}\right] $$

∎
:::
:::

::: exercise q2
Two words have the logits $[\ln 3,\ 0]$. What is the softmax?

::: answer
$[0.75,\ 0.25]$. The exponentials are 3 and 1.
:::

::: solution
$$ e^{\ln 3} = 3, \qquad e^{0} = 1, \qquad 3 + 1 = 4 $$

$$ \operatorname{softmax} = \left[\tfrac{3}{4},\ \tfrac{1}{4}\right] = [0.75,\ 0.25] $$

∎
:::
:::

::: exercise q3
You add 5 to every logit of $[2.0,\ 1.0,\ 0.1,\ -1.0]$. What is the new softmax?

::: answer
$[0.638,\ 0.235,\ 0.095,\ 0.032]$, unchanged. A shift common to all logits cancels.
:::
:::

::: exercise q4
Two words have the logits $[1,\ 0]$. What probability does the first word get at temperature
$T = 0.5$?

::: answer
About $0.881$. Divide by $T$ first: the logits become $[2,\ 0]$.
:::

::: solution
$$ \left[\tfrac{1}{0.5},\ \tfrac{0}{0.5}\right] = [2,\ 0] $$

$$ p_1 = \frac{e^{2}}{e^{2} + e^{0}} = \frac{7.389}{8.389} \approx 0.881 $$

∎
:::
:::

::: reference logit-temperature
# Softmax with temperature

Dividing the logits by a temperature before the softmax controls how sharp the distribution is. A
low temperature concentrates the probability on the largest logit; a high one spreads it toward
uniform.

::: equation
p_i = \frac{\exp(z_i / T)}{\sum_{j=1}^{V} \exp(z_j / T)}
:::

::: legend
$p_i$: the probability of word $i$
$z_i$: the logit of word $i$
$T$: the temperature, a positive number; $T = 1$ is the plain softmax
$V$: the size of the vocabulary
:::

::: derivation
Put $z_i / T$ in place of $z_i$ in the softmax.[Softmax](reference:softmax)
As $T \to \infty$, every $z_i / T \to 0$, so every $\exp(z_i / T) \to 1$ and $p_i \to 1/V$.
As $T \to 0$, the gap $(z_{\max} - z_i)/T$ grows without bound for every $z_i < z_{\max}$.
Divide top and bottom by $\exp(z_{\max}/T)$: each other term becomes $\exp(-(z_{\max} - z_i)/T) \to 0$.
So the largest logit takes all the probability, as long as it is unique. ∎
:::
:::

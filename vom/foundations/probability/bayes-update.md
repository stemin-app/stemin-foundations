---
title: Bayes' rule
---

::: card
A conditional often comes in the wrong direction. A spam filter learns how often the word "free"
appears in spam, $P(\text{free} \mid \text{spam})$. When an email arrives, you want the reverse: how
likely the email is spam, given that it contains "free", $P(\text{spam} \mid \text{free})$.

The two are different numbers. **Bayes' rule** turns one into the other.
:::

::: card
Write the probability of "$A$ and $B$" in two ways, once through each conditional, and set them
equal. Divide by $P(B)$:[Bayes' rule](reference:bayes-rule)

$$ P(A \mid B) = \frac{P(B \mid A)\, P(A)}{P(B)} $$
:::

::: card
Each part has a name. $P(A)$ is the **prior**: your belief in $A$ before the evidence. $P(B \mid A)$
is the **likelihood**: how well $A$ explains the evidence $B$. $P(A \mid B)$ is the **posterior**:
your belief in $A$ after the evidence. $P(B)$ scales the result so the posteriors add up to 1.
:::

::: card
You find $P(B)$ by splitting on whether $A$ holds:[Total probability](reference:total-probability)

$$ P(B) = P(B \mid A)\, P(A) + P(B \mid \text{not } A)\, P(\text{not } A) $$

Every way to see $B$ goes through either $A$ or "not $A$", and the two paths do not overlap.
:::

::: card
Say 20% of email is spam. "free" appears in half of all spam and in 5% of other email:

$$ P(\text{free}) = 0.5 \times 0.2 + 0.05 \times 0.8 = 0.10 + 0.04 = 0.14 $$

$$ P(\text{spam} \mid \text{free}) = \frac{0.5 \times 0.2}{0.14} = \frac{0.10}{0.14} \approx 0.71 $$

One word lifts the belief in spam from 20% to 71%.
:::

::: card
The curve maps each prior to its posterior. When the two likelihoods are equal, the evidence
cannot tell $A$ from "not $A$", and the curve lies on the dashed diagonal: the posterior equals the
prior. The further apart the likelihoods, the harder the curve bends away from it. With the spam
numbers, the point sits at prior 0.2 and posterior 0.71.

```plot
x: { var: h, label: "prior $P(A)$", from: 0, to: 1, ticks: 0.1, grid: true }
y: { label: "posterior $P(A | B)$", from: 0, to: 1, ticks: 0.1 }

inputs:
  - { name: h0, min: 0.01, max: 0.99, default: 0.2, step: 0.01, label: "prior P(A)" }
  - { name: a, min: 0.01, max: 1, default: 0.5, step: 0.01, label: "likelihood P(B | A)" }
  - { name: b, min: 0.01, max: 1, default: 0.05, step: 0.01, label: "likelihood P(B | ¬A)" }

let:
  post: h * a / (h * a + (1 - h) * b)
  post0: h0 * a / (h0 * a + (1 - h0) * b)

draw:
  - curve: { is: h, dash: true }
  - curve: { is: post, accent: true }
  - point: { at: [h0, post0], label: "posterior" }
```
:::

::: card
Today's posterior is tomorrow's prior. Read a second word, and you apply the rule again, starting
from 0.71 instead of 0.2. This is what the Bayesian reading of probability means in practice: a
belief that each piece of evidence updates.
:::

::: exercise q1
A disease affects 10% of people. A test is positive for 90% of the sick and for 10% of the healthy.
A person tests positive. What is the probability that they are sick?

::: answer
$0.5$. Find $P(\text{positive})$ first, by total probability.
:::

::: solution
$$ P(\text{pos}) = 0.9 \times 0.1 + 0.1 \times 0.9 = 0.09 + 0.09 = 0.18 $$

$$ P(\text{sick} \mid \text{pos}) = \frac{0.9 \times 0.1}{0.18} = \frac{0.09}{0.18} = 0.5 $$

∎
:::
:::

::: exercise q2
$P(B \mid A) = 0.6$, $P(A) = 0.5$ and $P(B) = 0.4$. What is $P(A \mid B)$?

::: answer
$0.75$. Apply Bayes' rule directly.
:::

::: solution
$$ P(A \mid B) = \frac{0.6 \times 0.5}{0.4} = \frac{0.3}{0.4} = 0.75 $$

∎
:::
:::

::: exercise q3
The evidence $B$ is equally likely whether $A$ holds or not: $P(B \mid A) = P(B \mid \text{not } A)$.
How does the posterior $P(A \mid B)$ compare with the prior $P(A)$?

::: answer
They are equal. $P(B)$ then equals the common likelihood, which cancels.
:::

::: solution
Let $P(B \mid A) = P(B \mid \text{not } A) = \ell$.

$$ P(B) = \ell\, P(A) + \ell\,(1 - P(A)) = \ell $$

$$ P(A \mid B) = \frac{\ell\, P(A)}{\ell} = P(A) $$

∎
:::
:::

::: reference bayes-rule
# Bayes' rule

The posterior probability of $A$ after seeing $B$ is the likelihood of $B$ under $A$, times the
prior of $A$, divided by the probability of $B$.

::: equation
P(A \mid B) = \frac{P(B \mid A)\, P(A)}{P(B)}
:::

::: legend
$P(A)$: the prior, the belief in $A$ before the evidence
$P(B \mid A)$: the likelihood, the probability of the evidence if $A$ holds
$P(B)$: the probability of the evidence, above zero
$P(A \mid B)$: the posterior, the belief in $A$ after the evidence
:::

::: derivation
$P(A \cap B) = P(A \mid B)\, P(B)$.[Conditional probability](reference:conditional-probability)
By the same definition with the roles swapped, $P(A \cap B) = P(B \mid A)\, P(A)$.
Set the two equal: $P(A \mid B)\, P(B) = P(B \mid A)\, P(A)$.
Divide by $P(B) > 0$: $P(A \mid B) = \dfrac{P(B \mid A)\, P(A)}{P(B)}$. ∎
:::
:::

::: reference total-probability
# Total probability

The probability of $B$ is the sum of its probability along each case of a split that covers every
outcome once: here, $A$ and "not $A$".

::: equation
P(B) = P(B \mid A)\, P(A) + P(B \mid \neg A)\, P(\neg A)
:::

::: legend
$\neg A$: the event "not $A$"
$P(B \mid A)$: the probability of $B$ when $A$ holds
$P(B \mid \neg A)$: the probability of $B$ when $A$ does not hold
:::

::: derivation
$B$ splits into $A \cap B$ and $\neg A \cap B$, which share no outcome, so $P(B) = P(A \cap B) + P(\neg A \cap B)$.
Write each part through its conditional: $P(A \cap B) = P(B \mid A)\, P(A)$ and $P(\neg A \cap B) = P(B \mid \neg A)\, P(\neg A)$.[Conditional probability](reference:conditional-probability)
Add the two parts. ∎
:::
:::

---
title: Independence
---

::: card
Two events are **independent** when knowing one tells you nothing about the other. Flip two fair
coins. The four outcomes HH, HT, TH and TT each have probability 25%. If the first coin shows
heads, the second still shows heads half the time:

$$ P(\text{coin 2 is H} \mid \text{coin 1 is H}) = P(\text{coin 2 is H}) = 0.5 $$
:::

::: card
Draw a card from a standard deck of 52: 26 red cards (13 hearts, 13 diamonds) and 26 black (13
spades, 13 clubs). Half the cards are red. But if you learn the card is a heart, it is certainly
red:

$$ P(\text{red}) = 0.5 \qquad P(\text{red} \mid \text{heart}) = 1 $$

Knowing one event changes the other, so "red" and "heart" are **not** independent.
:::

::: card
Independence has a test you can compute: $A$ and $B$ are independent exactly when the
probability of both is the product of their probabilities.[Independence](reference:independent-events-product)

$$ P(A \text{ and } B) = P(A) \times P(B) $$

It comes from $P(A \mid B) = P(A)$, the statement that $B$ does not change your belief in $A$,
multiplied through by $P(B)$.
:::

::: card
For the coins, the second coin does not care what the first did, so the chances multiply:
$P(\text{both heads}) = 0.5 \times 0.5 = 0.25$, which matches the table.

For the cards, the product predicts $P(\text{red}) \times P(\text{heart}) = 0.5 \times 0.25 = 0.125$.
The truth is $P(\text{red and heart}) = P(\text{heart}) = 0.25$, because every heart is red. The
test fails.
:::

::: card
Independence is what lets you predict a network at initialization. A neuron computes
$\text{output} = w_1 x_1 + w_2 x_2 + \cdots + w_n x_n$. When the terms are independent, their
variances add:[Variance of a sum](reference:variance-of-independent-sum)

$$ \operatorname{Var}(\text{output}) = \operatorname{Var}(w_1 x_1) + \operatorname{Var}(w_2 x_2) + \cdots + \operatorname{Var}(w_n x_n) $$
:::

::: card
With weights and inputs independent and of mean zero, each term has
$\operatorname{Var}(w_i x_i) = \operatorname{Var}(w)\operatorname{Var}(x)$. With $\operatorname{Var}(x) = 1$, the output
variance is $n \operatorname{Var}(w)$: it grows with the number of inputs. It stays at 1 only when
$\operatorname{Var}(w) = 1/n$, the rule behind Xavier initialization. For $n = 256$ that is
$\operatorname{Var}(w) = 1/256 \approx 0.0039$.

```plot
x: { var: n, label: "number of inputs $n$", from: 0, to: 512, ticks: 64, grid: true }
y: { label: "output variance", from: 0, to: 4, ticks: 1 }

inputs:
  - { name: v, min: 0.002, max: 0.02, default: 0.004, step: 0.0005, label: "weight variance" }

draw:
  - hline: { at: 1, dash: true }
  - curve: { is: n * v, accent: true }
  - point: { at: [1 / v, 1], label: "$n = 1 / Var(w)$" }
```
:::

::: card
Without independence the rule breaks. Take $Y = X$, the same variable twice. Then
$\operatorname{Var}(X + Y) = \operatorname{Var}(2X) = 4\operatorname{Var}(X)$, not $2\operatorname{Var}(X)$.
Two terms that move together add their spreads more than independent ones do.
:::

::: exercise q1
$A$ and $B$ are independent, with $P(A) = 0.3$ and $P(B) = 0.5$. What is $P(A \text{ and } B)$?

::: answer
$0.15$. Multiply the two probabilities.
:::
:::

::: exercise q2
$P(A) = 0.4$, $P(B) = 0.5$ and $P(A \text{ and } B) = 0.3$. Are $A$ and $B$ independent?

::: answer
No. The product is $0.4 \times 0.5 = 0.2$, not $0.3$.
:::
:::

::: exercise q3
$X$ and $Y$ are independent, with $\operatorname{Var}(X) = 2$ and $\operatorname{Var}(Y) = 3$.
What is $\operatorname{Var}(X + Y)$?

::: answer
$5$. Variances of independent variables add.
:::
:::

::: exercise q4
A neuron has 100 independent inputs, each of mean 0 and variance 1. Its weights are independent of
the inputs and of each other, with mean 0. What weight variance keeps the output variance at 1?

::: answer
$0.01$. The output variance is $n \operatorname{Var}(w)$, so $\operatorname{Var}(w) = 1/n$.
:::

::: solution
$$ \operatorname{Var}(\text{output}) = n \operatorname{Var}(w) \operatorname{Var}(x) = 100 \operatorname{Var}(w) \times 1 $$

$$ 100 \operatorname{Var}(w) = 1 \implies \operatorname{Var}(w) = 0.01 $$

∎
:::
:::

::: reference independent-events-product
# Independence

Two events are independent when the probability that both happen is the product of their
probabilities. Equivalently, knowing one does not change the probability of the other.

::: equation
P(A \cap B) = P(A)\, P(B) \iff P(A \mid B) = P(A) \quad (P(B) > 0)
:::

::: legend
$A, B$: two events
$P(A \cap B)$: the probability that both happen
:::

::: derivation
$P(A \mid B) = \dfrac{P(A \cap B)}{P(B)}$.[Conditional probability](reference:conditional-probability)
Put $P(A \mid B) = P(A)$: $P(A) = \dfrac{P(A \cap B)}{P(B)}$.
Multiply both sides by $P(B)$: $P(A)\, P(B) = P(A \cap B)$.
Each step reverses, so the two statements are equivalent. ∎
:::
:::

::: reference variance-of-independent-sum
# Variance of a sum

For independent random variables, the variance of the sum is the sum of the variances.

::: equation
\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) \qquad X, Y \text{ independent}
:::

::: legend
$X, Y$: independent random variables
$\operatorname{Var}$: the variance
:::

::: derivation
Let $\mu_X = \mathbb{E}[X]$ and $\mu_Y = \mathbb{E}[Y]$, so the mean of $X + Y$ is $\mu_X + \mu_Y$.[Linearity of expectation](reference:expectation-linearity)
$\operatorname{Var}(X + Y) = \mathbb{E}\left[\left((X - \mu_X) + (Y - \mu_Y)\right)^2\right]$.[Variance](reference:variance)
Expand: $\operatorname{Var}(X) + \operatorname{Var}(Y) + 2\,\mathbb{E}\left[(X - \mu_X)(Y - \mu_Y)\right]$.
Independence makes the joint probability the product $p(x)\, p(y)$, so the last mean factors: $\mathbb{E}[X - \mu_X]\,\mathbb{E}[Y - \mu_Y]$.[Independence](reference:independent-events-product)
Each factor is 0, so the cross term vanishes: $\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y)$. ∎
:::
:::

---
title: Random variables
---

::: card
You are about to roll a die. Before the roll, the outcome is not yet decided: it could be 1, 2,
3, 4, 5 or 6. Call that undecided outcome $X$. It is a **random variable**, a placeholder for an
outcome that has not happened yet.

You roll and get a 4. Now $x = 4$ is the value that occurred. The random variable is the
question; the value is the answer.
:::

::: card
Probability writes a random variable in italic uppercase, without bold: $X$, $Y$, $Z$. A value it
takes is the same letter in lowercase, $x$.

Keep this apart from linear algebra. There, bold uppercase $\mathbf{X}$ is a matrix and bold
lowercase $\mathbf{x}$ is a vector. Plain $X$ is a random variable, and plain $x$ is one number.
:::

::: card
$P(X = 4)$ asks: before the roll, how likely is it that $X$ comes out as 4? For a fair die it is
$\frac{1}{6}$. The shorthand $p(x)$ means the same as $P(X = x)$.

$$ p(1) = p(2) = p(3) = p(4) = p(5) = p(6) = \frac{1}{6} $$
:::

::: card
A language model has its own random variable. Let $Y$ be the next word the model predicts. One
possible value is $y = \text{cat}$. If the model gives "cat" 15% of its belief, you write

$$ P(Y = \text{cat}) = p(\text{cat}) = 0.15 $$
:::

::: card
Give a model the words "The cat sat on the". It should not answer with one word: that is too
confident. "mat" is likely, "floor" is possible, "elephant" is unlikely but not impossible. The
model outputs a **probability distribution** over its vocabulary: mat 0.35, floor 0.20, rug
0.15, bed 0.10, and so on down to elephant at 0.0001.

```plot
x: { var: k, label: "word", from: 0, to: 5 }
y: { label: "$p$", from: 0, to: 0.4, ticks: 0.1, grid: true }

draw:
  - area: { under: 0.35, over: [0.7, 1.3], accent: true }
  - area: { under: 0.20, over: [1.7, 2.3] }
  - area: { under: 0.15, over: [2.7, 3.3] }
  - area: { under: 0.10, over: [3.7, 4.3] }
  - point: { at: [1, 0.35], label: "mat" }
  - point: { at: [2, 0.20], label: "floor" }
  - point: { at: [3, 0.15], label: "rug" }
  - point: { at: [4, 0.10], label: "bed" }
```
:::

::: card
Training adjusts the parameters of the model so that its probabilities match what really comes
next. If the true next word was "mat", training raises $p(\text{mat})$. The loss function measures
how far off the model is, and it is built from probability.
:::

::: card
$P(\text{mat}) = 0.35$ has two readings. The **frequentist** reading: if the phrase came up many
times, about 35% of those times the next word would be "mat". The **Bayesian** reading: the model
holds a 35% degree of belief that the next word is "mat".

The Bayesian reading suits a network better. It never sees endless repeats. It sees a finite
training set and forms beliefs from it.
:::

::: card
Any set of beliefs must form a **valid distribution**: every probability is at least zero, and
together they add up to one.[A valid distribution](reference:valid-probability-distribution)

$$ P(x) \geq 0 \text{ for all } x \qquad \sum_x P(x) = 1 $$

Certainty puts 1 on one outcome and 0 on the rest. Complete uncertainty among $n$ outcomes puts
$1/n$ on each.
:::

::: card
The two rules are separate. The scores $[0.6,\ 0.5,\ -0.1]$ add up to 1, but one of them is
negative, so they are not a distribution. A sum of one is not enough on its own.
:::

::: exercise q1
A fair eight-sided die shows the numbers 1 to 8. What is $p(3)$?

::: answer
$\frac{1}{8} = 0.125$. Eight outcomes share the total of 1 equally.
:::
:::

::: exercise q2
A model gives three words the probabilities $[0.5,\ 0.3,\ 0.3]$. Is this a valid distribution?

::: answer
No. Every value is non-negative, but they add up to 1.1, not 1.
:::

::: solution
Check the signs: $0.5,\ 0.3,\ 0.3 \geq 0$.

Check the sum: $0.5 + 0.3 + 0.3 = 1.1 \neq 1$.

The second rule fails, so it is not a distribution. ∎
:::
:::

::: exercise q3
A model has no idea which of five words comes next. What probability does it give each word?

::: answer
$\frac{1}{5} = 0.2$. Complete uncertainty spreads the total of 1 evenly.
:::
:::

::: reference valid-probability-distribution
# A valid distribution

A list of probabilities over the outcomes of a discrete random variable is a distribution when
no value is negative and the values add up to one.

::: equation
p(x) \geq 0 \ \text{for all } x \qquad \sum_x p(x) = 1
:::

::: legend
$x$: one possible outcome
$p(x)$: the probability of that outcome, $P(X = x)$
:::
:::

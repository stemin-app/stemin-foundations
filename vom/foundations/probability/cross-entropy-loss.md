---
title: Cross-entropy
---

::: card
Training needs a number that says how wrong the model is. Take an image whose true class is "dog".
The model predicts:

- cat: 70%
- dog: 10%
- bird: 15%
- fish: 5%

The model is wrong: it thinks the image is most likely a cat. How wrong?
:::

::: card
Ask how surprised the model is by the truth. It gave the true class, dog, only 10%. The loss is
the surprise of the correct answer under the model:

$$ \text{loss} = -\log(\text{probability given to the correct answer}) $$

Here $\text{loss} = -\log 0.10 = 2.30$. This loss is the **cross-entropy**.
:::

::: card
The loss as a function of the probability $q$ the model gives the correct answer: 1% costs 4.61,
10% costs 2.30, 25% costs 1.39, 50% costs 0.69, 70% costs 0.36, 90% costs 0.11, and 99% costs 0.01.

```plot
x: { var: q, label: "probability of the correct answer $q$", from: 0, to: 1, ticks: 0.1, grid: true }
y: { label: "loss $-log q$", from: 0, to: 5, ticks: 1 }

inputs:
  - { name: q0, min: 0.01, max: 0.99, default: 0.1, step: 0.01, label: "the model's confidence" }

let:
  loss: -log(e(), q)
  loss0: -log(e(), q0)

draw:
  - curve: { is: loss, over: [0.006, 1], accent: true }
  - point: { at: [q0, loss0], label: "loss" }
```
:::

::: card
Two features of the curve shape training. A confident wrong answer costs a lot: raising $q$ from
1% to 10% cuts the loss by 2.30. A good answer gains little from more confidence: raising $q$ from
90% to 99% cuts it by only 0.10.

So the loss pushes hardest where the model is most wrong. It spends effort making bad predictions
less bad, not making good ones slightly better.
:::

::: card
Watch a model learn the dog image over five epochs. Its probability for "dog" climbs 20%, 40%, 60%,
80%, 90%, and the loss falls 1.61, 0.92, 0.51, 0.22, 0.11.

```plot
x: { var: t, label: "epoch", from: 0, to: 6, ticks: 1, grid: true }
y: { label: "loss", from: 0, to: 2, ticks: 0.5 }

draw:
  - points: { at: [[1, 1.61], [2, 0.92], [3, 0.51], [4, 0.22], [5, 0.11]], accent: true }
```
:::

::: card
When the truth is one class $j$, a **one-hot** target, the loss is the surprise of that class:

$$ \text{loss} = -\log q_j $$

The general form lets the truth $p$ be any distribution, and averages the surprise of the model
$q$ over it:[Cross-entropy](reference:cross-entropy)

$$ H(p, q) = -\sum_x p(x) \log q(x) $$
:::

::: card
When the model makes $q$ with a softmax, the gradient of this loss with respect to each logit is
the predicted probability minus the true one, $q_i - p_i$:[Gradient of softmax with cross-entropy](reference:softmax-cross-entropy-gradient)

- cat: $0.70 - 0 = +0.70$, push down
- dog: $0.10 - 1 = -0.90$, push up
- bird: $0.15 - 0 = +0.15$, push down
- fish: $0.05 - 0 = +0.05$, push down
:::

::: card
Gradient descent steps against the gradient. A positive entry lowers its logit; the one negative
entry, for the correct class, raises it. Each step moves probability away from the wrong answers
and onto the right one, with no extra rule.
:::

::: exercise q1
A model gives the correct class a probability of 0.25. What is its cross-entropy loss?

::: answer
$-\log 0.25 \approx 1.386$. With a one-hot target, the loss is the surprise of the correct class.
:::
:::

::: exercise q2
The truth is $p = [0.5,\ 0.5]$ and the model is $q = [0.25,\ 0.75]$. What is $H(p, q)$?

::: answer
About $0.837$. Average the surprise of $q$ under the weights of $p$.
:::

::: solution
$$ -\log 0.25 = 1.386, \qquad -\log 0.75 = 0.288 $$

$$ H(p, q) = 0.5 \times 1.386 + 0.5 \times 0.288 = 0.693 + 0.144 = 0.837 $$

∎
:::
:::

::: exercise q3
A softmax model predicts $q = [0.2,\ 0.5,\ 0.3]$, and the true class is the second. What is the
gradient of the cross-entropy loss with respect to the three logits?

::: answer
$[0.2,\ -0.5,\ 0.3]$. Subtract the one-hot target $[0,\ 1,\ 0]$ from $q$.
:::

::: solution
$$ q - p = [0.2 - 0,\ 0.5 - 1,\ 0.3 - 0] = [0.2,\ -0.5,\ 0.3] $$

∎
:::
:::

::: exercise q4
A model has a cross-entropy loss of $\log 2 \approx 0.693$ on one example with a one-hot target.
What probability did it give the correct class?

::: answer
$0.5$. Solve $-\log q = \log 2$ for $q$.
:::
:::

::: reference cross-entropy
# Cross-entropy

The cross-entropy of a model $q$ against a true distribution $p$ is the expected surprise of the
model, averaged over the truth. For a one-hot truth on class $j$ it is the surprise of that class.

::: equation
H(p, q) = -\sum_x p(x) \log q(x) \qquad H(p, q) = -\log q_j \ \text{ when } p \text{ is one-hot on } j
:::

::: legend
$p$: the true distribution
$q$: the distribution the model predicts
$H(p, q)$: the cross-entropy, in nats
$q_j$: the probability the model gives the correct class $j$
:::

::: derivation
Under the model, the surprise of $x$ is $-\log q(x)$.[Surprisal](reference:surprisal)
Average it with the true probabilities as weights: $H(p, q) = \sum_x p(x)\left(-\log q(x)\right)$.[Expectation](reference:expectation)
For a one-hot truth, $p(j) = 1$ and $p(x) = 0$ for every other $x$.
Every term but one vanishes: $H(p, q) = -1 \cdot \log q_j = -\log q_j$. ∎
:::
:::

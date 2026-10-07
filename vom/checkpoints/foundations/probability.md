---
title: Probability
---

Answer every question without notes. Take $\log$ as the natural logarithm, and give numbers to three decimal places where they do not come out exact.

::: exercise q1
$X$ takes the values $-1$, $0$ and $3$ with probabilities $0.5$, $0.3$ and $0.2$. What are
$\mathbb{E}[X]$ and $\operatorname{Var}(X)$?

::: answer
$\mathbb{E}[X] = 0.1$ and $\operatorname{Var}(X) = 2.29$.
:::

::: solution
$$ \mathbb{E}[X] = -1 \times 0.5 + 0 \times 0.3 + 3 \times 0.2 = -0.5 + 0.6 = 0.1 $$

$$ \mathbb{E}[X^2] = 1 \times 0.5 + 0 \times 0.3 + 9 \times 0.2 = 0.5 + 1.8 = 2.3 $$

$$ \operatorname{Var}(X) = 2.3 - 0.1^2 = 2.29 $$

∎
:::
:::

::: exercise q2
$X \sim \mathcal{N}(5, 16)$. What is the standard score of the value $x = -3$?

::: answer
$z = -2$.
:::

::: solution
$$ \sigma = \sqrt{16} = 4 $$

$$ z = \frac{-3 - 5}{4} = -2 $$

∎
:::
:::

::: exercise q3
A model outputs the logits $[\log 1,\ \log 2,\ \log 5]$. What is the softmax?

::: answer
$[0.125,\ 0.25,\ 0.625]$.
:::

::: solution
$$ [e^{\log 1},\ e^{\log 2},\ e^{\log 5}] = [1,\ 2,\ 5], \qquad 1 + 2 + 5 = 8 $$

$$ \operatorname{softmax} = \left[\tfrac{1}{8},\ \tfrac{2}{8},\ \tfrac{5}{8}\right] = [0.125,\ 0.25,\ 0.625] $$

∎
:::
:::

::: exercise q4
What is the entropy of the distribution $[0.5,\ 0.25,\ 0.25]$?

::: answer
$1.5 \log 2 \approx 1.040$ nats.
:::

::: solution
$$ H = -0.5 \log 0.5 - 2 \times 0.25 \log 0.25 = 0.5 \log 2 + 0.5 \log 4 $$

$$ = 0.5 \log 2 + 1.0 \log 2 = 1.5 \log 2 = 1.5 \times 0.693 \approx 1.040 $$

∎
:::
:::

::: exercise q5
A softmax classifier predicts $q = [0.1,\ 0.6,\ 0.3]$, and the true class is the third. What is the
cross-entropy loss, and what is its gradient with respect to the three logits?

::: answer
The loss is $-\log 0.3 \approx 1.204$, and the gradient is $[0.1,\ 0.6,\ -0.7]$.
:::

::: solution
$$ \text{loss} = -\log q_3 = -\log 0.3 \approx 1.204 $$

$$ q - p = [0.1 - 0,\ 0.6 - 0,\ 0.3 - 1] = [0.1,\ 0.6,\ -0.7] $$

∎
:::
:::

::: exercise q6
The truth is $p = [0.75,\ 0.25]$ and the model is $q = [0.5,\ 0.5]$. What is $D_{KL}(p \,\|\, q)$?

::: answer
About $0.131$ nats.
:::

::: solution
$$ 0.75 \log \frac{0.75}{0.5} = 0.75 \log 1.5 = 0.75 \times 0.405 = 0.304 $$

$$ 0.25 \log \frac{0.25}{0.5} = 0.25 \log 0.5 = 0.25 \times (-0.693) = -0.173 $$

$$ D_{KL}(p \,\|\, q) = 0.304 - 0.173 = 0.131 $$

∎
:::
:::

::: exercise q7
Two boxes are equally likely to be chosen. Box 1 holds 3 red balls and 1 blue ball. Box 2 holds 1
red ball and 3 blue balls. You draw one ball from the chosen box, and it is red. What is the
probability that you chose box 1?

::: answer
$0.75$.
:::

::: solution
$$ P(\text{red}) = 0.75 \times 0.5 + 0.25 \times 0.5 = 0.5 $$

$$ P(\text{box 1} \mid \text{red}) = \frac{0.75 \times 0.5}{0.5} = 0.75 $$

∎
:::
:::

::: exercise q8
You roll two fair dice. $A$ is "the first die is even", and $B$ is "the two dice add up to 7". Are
$A$ and $B$ independent?

::: answer
Yes. $P(A \text{ and } B) = \frac{1}{12} = P(A)\, P(B)$.
:::

::: solution
$$ P(A) = \frac{1}{2}, \qquad P(B) = \frac{6}{36} = \frac{1}{6} $$

Both happen for the pairs $(2, 5)$, $(4, 3)$ and $(6, 1)$:

$$ P(A \text{ and } B) = \frac{3}{36} = \frac{1}{12} = \frac{1}{2} \times \frac{1}{6} $$

The product rule holds, so $A$ and $B$ are independent. ∎
:::
:::

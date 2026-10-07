---
title: Training objectives
---

Answer every question without notes. Use the natural logarithm.

::: exercise q1
A model gives the true tokens of a four-token text the probabilities $0.5$, $0.5$, $0.5$ and
$0.125$. Find $\mathcal{L}_{LM}$ and the perplexity.

::: answer
$\mathcal{L}_{LM} = 6 \ln 2 \approx 4.159$ and $\text{PPL} = 2^{6/4} \approx 2.83$.
:::

::: solution
$\mathcal{L}_{LM} = 3\ln 2 + \ln 8 = 3\ln 2 + 3\ln 2 = 6\ln 2 \approx 4.159$.

Mean: $1.5 \ln 2$. $\text{PPL} = e^{1.5\ln 2} = 2^{1.5} \approx 2.83$. ∎
:::
:::

::: exercise q2
A model has a perplexity of 50 on a test set. What is its mean loss per token in nats?

::: answer
$\ln 50 \approx 3.912$.
:::
:::

::: exercise q3
With label smoothing $\epsilon = 0.1$ over $V = 10$ tokens, give the target of the correct token
and of each other token.

::: answer
$0.91$ and $0.01$.
:::
:::

::: exercise q4
An input of 2,000 tokens goes through standard BERT masking. How many positions add to the loss,
and how many of those show [MASK]?

::: answer
300 add to the loss, and 240 of them show [MASK].
:::
:::

::: exercise q5
LoRA with $r = 8$ adapts $\mathbf{W}_Q$ and $\mathbf{W}_V$, each $2{,}048 \times 2{,}048$, in 24
layers. How many numbers does it train?

::: answer
1,572,864.
:::

::: solution
One pair: $r(d + k) = 8 \times 4{,}096 = 32{,}768$.

Pairs: $24 \times 2 = 48$.

Total: $48 \times 32{,}768 = 1{,}572{,}864$. ∎
:::
:::

::: exercise q6
Response A has reward $0.5$ and response B has reward $-1$. By Bradley-Terry, what is
$P(A \succ B)$, and what is the reward model loss if A was preferred?

::: answer
$\sigma(1.5) \approx 0.818$, and the loss is $-\ln 0.818 \approx 0.201$.
:::
:::

::: exercise q7
Why does RLHF subtract a KL penalty toward the reference model?

::: answer
To stop reward hacking: the reward model is an imperfect proxy, and the penalty keeps the policy
near text the reference model finds likely.
:::
:::

::: exercise q8
With $\epsilon = 0.2$ and a negative advantage $\hat{A}_t = -1$, what is the PPO objective at the
ratio $\rho = 0.5$?

::: answer
$-0.8$. The ratio is clipped up to $0.8$, and $\min(-0.5, -0.8) = -0.8$.
:::
:::

::: exercise q9
With $\beta = 0.5$, the log-ratio margin of a triple is $h = 2$. What is its DPO loss?

::: answer
$\ln(1 + e^{-1}) \approx 0.313$.
:::
:::

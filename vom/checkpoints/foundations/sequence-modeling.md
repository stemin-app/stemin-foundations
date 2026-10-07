---
title: Sequence modeling
---

Answer every question without notes.

::: exercise q1
"The food was not good" and "The food was good, not bad" share most of their words. Which input
representation cannot tell "not good" from "good, not bad", and why?

::: answer
The bag of words. It counts the words and drops their order, and both phrases hold "not" and
"good".
:::
:::

::: exercise q2
A scalar RNN has $h_t = \tanh(0.5\,h_{t-1} + 0.5\,x_t)$ and $h_0 = 0$. Compute $h_1$ and $h_2$
for $x_1 = 2$, $x_2 = 2$.

::: answer
$h_1 \approx 0.762$ and $h_2 \approx 0.881$.
:::

::: solution
$h_1 = \tanh(0 + 1) = \tanh 1 \approx 0.762$.

$h_2 = \tanh(0.5 \cdot 0.762 + 1) = \tanh(1.381) \approx 0.881$. ∎
:::
:::

::: exercise q3
An RNN has hidden size 64 and input size 32. How many parameters does it have, biases included?

::: answer
6,208. $64^2 + 64 \cdot 32 + 64$.
:::
:::

::: exercise q4
Every step of a scalar RNN has $\frac{\partial h_t}{\partial h_{t-1}} = 0.8$. By what factor is
the gradient multiplied between step 21 and step 1?

::: answer
$0.8^{20} \approx 0.0115$.
:::
:::

::: exercise q5
$\mathbf{W}_h = \mathrm{diag}(1.2, 0.7)$, with tanh slopes near 1. Which direction explodes and
which vanishes as the sequence grows? Give both factors after 10 steps.

::: answer
The first direction explodes, $1.2^{10} \approx 6.19$; the second vanishes, $0.7^{10} \approx 0.028$.
:::
:::

::: exercise q6
A gradient $[3, 0, 4]^T$ is clipped to norm $\tau = 1$. What is the result?

::: answer
$[0.6, 0, 0.8]^T$. The norm is 5, so scale by $1/5$.
:::
:::

::: exercise q7
An LSTM cell has $c_{t-1} = 1$, $f_t = 0.5$, $i_t = 0.8$, $\tilde{c}_t = 0.5$ and $o_t = 1$.
Compute $c_t$ and $h_t$.

::: answer
$c_t = 0.9$ and $h_t = \tanh 0.9 \approx 0.716$.
:::

::: solution
$c_t = 0.5 \cdot 1 + 0.8 \cdot 0.5 = 0.5 + 0.4 = 0.9$.

$h_t = 1 \cdot \tanh(0.9) \approx 0.716$. ∎
:::
:::

::: exercise q8
Along the cell state of an LSTM, what is the Jacobian $\frac{\partial \mathbf{c}_t}{\partial \mathbf{c}_{t-1}}$,
holding the gates fixed? Why does it help the gradient survive?

::: answer
$\mathrm{diag}(\mathbf{f}_t)$. It holds no weight matrix and no tanh slope, and the network can
keep the forget gates near 1, so the product over many steps stays near the identity.
:::
:::

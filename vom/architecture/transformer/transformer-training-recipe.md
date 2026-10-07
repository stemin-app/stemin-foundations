---
title: The training recipe
---

::: card
The original transformer trains with [Adam](reference:adam), with $\beta_1 = 0.9$,
$\beta_2 = 0.98$ and $\epsilon = 10^{-9}$. Each parameter $\theta$ keeps its own running averages
$m$ and $v$ of the gradient and of its square, and steps by
$\alpha\,\hat{m}/(\sqrt{\hat{v}} + \epsilon)$.
:::

::: card
The learning rate is not fixed. It rises for a **warmup** of $w = 4{,}000$ steps, then decays:

$$ \alpha_t = d_{model}^{-0.5} \cdot \min\left(t^{-0.5},\; t \cdot w^{-1.5}\right) $$

During warmup the second term is smaller, and the rate grows linearly with $t$. After step $w$
the first term takes over, and the rate falls as $1/\sqrt{t}$.
:::

::: card
Early in training, Adam's averages rest on few gradients and are noisy. A large rate then would
throw the weights far off. Warmup holds the steps small until the estimates settle. The peak comes
at $t = w$, with $\alpha_{\max} = (d_{model} \cdot w)^{-0.5}$: about $7.0 \times 10^{-4}$ for
$d_{model} = 512$ and $w = 4{,}000$.

```plot
x: { var: t, label: "training step, thousands", from: 0.01, to: 40, ticks: 5, grid: true }
y: { label: "learning rate, ×10⁻⁴", from: 0, to: 30 }

inputs:
  - { name: wk, min: 1, max: 16, default: 4, step: 0.5, label: "warmup, thousands of steps" }
  - { name: d, min: 128, max: 1024, default: 512, step: 128, label: "model dimension" }

let:
  s: 1000 * t
  w: 1000 * wk
  a: "10000 * d ^ (-0.5) * min(s ^ (-0.5), s * w ^ (-1.5))"

draw:
  - curve: { is: a, accent: true }
  - vline: { at: wk, dash: true, label: "end of warmup" }
```
:::

::: card
Around this core sit a few standard practices. Weight matrices start from a Xavier (Glorot)
uniform draw, biases at 0, and every layer norm at $\boldsymbol{\gamma} = \mathbf{1}$,
$\boldsymbol{\beta} = \mathbf{0}$. Dropout with rate 0.1 acts on the attention weights and the FFN
activations. The loss uses label smoothing with $\epsilon = 0.1$.
:::

::: card
Batches mix sequences of different lengths, so shorter ones are **padded** to the longest. A
padding mask sets the attention scores of padded positions to $-\infty$, like the causal mask,
so real tokens never attend to padding. Gradient clipping caps the global norm of the gradient,
and mixed precision runs the forward and backward passes in 16-bit floats while keeping the
master weights in 32-bit.
:::

::: exercise q1
With $d_{model} = 512$ and $w = 4{,}000$, what is the learning rate at step 1,000?

::: answer
About $1.75 \times 10^{-4}$. In warmup, $\alpha_t = d_{model}^{-0.5}\, t\, w^{-1.5}$.
:::

::: solution
$d_{model}^{-0.5} = 1/22.63 = 0.0442$.

$w^{-1.5} = 1/252{,}982 = 3.95 \times 10^{-6}$.

$\alpha = 0.0442 \times 1{,}000 \times 3.95 \times 10^{-6} \approx 1.75 \times 10^{-4}$. ∎
:::
:::

::: exercise q2
After warmup, by what factor does the learning rate fall when the step count grows from 16,000
to 64,000?

::: answer
By 2. After warmup the rate is proportional to $t^{-0.5}$, and $\sqrt{64{,}000/16{,}000} = 2$.
:::
:::

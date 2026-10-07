---
title: Backpropagation through time
---

::: card
To train an RNN, unroll it over the whole sequence, compute the loss, and run backpropagation
through the unrolled chain. This is **backpropagation through time**, or BPTT. The loss may sit
at the last step only, or at every step.
:::

::: card
The weights are shared, so every step contributes to their gradient. Each step's contribution is
added:

$$ \frac{\partial \mathcal{L}}{\partial \mathbf{W}_h} = \sum_{t=1}^{T} \frac{\partial \mathcal{L}}{\partial \mathbf{h}_t} \frac{\partial \mathbf{h}_t}{\partial \mathbf{W}_h} $$

Here $\frac{\partial \mathbf{h}_t}{\partial \mathbf{W}_h}$ means the direct effect at step $t$ only,
with $\mathbf{h}_{t-1}$ held fixed.
:::

::: card
The trouble is in the first factor. When the loss sits at step $T$, the gradient reaches
$\mathbf{h}_1$ only through every step in between:

$$ \frac{\partial \mathcal{L}}{\partial \mathbf{h}_1} = \frac{\partial \mathcal{L}}{\partial \mathbf{h}_T} \cdot \frac{\partial \mathbf{h}_T}{\partial \mathbf{h}_{T-1}} \cdot \frac{\partial \mathbf{h}_{T-1}}{\partial \mathbf{h}_{T-2}} \cdots \frac{\partial \mathbf{h}_2}{\partial \mathbf{h}_1} $$

That is a product of $T - 1$ Jacobians, one per step. Its size decides whether the first input
can learn anything.
:::

::: card
Each factor comes from the RNN update. Differentiate $\mathbf{h}_t = \tanh(\mathbf{z}_t)$ with
$\mathbf{z}_t = \mathbf{W}_h\mathbf{h}_{t-1} + \mathbf{W}_x\mathbf{x}_t + \mathbf{b}$:

$$ \frac{\partial \mathbf{h}_t}{\partial \mathbf{h}_{t-1}} = \mathrm{diag}\left(1 - \tanh^2(\mathbf{z}_t)\right)\mathbf{W}_h $$

The diagonal holds the slopes of tanh, each in $(0, 1]$. The same $\mathbf{W}_h$ appears in every
factor.
:::

::: exercise q1
A sequence has 30 steps and the loss sits at the last one. How many Jacobians multiply together
on the way from $\mathbf{h}_{30}$ back to $\mathbf{h}_1$?

::: answer
29. One for each step from $\mathbf{h}_{t-1}$ to $\mathbf{h}_t$, $t = 2, \ldots, 30$.
:::
:::

::: exercise q2
A scalar RNN has $h_t = \tanh(w\,h_{t-1} + x_t)$. What is $\frac{\partial h_t}{\partial h_{t-1}}$
at a step where $h_t = 0.6$ and $w = 0.8$?

::: answer
$0.512$. It is $(1 - 0.6^2) \cdot 0.8 = 0.64 \cdot 0.8$.
:::
:::

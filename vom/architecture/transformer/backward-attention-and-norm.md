---
title: Gradients through attention and the norm
---

::: card
Attention is a chain of matrix products with a softmax in the middle. For one head,
$\mathbf{S} = \mathbf{Q}\mathbf{K}^T/\sqrt{d_k}$, $\mathbf{A} = \mathrm{softmax}(\mathbf{S})$ by
rows, and $\mathbf{H} = \mathbf{A}\mathbf{V}$. The backward pass undoes these steps from the
last to the first, given $\frac{\partial \mathcal{L}}{\partial \mathbf{H}}$.
:::

::: card
**Step 1, the weighted sum.** $\mathbf{H} = \mathbf{A}\mathbf{V}$ is a product of two matrices,
so each factor gets the upstream gradient times the other one, transposed
([linear layer gradients](reference:linear-layer-gradients)):

$$ \frac{\partial \mathcal{L}}{\partial \mathbf{A}} = \frac{\partial \mathcal{L}}{\partial \mathbf{H}}\mathbf{V}^T, \qquad \frac{\partial \mathcal{L}}{\partial \mathbf{V}} = \mathbf{A}^T\frac{\partial \mathcal{L}}{\partial \mathbf{H}} $$
:::

::: card
**Step 2, the softmax.** Each row is its own softmax. For one row with weights $\mathbf{a}$ and
scores $\mathbf{s}$, the [softmax Jacobian](reference:softmax-jacobian) gives

$$ \frac{\partial \mathcal{L}}{\partial s_j} = a_j\left(\frac{\partial \mathcal{L}}{\partial a_j} - \sum_r a_r\frac{\partial \mathcal{L}}{\partial a_r}\right) $$

The subtracted mean keeps the weights summing to 1: the score gradients of a row always add to
zero.
:::

::: card
**Step 3, the scale.** Dividing by $\sqrt{d_k}$ divides the gradient by $\sqrt{d_k}$ too.
**Step 4, the scores.** $\mathbf{Q}\mathbf{K}^T$ is a product again:

$$ \frac{\partial \mathcal{L}}{\partial \mathbf{Q}} = \frac{\partial \mathcal{L}}{\partial \mathbf{S}'}\mathbf{K}, \qquad \frac{\partial \mathcal{L}}{\partial \mathbf{K}} = \left(\frac{\partial \mathcal{L}}{\partial \mathbf{S}'}\right)^T\mathbf{Q} $$

where $\mathbf{S}' = \mathbf{Q}\mathbf{K}^T$ is the unscaled score matrix.
:::

::: card
**Step 5, the projections.** $\mathbf{Q} = \mathbf{X}\mathbf{W}^Q$ gives
$\frac{\partial \mathcal{L}}{\partial \mathbf{W}^Q} = \mathbf{X}^T\frac{\partial \mathcal{L}}{\partial \mathbf{Q}}$,
and the same for $\mathbf{W}^K$ and $\mathbf{W}^V$. The input $\mathbf{X}$ feeds all three, so its
gradient is a sum of three parts:

$$ \frac{\partial \mathcal{L}}{\partial \mathbf{X}} = \frac{\partial \mathcal{L}}{\partial \mathbf{Q}}(\mathbf{W}^Q)^T + \frac{\partial \mathcal{L}}{\partial \mathbf{K}}(\mathbf{W}^K)^T + \frac{\partial \mathcal{L}}{\partial \mathbf{V}}(\mathbf{W}^V)^T $$
:::

::: card
**Layer norm**, $y_j = \gamma_j\hat{x}_j + \beta_j$. Its parameters get sums over the positions:
$\frac{\partial \mathcal{L}}{\partial \gamma_j} = \sum \frac{\partial \mathcal{L}}{\partial y_j}\hat{x}_j$
and $\frac{\partial \mathcal{L}}{\partial \beta_j} = \sum \frac{\partial \mathcal{L}}{\partial y_j}$.
Each input reaches the output three ways: directly, through $\mu$, and through $\sigma^2$. With
$\mathbf{g} = \boldsymbol{\gamma} \odot \frac{\partial \mathcal{L}}{\partial \mathbf{y}}$:

$$ \frac{\partial \mathcal{L}}{\partial x_j} = \frac{1}{\sqrt{\sigma^2 + \epsilon}}\left(g_j - \frac{1}{d}\sum_k g_k - \frac{\hat{x}_j}{d}\sum_k g_k\hat{x}_k\right) $$
:::

::: card
The three terms match the three paths. The first is the direct effect. The second removes the
part of the gradient that only shifts the whole row, which the mean would cancel. The third
removes the part that only scales the row, which $\sigma$ would cancel. Layer norm passes back
only the changes it does not erase.
:::

::: exercise q1
One row of attention weights is $\mathbf{a} = [0.5, 0.3, 0.2]$, and
$\frac{\partial \mathcal{L}}{\partial \mathbf{a}} = [1, 0, 0]$. What is
$\frac{\partial \mathcal{L}}{\partial \mathbf{s}}$?

::: answer
$[0.25, -0.15, -0.10]$. The weighted mean of the upstream gradient is 0.5.
:::

::: solution
$\sum_r a_r \frac{\partial \mathcal{L}}{\partial a_r} = 0.5 \times 1 = 0.5$.

$j = 1$: $0.5(1 - 0.5) = 0.25$. $j = 2$: $0.3(0 - 0.5) = -0.15$. $j = 3$: $0.2(0 - 0.5) = -0.10$.

The three add to 0. ∎
:::
:::

::: exercise q2
In a self-attention layer, through how many matrices does the gradient reach the input
$\mathbf{X}$, not counting the residual path?

::: answer
Three: through $\mathbf{W}^Q$, $\mathbf{W}^K$ and $\mathbf{W}^V$, and the three parts are added.
:::
:::

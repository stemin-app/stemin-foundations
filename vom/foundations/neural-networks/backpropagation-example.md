---
title: Backpropagation by hand
---

::: card
Run the backward pass on the network you computed forward. The input was $\mathbf{x} = [1.0, 0.5]^T$.
The forward pass gave

$$ \mathbf{z}^{(1)} = [0.5, -0.45, 0.2]^T, \quad \mathbf{h}^{(1)} = [0.5, 0, 0.2]^T, \quad z^{(2)} = 0.2, \quad \hat{y} = \sigma(0.2) \approx 0.55 $$

The true label is $y = 1$, so the output should rise. The loss is $\frac{1}{2}(y - \hat{y})^2$.
:::

::: card
**Step 1, the output error.**

$$ \delta^{(2)} = (\hat{y} - y)\,\hat{y}(1 - \hat{y}) = (0.55 - 1) \cdot 0.55 \cdot 0.45 = -0.45 \cdot 0.2475 \approx -0.111 $$

The sign is negative: raising $z^{(2)}$ lowers the loss.
:::

::: card
**Step 2, the output layer's gradients.** Multiply the error by the layer's input:

$$ \frac{\partial \mathcal{L}}{\partial \mathbf{W}^{(2)}} = \delta^{(2)} \left(\mathbf{h}^{(1)}\right)^T = -0.111 \cdot [0.5, 0, 0.2] = [-0.056, 0, -0.022] $$

and $\frac{\partial \mathcal{L}}{\partial b^{(2)}} = -0.111$. The weight from the silent neuron
$h_2$ gets no gradient: it carried nothing forward.
:::

::: card
**Step 3, back to the hidden layer.** Send the error through the weights
$\mathbf{W}^{(2)} = [0.6, -0.4, 0.5]$:

$$ \frac{\partial \mathcal{L}}{\partial \mathbf{h}^{(1)}} = \left(\mathbf{W}^{(2)}\right)^T \delta^{(2)} = \begin{bmatrix} 0.6 \\ -0.4 \\ 0.5 \end{bmatrix} (-0.111) = \begin{bmatrix} -0.067 \\ 0.044 \\ -0.056 \end{bmatrix} $$
:::

::: card
**Step 4, through the ReLU.** The slope is 1 where $\mathbf{z}^{(1)} > 0$, so the mask for
$[0.5, -0.45, 0.2]^T$ is $[1, 0, 1]^T$:

$$ \boldsymbol{\delta}^{(1)} = [-0.067, 0.044, -0.056]^T \odot [1, 0, 1]^T = [-0.067, 0, -0.056]^T $$

The ReLU stopped the second neuron going forward, and it stops its gradient going back.
:::

::: card
**Step 5, the hidden layer's gradients.**

$$ \frac{\partial \mathcal{L}}{\partial \mathbf{W}^{(1)}} = \boldsymbol{\delta}^{(1)} \mathbf{x}^T = \begin{bmatrix} -0.067 \\ 0 \\ -0.056 \end{bmatrix} [1.0, 0.5] = \begin{bmatrix} -0.067 & -0.033 \\ 0 & 0 \\ -0.056 & -0.028 \end{bmatrix} $$

and $\frac{\partial \mathcal{L}}{\partial \mathbf{b}^{(1)}} = [-0.067, 0, -0.056]^T$. Every gradient
is negative or zero, so a step against them raises the output toward 1.
:::

::: exercise q1
In the example, the label is $y = 0$ instead of 1. What is $\delta^{(2)}$?

::: answer
About $0.136$. $(0.55 - 0) \cdot 0.55 \cdot 0.45$.
:::

::: solution
$\delta^{(2)} = (\hat{y} - y)\,\hat{y}(1 - \hat{y}) = 0.55 \cdot 0.2475 \approx 0.136$. ∎
:::
:::

::: exercise q2
With that $\delta^{(2)} \approx 0.136$, what is $\frac{\partial \mathcal{L}}{\partial \mathbf{W}^{(2)}}$?

::: answer
About $[0.068, 0, 0.027]$. Multiply $\delta^{(2)}$ by $\mathbf{h}^{(1)} = [0.5, 0, 0.2]$.
:::
:::

::: exercise q3
A network has one weight on the path $x \to z = wx \to \hat{y} = z \to \mathcal{L} = \frac{1}{2}(\hat{y} - y)^2$,
with $x = 2$, $w = 3$ and $y = 4$. What is $\frac{\partial \mathcal{L}}{\partial w}$?

::: answer
4. The error is $\hat{y} - y = 6 - 4 = 2$, times the input 2.
:::

::: solution
$\hat{y} = wx = 6$.

$\delta = \frac{\partial \mathcal{L}}{\partial z} = \hat{y} - y = 2$.

$\frac{\partial \mathcal{L}}{\partial w} = \delta \cdot x = 2 \cdot 2 = 4$. ∎
:::
:::

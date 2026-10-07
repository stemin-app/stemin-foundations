---
title: Neural networks
---

Answer every question without notes. Use the natural logarithm.

::: exercise q1
A ReLU neuron has $\mathbf{w} = [1, -2, 0.5]^T$ and $b = 1$. What does it output for
$\mathbf{x} = [2, 1, 4]^T$?

::: answer
3. $z = 2 - 2 + 2 + 1 = 3$, and $\max(0, 3) = 3$.
:::
:::

::: exercise q2
Show that two layers $\mathbf{W}_1 = \begin{bmatrix} 2 & 0 \\ 0 & 1 \end{bmatrix}$ and
$\mathbf{W}_2 = \begin{bmatrix} 1 & 1 \end{bmatrix}$ with no activation equal one layer. Give
its matrix.

::: answer
$\begin{bmatrix} 2 & 1 \end{bmatrix}$, the product $\mathbf{W}_2\mathbf{W}_1$.
:::
:::

::: exercise q3
A network maps $\mathbb{R}^{10}$ to $\mathbb{R}^{3}$ through hidden layers of 20 and 8 neurons.
How many parameters does it have, biases included?

::: answer
415.
:::

::: solution
Layer 1: $20 \times 10 + 20 = 220$.

Layer 2: $8 \times 20 + 8 = 168$.

Layer 3: $3 \times 8 + 3 = 27$.

Total: $220 + 168 + 27 = 415$. ∎
:::
:::

::: exercise q4
Predictions $2.5, 0, 2$ meet true values $3, -0.5, 2$. What is the mean squared error?

::: answer
$1/6 \approx 0.167$.
:::

::: solution
Errors: $-0.5, 0.5, 0$. Squares: $0.25, 0.25, 0$.

Mean: $0.5 / 3 \approx 0.167$. ∎
:::
:::

::: exercise q5
A binary classifier outputs $\hat{y} = 0.25$ for an example with $y = 1$. What is the loss, and
what is $\frac{\partial \mathcal{L}}{\partial z}$ for its sigmoid input $z$?

::: answer
$-\log 0.25 \approx 1.386$, and $\frac{\partial \mathcal{L}}{\partial z} = \hat{y} - y = -0.75$.
:::
:::

::: exercise q6
A sigmoid output with $\mathcal{L} = \frac{1}{2}(y - \hat{y})^2$ has $\hat{y} = 0.8$ and
$y = 1$. Its input from the hidden layer is $\mathbf{h} = [1, 0.5]^T$. Find $\delta$ and
$\frac{\partial \mathcal{L}}{\partial \mathbf{W}}$.

::: answer
$\delta = -0.032$ and $\frac{\partial \mathcal{L}}{\partial \mathbf{W}} = [-0.032, -0.016]$.
:::

::: solution
$\delta = (\hat{y} - y)\,\hat{y}(1 - \hat{y}) = (-0.2)(0.8)(0.2) = -0.032$.

$\frac{\partial \mathcal{L}}{\partial \mathbf{W}} = \delta\,\mathbf{h}^T = [-0.032, -0.016]$. ∎
:::
:::

::: exercise q7
The error at a layer's output is $\boldsymbol{\delta}^{(2)} = [1, -1]^T$, its weights are
$\mathbf{W}^{(2)} = \begin{bmatrix} 2 & 1 \\ 0 & 3 \end{bmatrix}$, and the ReLU layer before it
had $\mathbf{z}^{(1)} = [0.4, -0.2]^T$. What is $\boldsymbol{\delta}^{(1)}$?

::: answer
$[2, 0]^T$.
:::

::: solution
$(\mathbf{W}^{(2)})^T \boldsymbol{\delta}^{(2)} = \begin{bmatrix} 2 & 0 \\ 1 & 3 \end{bmatrix}\begin{bmatrix} 1 \\ -1 \end{bmatrix} = \begin{bmatrix} 2 \\ -2 \end{bmatrix}$.

The ReLU mask for $[0.4, -0.2]^T$ is $[1, 0]^T$.

$\boldsymbol{\delta}^{(1)} = [2, -2]^T \odot [1, 0]^T = [2, 0]^T$. ∎
:::
:::

::: exercise q8
Gradient descent on $f(\theta) = \theta^2$ starts at $\theta_0 = 1$ with $\eta = 0.75$. Give
$\theta_1$, $\theta_2$, and say whether it converges.

::: answer
$\theta_1 = -0.5$, $\theta_2 = 0.25$. It converges, with alternating signs, since
$|1 - 2\eta| = 0.5 < 1$.
:::
:::

::: exercise q9
Adam has $\beta_1 = 0.9$ and $\beta_2 = 0.999$. After the first step with gradient $g = 0.5$,
what are $\hat{m}_1$, $\hat{v}_1$, and the step $\hat{m}_1 / \sqrt{\hat{v}_1}$, ignoring $\epsilon$?

::: answer
$\hat{m}_1 = 0.5$, $\hat{v}_1 = 0.25$, and the ratio is 1.
:::

::: solution
$m_1 = 0.1 \times 0.5 = 0.05$, so $\hat{m}_1 = 0.05 / 0.1 = 0.5$.

$v_1 = 0.001 \times 0.25 = 0.00025$, so $\hat{v}_1 = 0.00025 / 0.001 = 0.25$.

$\hat{m}_1 / \sqrt{\hat{v}_1} = 0.5 / 0.5 = 1$. ∎
:::
:::

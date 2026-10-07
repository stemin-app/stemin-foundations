---
title: Why a neuron bends
---

::: card
Take away the activation and a neuron is linear. Stack two linear layers,
$f(\mathbf{x}) = \mathbf{A}\mathbf{x}$ and then $g(\mathbf{y}) = \mathbf{B}\mathbf{y}$, and you
get one linear layer back:

$$ g(f(\mathbf{x})) = \mathbf{B}\mathbf{A}\mathbf{x} $$

The product $\mathbf{B}\mathbf{A}$ is a single matrix. Ten linear layers are still one matrix,
so the depth buys nothing.[Linear layers collapse](reference:linear-layers-collapse)
:::

::: card
So the activation $\sigma$ must be **nonlinear**. It puts a bend between the layers that no
matrix product can remove.

A counterexample shows the rule: the identity $\sigma(z) = z$ is linear, and a network that uses
it is linear at any depth. Any affine choice $\sigma(z) = az + c$ fails in the same way.
:::

::: card
The **sigmoid** squashes any input into the range $(0, 1)$:[Sigmoid](reference:sigmoid)

$$ \sigma(z) = \frac{1}{1 + e^{-z}} $$

For large positive $z$ it is close to 1, for large negative $z$ close to 0. The change is
smooth, and it is centred at $z = 0$, where $\sigma(0) = 0.5$.
:::

::: card
The **ReLU**, the rectified linear unit, is zero for a negative input and the identity for a
positive one:[ReLU](reference:relu)

$$ \text{ReLU}(z) = \max(0, z) $$

Each piece is linear, but the whole is not: the kink at zero breaks linearity. It costs almost
nothing to compute, and it avoids some training problems of the sigmoid.
:::

::: card
The **tanh** has the shape of the sigmoid, but its outputs lie in $(-1, 1)$ and it is centred
on zero.[Tanh](reference:tanh-activation)

$$ \tanh(z) = \frac{e^{z} - e^{-z}}{e^{z} + e^{-z}} $$

Drag $z$ and read the three outputs side by side.

```plot
x: { var: z, label: "$z$", from: -4, to: 4, ticks: 1, grid: true }
y: { label: "output", from: -1.2, to: 3 }

inputs:
  - { name: z0, min: -3, max: 3, default: 1, step: 0.1, label: "the input z" }

draw:
  - hline: { at: 0 }
  - curve: { is: "max(0, z)", label: "ReLU" }
  - curve: { is: 1 / (1 + exp(-z)), accent: true, label: "sigmoid" }
  - curve: { is: tanh(z), dash: true, label: "tanh" }
  - vline: { at: z0, dash: true }
  - point: { at: [z0, "max(0, z0)"] }
  - point: { at: [z0, 1 / (1 + exp(-z0))] }
  - point: { at: [z0, tanh(z0)] }
```
:::

::: exercise compose-linear-layers
Two layers use the identity as activation and have no bias:
$\mathbf{W}_1 = \begin{bmatrix} 1 & 2 \\ 0 & 1 \end{bmatrix}$, then
$\mathbf{W}_2 = \begin{bmatrix} 1 & 0 \\ 1 & 1 \end{bmatrix}$. Which single matrix does the
same job?

::: answer
$\begin{bmatrix} 1 & 2 \\ 1 & 3 \end{bmatrix}$. The second layer acts last, so the product is
$\mathbf{W}_2 \mathbf{W}_1$.
:::

::: solution
$$ \mathbf{W}_2 \mathbf{W}_1 = \begin{bmatrix} 1 \cdot 1 + 0 \cdot 0 & 1 \cdot 2 + 0 \cdot 1 \\ 1 \cdot 1 + 1 \cdot 0 & 1 \cdot 2 + 1 \cdot 1 \end{bmatrix} = \begin{bmatrix} 1 & 2 \\ 1 & 3 \end{bmatrix} $$

∎
:::
:::

::: exercise relu-vector
Apply the ReLU, element by element, to $\mathbf{z} = [-1.5, 0, 2.3]^T$.

::: answer
$[0, 0, 2.3]^T$. Each negative entry becomes zero; the others pass unchanged.
:::
:::

::: exercise sigmoid-symmetry
What is $\sigma(z) + \sigma(-z)$, for any $z$?

::: answer
1. Put both fractions over a common denominator.
:::

::: solution
$$ \sigma(-z) = \frac{1}{1 + e^{z}} = \frac{e^{-z}}{e^{-z} + 1} $$

$$ \sigma(z) + \sigma(-z) = \frac{1}{1 + e^{-z}} + \frac{e^{-z}}{1 + e^{-z}} = \frac{1 + e^{-z}}{1 + e^{-z}} = 1 $$

∎
:::
:::

::: reference linear-layers-collapse
# Linear layers collapse

Two affine layers with no activation between them equal one affine layer.

::: equation
\mathbf{W}_2(\mathbf{W}_1 \mathbf{x} + \mathbf{b}_1) + \mathbf{b}_2 = (\mathbf{W}_2 \mathbf{W}_1)\mathbf{x} + (\mathbf{W}_2 \mathbf{b}_1 + \mathbf{b}_2)
:::

::: legend
$\mathbf{x}$: the input vector
$\mathbf{W}_1, \mathbf{b}_1$: the weights and the bias of the first layer
$\mathbf{W}_2, \mathbf{b}_2$: the weights and the bias of the second layer
:::

::: derivation
Distribute $\mathbf{W}_2$ over the sum: $\mathbf{W}_2 \mathbf{W}_1 \mathbf{x} + \mathbf{W}_2 \mathbf{b}_1 + \mathbf{b}_2$.
The matrix product is associative, so $\mathbf{W}_2 (\mathbf{W}_1 \mathbf{x}) = (\mathbf{W}_2 \mathbf{W}_1) \mathbf{x}$.[Matrix product](reference:matrix-product)
$\mathbf{W}_2 \mathbf{W}_1$ is one matrix and $\mathbf{W}_2 \mathbf{b}_1 + \mathbf{b}_2$ is one vector, so the result is one affine layer. ∎
:::
:::

::: reference tanh-activation
# Tanh

The hyperbolic tangent maps every real number into $(-1, 1)$, with $\tanh(0) = 0$. It is a
sigmoid stretched to twice the height and shifted down.

::: equation
\tanh(z) = \frac{e^{z} - e^{-z}}{e^{z} + e^{-z}} = 2\sigma(2z) - 1
:::

::: legend
$z$: the input, a real number
$\sigma$: the sigmoid function
:::

::: derivation
Divide the numerator and the denominator by $e^{z}$: $\tanh(z) = \dfrac{1 - e^{-2z}}{1 + e^{-2z}}$.
Write the numerator as $2 - (1 + e^{-2z})$: $\tanh(z) = \dfrac{2}{1 + e^{-2z}} - 1$.
The fraction is $2\sigma(2z)$, so $\tanh(z) = 2\sigma(2z) - 1$.[Sigmoid](reference:sigmoid)
$\sigma$ takes values in $(0, 1)$, so $\tanh$ takes values in $(-1, 1)$. ∎
:::
:::

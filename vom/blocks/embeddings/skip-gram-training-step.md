---
title: One training step by hand
---

::: card
Trace one step of skip-gram with negative sampling. The corpus is the sentence "the cat sat on
the mat", with a window of $c = 1$: one word on each side of the center. The embeddings have 3
dimensions.

With "cat" as the center, the context words are "the" on the left and "sat" on the right. That
gives two training pairs: (cat, the) and (cat, sat). Take the first.
:::

::: card
After random initialization, the center vector of "cat" and the context vector of "the" are

$$
\mathbf{w}_{\text{cat}} = \begin{bmatrix} 0.1 \\ 0.2 \\ -0.1 \end{bmatrix}, \quad
\mathbf{w}'_{\text{the}} = \begin{bmatrix} 0.3 \\ 0.1 \\ 0.2 \end{bmatrix}
$$
:::

::: card
Their dot product says how aligned they are:

$$ \mathbf{w}'_{\text{the}}{}^T \mathbf{w}_{\text{cat}} = 0.3 \times 0.1 + 0.1 \times 0.2 + 0.2 \times (-0.1) = 0.03 + 0.02 - 0.02 = 0.03 $$

The vectors are barely aligned.
:::

::: card
Through the sigmoid, $\sigma(0.03) = \frac{1}{1 + e^{-0.03}} \approx 0.507$. The model gives a
50.7% chance that "the" is a context word of "cat": a coin flip. Yet "the" is a real context
word, so the prediction is poor.
:::

::: card
The loss of a positive pair is $-\ln \sigma(x)$, and its slope is
$\frac{d}{dx}\left(-\ln \sigma(x)\right) = -(1 - \sigma(x)) \approx -0.493$ at $x = 0.03$. The
slope is negative, so gradient descent raises the dot product. A fake pair has the mirror
loss $-\ln \sigma(-x)$.

```plot
x: { var: x, label: "the dot product", from: -4, to: 4, ticks: 1, grid: true }
y: { label: "loss", from: 0, to: 4 }

draw:
  - curve: { is: "log(e(), 1 + exp(-x))", accent: true, label: "real pair" }
  - curve: { is: "log(e(), 1 + exp(x))", dash: true, label: "fake pair" }
  - point: { at: [0.03, 0.678], label: "(cat, the)" }
  - point: { at: [-0.02, 0.683], label: "(cat, algorithm)" }
```
:::

::: card
Now a negative sample. Draw a random word that is not a context of "cat", say "algorithm", with
$\mathbf{w}'_{\text{algorithm}} = [0.5, -0.3, 0.1]^T$:

$$ \mathbf{w}'_{\text{algorithm}}{}^T \mathbf{w}_{\text{cat}} = 0.5 \times 0.1 + (-0.3) \times 0.2 + 0.1 \times (-0.1) = 0.05 - 0.06 - 0.01 = -0.02 $$
:::

::: card
For a negative sample the model should say no with confidence, so the negated dot product goes
into the sigmoid: $\sigma(-(-0.02)) = \sigma(0.02) \approx 0.505$. The model leans only slightly
toward "not a context word". Gradient descent pushes this dot product further negative.
:::

::: card
Repeat this over millions of pairs and a pattern forms. Words that often appear together get
aligned vectors, with high dot products. Words that rarely appear together get perpendicular or
opposed vectors. And words with the same contexts, like "cat" and "dog" near "pet", "fur" and
"fed", end up with similar vectors, because the same context words pull on both.
:::

::: exercise training-step-sigmoid-slope
For the positive pair (cat, the), what is $\frac{d}{dx} \ln \sigma(x)$ at the dot product
$x = 0.03$?

::: answer
About 0.4925. The slope of $\ln \sigma(x)$ is $1 - \sigma(x)$.
:::

::: solution
$$ \frac{d}{dx} \ln \sigma(x) = \frac{1}{\sigma(x)} \cdot \sigma(x)(1 - \sigma(x)) = 1 - \sigma(x) $$

$$ \sigma(0.03) \approx 0.5075 $$

$$ 1 - 0.5075 = 0.4925 $$

∎
:::
:::

::: exercise training-step-gradient
What is the gradient of $\ln \sigma(\mathbf{w}'_{\text{the}}{}^T \mathbf{w}_{\text{cat}})$
with respect to $\mathbf{w}_{\text{cat}}$, to four decimals?

::: answer
$[0.1478, 0.0493, 0.0985]^T$. It is $(1 - \sigma(0.03))\,\mathbf{w}'_{\text{the}}$.
:::

::: solution
By the chain rule, with $x = \mathbf{w}'_{\text{the}}{}^T \mathbf{w}_{\text{cat}}$ and $\frac{\partial x}{\partial \mathbf{w}_{\text{cat}}} = \mathbf{w}'_{\text{the}}$:

$$ \frac{\partial \ln \sigma(x)}{\partial \mathbf{w}_{\text{cat}}} = (1 - \sigma(x))\, \mathbf{w}'_{\text{the}} $$

$$ = 0.4925 \times [0.3, 0.1, 0.2]^T = [0.1478,\ 0.0493,\ 0.0985]^T $$

∎
:::
:::

::: exercise training-step-update
Take one step of gradient ascent on $\mathbf{w}_{\text{cat}}$ alone, with step size 1 and the
gradient $[0.1478, 0.0493, 0.0985]^T$. What is the new dot product with $\mathbf{w}'_{\text{the}}$?

::: answer
About 0.099, up from 0.03. Add the gradient to $\mathbf{w}_{\text{cat}}$, then take the dot product.
:::

::: solution
$$ \mathbf{w}_{\text{cat}} \leftarrow [0.1, 0.2, -0.1]^T + [0.1478, 0.0493, 0.0985]^T = [0.2478,\ 0.2493,\ -0.0015]^T $$

$$ \mathbf{w}'_{\text{the}}{}^T \mathbf{w}_{\text{cat}} = 0.3 \times 0.2478 + 0.1 \times 0.2493 + 0.2 \times (-0.0015) $$

$$ = 0.07434 + 0.02493 - 0.00030 \approx 0.0990 $$

∎
:::
:::

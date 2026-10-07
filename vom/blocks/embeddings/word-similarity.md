---
title: How close two words are
---

::: card
In a good embedding space, similar words have similar vectors. To make "similar vectors"
precise, you measure the [cosine similarity](reference:cosine-similarity) of two embeddings
$\mathbf{u}, \mathbf{v} \in \mathbb{R}^d$:

$$ \text{sim}(\mathbf{u}, \mathbf{v}) = \frac{\mathbf{u}^T \mathbf{v}}{\|\mathbf{u}\| \|\mathbf{v}\|} $$
:::

::: card
The numerator is the dot product, $\mathbf{u}^T \mathbf{v} = \sum_{i=1}^d u_i v_i$. The
denominator multiplies the two lengths, with $\|\mathbf{u}\| = \sqrt{\sum_{i=1}^d u_i^2}$.

Dividing by both lengths removes the size of the vectors. Only their directions count: double
$\mathbf{u}$ and the similarity does not change.
:::

::: card
Geometrically the cosine similarity is $\cos\theta$, where $\theta$ is the angle between the
two vectors. At 0° they point the same way and the similarity is $+1$. At 90° they are
perpendicular and it is 0. At 180° they point in opposite directions and it is $-1$. Drag the
angle and follow the point along the curve.

```plot
x: { var: th, label: "angle $θ$ in degrees", from: 0, to: 180, ticks: 30, grid: true }
y: { label: "similarity", from: -1.1, to: 1.1 }

inputs:
  - { name: a, min: 0, max: 180, default: 40, step: 1, label: "the angle between the vectors" }

draw:
  - hline: { at: 0 }
  - curve: { is: cos(th * pi() / 180), accent: true }
  - point: { at: [0, 1], label: "same direction" }
  - point: { at: [90, 0], label: "perpendicular" }
  - point: { at: [180, -1], label: "opposite" }
  - point: { at: [a, cos(a * pi() / 180)] }
```
:::

::: card
Return to the five-word vocabulary, with $\text{cat} = [0.2, 0.8, -0.1]$ and
$\text{dog} = [0.3, 0.7, -0.2]$. The dot product and the two lengths are

$$
\begin{aligned}
\text{cat}^T \text{dog} &= 0.06 + 0.56 + 0.02 = 0.64 \\
\|\text{cat}\| &= \sqrt{0.04 + 0.64 + 0.01} = \sqrt{0.69} \approx 0.831 \\
\|\text{dog}\| &= \sqrt{0.09 + 0.49 + 0.04} = \sqrt{0.62} \approx 0.787
\end{aligned}
$$
:::

::: card
Divide the dot product by the product of the lengths:

$$ \text{sim}(\text{cat}, \text{dog}) = \frac{0.64}{0.831 \times 0.787} = \frac{0.64}{0.654} \approx 0.98 $$

The two vectors point almost the same way.
:::

::: card
Now compare "cat" with $\text{car} = [-0.5, 0.1, 0.6]$, whose length is
$\sqrt{0.25 + 0.01 + 0.36} = \sqrt{0.62}$:

$$ \text{sim}(\text{cat}, \text{car}) = \frac{-0.1 + 0.08 - 0.06}{\sqrt{0.69}\sqrt{0.62}} = \frac{-0.08}{0.654} \approx -0.12 $$

The similarity is slightly negative. "Cat" and "dog" are much closer to each other than either is
to "car".
:::

::: exercise similarity-car-truck
With $\text{car} = [-0.5, 0.1, 0.6]$ and $\text{truck} = [-0.4, 0.2, 0.5]$, what is
$\text{sim}(\text{car}, \text{truck})$, to two decimals?

::: answer
0.98. The dot product is 0.52 and the lengths are $\sqrt{0.62}$ and $\sqrt{0.45}$.
:::

::: solution
$$ \text{car}^T \text{truck} = 0.20 + 0.02 + 0.30 = 0.52 $$

$$ \|\text{car}\| = \sqrt{0.62} \approx 0.787 \qquad \|\text{truck}\| = \sqrt{0.16 + 0.04 + 0.25} = \sqrt{0.45} \approx 0.671 $$

$$ \text{sim} = \frac{0.52}{0.787 \times 0.671} = \frac{0.52}{0.528} \approx 0.98 $$

∎
:::
:::

::: exercise similarity-cat-fish
With $\text{cat} = [0.2, 0.8, -0.1]$ and $\text{fish} = [0.1, 0.9, 0.3]$, what is
$\text{sim}(\text{cat}, \text{fish})$, to two decimals?

::: answer
0.90. The dot product is 0.71 and the lengths are $\sqrt{0.69}$ and $\sqrt{0.91}$.
:::

::: solution
$$ \text{cat}^T \text{fish} = 0.02 + 0.72 - 0.03 = 0.71 $$

$$ \|\text{cat}\| = \sqrt{0.69} \approx 0.831 \qquad \|\text{fish}\| = \sqrt{0.01 + 0.81 + 0.09} = \sqrt{0.91} \approx 0.954 $$

$$ \text{sim} = \frac{0.71}{0.831 \times 0.954} = \frac{0.71}{0.792} \approx 0.90 $$

∎
:::
:::

::: exercise similarity-scaled
A vector $\mathbf{u}$ is nonzero. What is $\text{sim}(\mathbf{u}, 3\mathbf{u})$?

::: answer
1. Scaling by a positive number keeps the direction, and the lengths cancel.
:::

::: solution
$$ \text{sim}(\mathbf{u}, 3\mathbf{u}) = \frac{3\,\mathbf{u}^T \mathbf{u}}{\|\mathbf{u}\| \cdot 3\|\mathbf{u}\|} = \frac{3\|\mathbf{u}\|^2}{3\|\mathbf{u}\|^2} = 1 $$

∎
:::
:::

::: exercise similarity-angle
Two embeddings have a cosine similarity of 0.5. What is the angle between them?

::: answer
60°. $\cos 60° = 0.5$.
:::
:::

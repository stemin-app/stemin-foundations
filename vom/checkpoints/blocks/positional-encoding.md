---
title: Positional encoding
---

Answer every question without notes. Angles are in radians.

::: exercise q1
Why does self-attention without position information give the word "bit" the same output in "the
dog bit the man" and "the man bit the dog"?

::: answer
Attention scores depend only on content, so it is permutation equivariant: the same tokens get the
same outputs, in any order.
:::
:::

::: exercise q2
Give two reasons why appending the raw index $t$ as one more coordinate fails.

::: answer
Any two of: positions beyond training get values never seen; large $t$ swamps the content values;
the relative step $1/t$ shrinks along the sequence.
:::
:::

::: exercise q3
With $d = 8$, compute $\mathbf{p}_2$ to two decimal places.

::: answer
$(0.91, -0.42, 0.20, 0.98, 0.02, 1.00, 0.00, 1.00)$.
:::

::: solution
The angles are $2, 0.2, 0.02, 0.002$.

$(\sin 2, \cos 2) = (0.91, -0.42)$; $(\sin 0.2, \cos 0.2) = (0.20, 0.98)$;
$(\sin 0.02, \cos 0.02) = (0.02, 1.00)$; $(\sin 0.002, \cos 0.002) = (0.00, 1.00)$. ∎
:::
:::

::: exercise q4
With $d = 32$, what are the frequency and the wavelength of pair 8?

::: answer
$\omega_8 = 0.01$ and $\lambda_8 \approx 628$ positions.
:::

::: solution
$\omega_8 = 10000^{-16/32} = 10000^{-0.5} = 0.01$.

$\lambda_8 = 2\pi / 0.01 \approx 628.3$. ∎
:::
:::

::: exercise q5
What is $\|\mathbf{p}_t\|$ for $d = 128$, at any position?

::: answer
8. The squared length is $d/2 = 64$.
:::
:::

::: exercise q6
One pair has $\omega = 0.25$. Give the rotation matrix that takes position $t$ to position $t + 2$.

::: answer
$\begin{bmatrix} \cos 0.5 & \sin 0.5 \\ -\sin 0.5 & \cos 0.5 \end{bmatrix} \approx \begin{bmatrix} 0.878 & 0.479 \\ -0.479 & 0.878 \end{bmatrix}$.
:::
:::

::: exercise q7
With $d = 4$, $\omega_0 = 1$ and $\omega_1 = 0.01$, does $\mathbf{p}_{10} \cdot \mathbf{p}_{12}$
equal $\mathbf{p}_{50} \cdot \mathbf{p}_{52}$? Give the value.

::: answer
Yes, both are $\cos 2 + \cos 0.02 \approx -0.416 + 1.000 = 0.584$. The dot product depends only on
the offset.
:::
:::

::: exercise q8
A model uses learned positions with $L_{\max} = 4{,}096$ and $d = 2{,}048$. How many parameters do
they hold, and what happens at position 5,000?

::: answer
8,388,608 parameters. Position 5,000 has no learned row.
:::
:::

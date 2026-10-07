---
title: Calculus
---

Answer every question without notes. Use Leibniz notation for every derivative.

::: exercise q1
Find $\displaystyle \lim_{x \to 4} \frac{x^2 - 16}{x - 4}$.

::: answer
8. Factor the top as $(x - 4)(x + 4)$ and cancel.
:::
:::

::: exercise q2
Use the limit definition to find the derivative of $f(x) = 3x^2$ at $x = 1$.

::: answer
6.
:::

::: solution
$\frac{f(1 + h) - f(1)}{h} = \frac{3(1 + 2h + h^2) - 3}{h} = \frac{6h + 3h^2}{h} = 6 + 3h$.

As $h \to 0$, $6 + 3h \to 6$. ∎
:::
:::

::: exercise q3
Find $\frac{dy}{dx}$ for $y = \ln(x^2 + 1)$, and its value at $x = 1$.

::: answer
$\frac{dy}{dx} = \frac{2x}{x^2 + 1}$, which is 1 at $x = 1$.
:::

::: solution
Let $u = x^2 + 1$, so $y = \ln u$.

$\frac{dy}{du} = \frac{1}{u}$ and $\frac{du}{dx} = 2x$.

$\frac{dy}{dx} = \frac{2x}{x^2 + 1}$. At $x = 1$: $\frac{2}{2} = 1$. ∎
:::
:::

::: exercise q4
For $f(x, y) = x^3 y^2$, find $\nabla f$ at $(1, 2)$.

::: answer
$[12, 4]^T$.
:::

::: solution
$\frac{\partial f}{\partial x} = 3x^2 y^2 = 3 \cdot 1 \cdot 4 = 12$.

$\frac{\partial f}{\partial y} = 2x^3 y = 2 \cdot 1 \cdot 2 = 4$. ∎
:::
:::

::: exercise q5
At a point, $\nabla f = [5, -12]^T$. What is the steepest slope, and what is the slope in the
direction $[1, 0]^T$?

::: answer
13 and 5. The steepest slope is $\|\nabla f\|$; the slope along $\mathbf{u}$ is
$\nabla f \cdot \mathbf{u}$.
:::
:::

::: exercise q6
Let $z = y_1^2 + y_2$, with $y_1 = 2x$ and $y_2 = x^3$. Find $\frac{dz}{dx}$ at $x = 1$ by
adding the two paths.

::: answer
11. The paths give $2y_1 \cdot 2 = 8$ and $1 \cdot 3x^2 = 3$.
:::

::: solution
At $x = 1$: $y_1 = 2$.

Path through $y_1$: $\frac{\partial z}{\partial y_1}\frac{dy_1}{dx} = 2y_1 \cdot 2 = 8$.

Path through $y_2$: $\frac{\partial z}{\partial y_2}\frac{dy_2}{dx} = 1 \cdot 3x^2 = 3$.

Sum: 11. Check: $z = 4x^2 + x^3$, so $\frac{dz}{dx} = 8x + 3x^2 = 11$. ∎
:::
:::

::: exercise q7
Find the Jacobian of $\mathbf{y} = [x_1 x_2, \; x_1^2]^T$ at $(2, 3)$.

::: answer
$\begin{bmatrix} 3 & 2 \\ 4 & 0 \end{bmatrix}$.
:::

::: solution
Row 1: $\left[\frac{\partial y_1}{\partial x_1}, \frac{\partial y_1}{\partial x_2}\right] = [x_2, x_1] = [3, 2]$.

Row 2: $\left[\frac{\partial y_2}{\partial x_1}, \frac{\partial y_2}{\partial x_2}\right] = [2x_1, 0] = [4, 0]$. ∎
:::
:::

::: exercise q8
A layer has Jacobian $\mathbf{J} = \begin{bmatrix} 1 & 2 \\ 0 & 1 \\ 3 & 0 \end{bmatrix}$, and the
gradient at its output is $[1, 1, 1]^T$. What is the gradient at its input?

::: answer
$[4, 3]^T$. Multiply by $\mathbf{J}^T$.
:::

::: solution
$\mathbf{J}^T = \begin{bmatrix} 1 & 0 & 3 \\ 2 & 1 & 0 \end{bmatrix}$.

$\mathbf{J}^T [1, 1, 1]^T = [1 + 0 + 3, \; 2 + 1 + 0]^T = [4, 3]^T$. ∎
:::
:::

::: exercise q9
$\sigma(x) = 0.2$ at some $x$. What is $\frac{d\sigma}{dx}$ there?

::: answer
$0.16$. The slope is $0.2 \times 0.8$.
:::
:::

::: exercise q10
With $\mathbf{p} = \mathrm{softmax}(\mathbf{z}) = [0.6, 0.4]$, find the Jacobian
$\frac{\partial p_i}{\partial z_j}$.

::: answer
$\begin{bmatrix} 0.24 & -0.24 \\ -0.24 & 0.24 \end{bmatrix}$.
:::

::: solution
Diagonal: $p_1(1 - p_1) = 0.6 \times 0.4 = 0.24$, and $p_2(1 - p_2) = 0.4 \times 0.6 = 0.24$.

Off the diagonal: $-p_1 p_2 = -0.24$. ∎
:::
:::

---
title: Approaching a value
---

::: card
Take the function

$$ f(x) = \frac{x^2 - 1}{x - 1} $$

and try $x = 1$. The top is $0$ and the bottom is $0$, and $\frac{0}{0}$ has no value. The
function is undefined at $x = 1$. It is defined everywhere near it, though, so you can ask what it
does as $x$ comes close.
:::

::: card
Walk in from both sides. At $x = 0.9$, $f = 1.9$. At $0.99$, $f = 1.99$. At $0.999$, $f = 1.999$.
From above, $1.1$ gives $2.1$, $1.01$ gives $2.01$, and $1.001$ gives $2.001$. Shrink the distance
$d$ and the two values close in on 2 from either side.

```plot
x: { var: x, label: "$x$", from: -0.5, to: 2.5, ticks: 0.5, grid: true }
y: { label: "$f(x)$", from: 0, to: 3.5 }

inputs:
  - { name: d, min: 0.02, max: 1, default: 0.5, step: 0.02, label: "the distance d from 1" }

draw:
  - curve: { is: (x^2 - 1) / (x - 1), over: [-0.5, 0.97] }
  - curve: { is: (x^2 - 1) / (x - 1), over: [1.03, 2.5] }
  - hline: { at: 2, dash: true }
  - vline: { at: 1, dash: true }
  - point: { at: [1 - d, 2 - d], label: "$f(1 - d)$" }
  - point: { at: [1 + d, 2 + d], label: "$f(1 + d)$" }
  - point: { at: [1, 2], accent: true, label: "the hole" }
```
:::

::: card
You write this behaviour as a **limit**:

$$ \lim_{x \to 1} \frac{x^2 - 1}{x - 1} = 2 $$

Read it as: as $x$ approaches 1, $f(x)$ approaches 2. The function never equals 2 at $x = 1$,
because it has no value there. It comes as close to 2 as you like, provided $x$ is close enough
to 1.
[Limit](reference:limit-of-a-function)
:::

::: card
Algebra confirms the numbers. Factor the top:

$$ \frac{x^2 - 1}{x - 1} = \frac{(x - 1)(x + 1)}{x - 1} = x + 1 \qquad (x \neq 1) $$

Away from $x = 1$, the function is the straight line $x + 1$ with one point missing. As
$x \to 1$, $x + 1 \to 2$.
:::

::: card
A limit can fail to exist. The function $\frac{|x|}{x}$ equals $-1$ for every negative $x$ and
$+1$ for every positive $x$. Approach $0$ from the left and you see $-1$; approach from the right
and you see $+1$. No single number is approached from both sides, so
$\lim_{x \to 0} \frac{|x|}{x}$ does not exist.

```plot
x: { var: x, label: "$x$", from: -2, to: 2, ticks: 0.5, grid: true }
y: { label: "$|x| / x$", from: -1.5, to: 1.5 }

draw:
  - curve: { is: -1, over: [-2, -0.03] }
  - curve: { is: 1, over: [0.03, 2], accent: true }
  - point: { at: [0, -1], label: "$-1$ from the left" }
  - point: { at: [0, 1], label: "$+1$ from the right" }
```
:::

::: card
Limits matter because some quantities cannot be computed directly but can be defined as a limit.
The most important one is the rate at which a function changes at one exact point. It comes out
as $\frac{0}{0}$ if you compute it directly, just like $f(1)$ above, and a limit gives it a value.
That rate is the derivative.
:::

::: exercise limit-difference-of-squares
Find $\displaystyle \lim_{x \to 2} \frac{x^2 - 4}{x - 2}$.

::: answer
$4$. Factor the top and cancel $x - 2$.
:::

::: solution
$$ \frac{x^2 - 4}{x - 2} = \frac{(x - 2)(x + 2)}{x - 2} = x + 2 \qquad (x \neq 2) $$

$$ \lim_{x \to 2} (x + 2) = 4 $$

∎
:::
:::

::: exercise limit-negative-point
Find $\displaystyle \lim_{x \to -3} \frac{x^2 - 9}{x + 3}$.

::: answer
$-6$. The function is $x - 3$ away from $x = -3$.
:::

::: solution
$$ \frac{x^2 - 9}{x + 3} = \frac{(x + 3)(x - 3)}{x + 3} = x - 3 \qquad (x \neq -3) $$

$$ \lim_{x \to -3} (x - 3) = -6 $$

∎
:::
:::

::: exercise limit-cubic
Find $\displaystyle \lim_{x \to 1} \frac{x^3 - 1}{x - 1}$.

::: answer
$3$. Use $x^3 - 1 = (x - 1)(x^2 + x + 1)$.
:::

::: solution
$$ \frac{x^3 - 1}{x - 1} = x^2 + x + 1 \qquad (x \neq 1) $$

$$ \lim_{x \to 1} (x^2 + x + 1) = 1 + 1 + 1 = 3 $$

∎
:::
:::

::: exercise limit-sign-function
Does $\displaystyle \lim_{x \to 0} \frac{|x|}{x}$ exist?

::: answer
No. The left side gives $-1$ and the right side gives $+1$.
:::
:::

::: reference limit-of-a-function
# Limit of a function

The limit of $f(x)$ as $x$ approaches $a$ is $L$ when $f(x)$ comes as close to $L$ as you like for
every $x$ close enough to $a$, with $x \neq a$. The value $f(a)$ plays no part, and may not exist.
The limit exists only when both sides approach the same $L$.

::: equation
\lim_{x \to a} f(x) = L
:::

::: legend
$f$: the function
$a$: the point that $x$ approaches
$L$: the value that $f(x)$ approaches
:::
:::

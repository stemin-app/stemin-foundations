---
title: Rules for derivatives
---

::: card
You rarely go back to the limit. A handful of derivatives cover most functions you meet, and you
learn them once.
[Standard derivatives](reference:standard-derivatives)

$$ \frac{d}{dx} x^n = n x^{n-1} \qquad \frac{d}{dx} e^x = e^x \qquad \frac{d}{dx} \ln x = \frac{1}{x} $$

$$ \frac{d}{dx} \sin x = \cos x \qquad \frac{d}{dx} \cos x = -\sin x $$
:::

::: card
The derivative is a function of its own: at each $x$ it gives the slope there. Slide the point
along $\sin x$. The tangent's slope is always the height of the dashed $\cos x$ at the same $x$.
At $a = 0$ the slope is $1$; at $a = \frac{\pi}{2} \approx 1.57$ the tangent is flat and
$\cos a = 0$.

```plot
x: { var: x, label: "$x$", from: -3.5, to: 3.5, ticks: 1, grid: true }
y: { label: "$y$", from: -1.6, to: 1.6 }

inputs:
  - { name: a, min: -3, max: 3, default: 0.8, step: 0.05, label: "the point a" }

draw:
  - curve: { is: sin(x), label: "$sin x$" }
  - curve: { is: cos(x), dash: true, label: "$cos x$" }
  - curve: { is: sin(a) + cos(a) * (x - a), accent: true, over: [a - 1, a + 1] }
  - point: { at: [a, sin(a)], label: "$(a, sin a)$" }
  - point: { at: [a, cos(a)], label: "the slope" }
```
:::

::: card
The exponential $e^x$ is the one function equal to its own derivative. At every point its slope
equals its height: $1$ at $x = 0$, about $2.72$ at $x = 1$. The logarithm $\ln x$ goes the other
way: its slope $\frac{1}{x}$ shrinks as $x$ grows, so it climbs ever more slowly.
:::

::: card
For a product $y = f(x) \cdot g(x)$, the **product rule** gives

$$ \frac{dy}{dx} = \frac{df}{dx} \cdot g(x) + f(x) \cdot \frac{dg}{dx} $$

[Product rule](reference:product-rule)
:::

::: card
Both factors depend on $x$, so a change in $x$ reaches $y$ along two paths. The first term is the
effect of $f$ changing while $g$ stays fixed. The second is the effect of $g$ changing while $f$
stays fixed. The two effects add.
:::

::: card
For a quotient $y = \frac{f(x)}{g(x)}$, with $g(x) \neq 0$, the **quotient rule** gives

$$ \frac{dy}{dx} = \frac{\frac{df}{dx} \cdot g(x) - f(x) \cdot \frac{dg}{dx}}{g(x)^2} $$

[Quotient rule](reference:quotient-rule)
:::

::: exercise power-rule-quintic
What is $\frac{d}{dx} x^5$ at $x = 2$?

::: answer
$80$. The derivative is $5x^4$.
:::

::: solution
$$ \frac{d}{dx} x^5 = 5x^4 = 5 \cdot 16 = 80 $$

∎
:::
:::

::: exercise product-rule-x-exp
What is the derivative of $y = x e^x$ at $x = 0$?

::: answer
$1$. The product rule gives $e^x + x e^x$.
:::

::: solution
$$ \frac{dy}{dx} = 1 \cdot e^x + x \cdot e^x = e^x (1 + x) $$

$$ e^0 (1 + 0) = 1 $$

∎
:::
:::

::: exercise quotient-rule-x-over-x-plus-one
What is the derivative of $y = \frac{x}{x + 1}$ at $x = 1$?

::: answer
$\frac{1}{4}$. The quotient rule gives $\frac{1}{(x + 1)^2}$.
:::

::: solution
$$ \frac{dy}{dx} = \frac{1 \cdot (x + 1) - x \cdot 1}{(x + 1)^2} = \frac{1}{(x + 1)^2} $$

$$ \frac{1}{(1 + 1)^2} = \frac{1}{4} $$

∎
:::
:::

::: reference standard-derivatives
# Standard derivatives

The derivatives of the power, the exponential, the natural logarithm, the sine and the cosine.

::: equation
\frac{d}{dx} x^n = n x^{n-1} \qquad \frac{d}{dx} e^x = e^x \qquad \frac{d}{dx} \ln x = \frac{1}{x} \qquad \frac{d}{dx} \sin x = \cos x \qquad \frac{d}{dx} \cos x = -\sin x
:::

::: legend
$x$: the input, positive for the logarithm
$n$: a constant exponent
$e$: the base of the natural logarithm, about 2.718
:::

::: derivation
For $x^n$ with $n$ a positive integer, expand by the binomial theorem: $(x + h)^n = x^n + n x^{n-1} h + (\text{terms with } h^2 \text{ or higher})$.
Subtract $x^n$ and divide by $h$: $n x^{n-1} + (\text{terms with } h \text{ or higher})$.
Let $h \to 0$: $n x^{n-1}$.[Derivative](reference:derivative)
For $e^x$: $\frac{e^{x + h} - e^x}{h} = e^x \cdot \frac{e^h - 1}{h}$, and $\frac{e^h - 1}{h} \to 1$, which is what defines the base $e$.
For $\ln x$: $y = \ln x$ means $x = e^y$, so $\frac{dx}{dy} = e^y = x$ and $\frac{dy}{dx} = \frac{1}{x}$. ∎
:::
:::

::: reference product-rule
# Product rule

The derivative of a product is the derivative of each factor times the other factor, summed.

::: equation
\frac{d}{dx}\big(f(x)\, g(x)\big) = \frac{df}{dx}\, g(x) + f(x)\, \frac{dg}{dx}
:::

::: legend
$f, g$: two differentiable functions of the same input
:::

::: derivation
Split the change: $f(x + h) g(x + h) - f(x) g(x) = \big(f(x + h) - f(x)\big) g(x + h) + f(x) \big(g(x + h) - g(x)\big)$.
Divide by $h$.
Let $h \to 0$: the first quotient tends to $\frac{df}{dx}$, $g(x + h)$ tends to $g(x)$, and the second quotient tends to $\frac{dg}{dx}$.[Derivative](reference:derivative)
Result: $\frac{df}{dx} g(x) + f(x) \frac{dg}{dx}$. ∎
:::
:::

::: reference quotient-rule
# Quotient rule

The derivative of a quotient, wherever the denominator is not zero.

::: equation
\frac{d}{dx}\left(\frac{f(x)}{g(x)}\right) = \frac{\frac{df}{dx}\, g(x) - f(x)\, \frac{dg}{dx}}{g(x)^2}
:::

::: legend
$f$: the numerator, a differentiable function
$g$: the denominator, a differentiable function that is not zero at the point
:::

::: derivation
Let $y = \frac{f}{g}$, so $f = y\, g$.
Differentiate both sides: $\frac{df}{dx} = \frac{dy}{dx}\, g + y\, \frac{dg}{dx}$.[Product rule](reference:product-rule)
Solve for $\frac{dy}{dx}$: $\frac{dy}{dx} = \frac{1}{g}\left(\frac{df}{dx} - \frac{f}{g} \frac{dg}{dx}\right)$.
Multiply top and bottom by $g$: $\frac{dy}{dx} = \frac{\frac{df}{dx}\, g - f\, \frac{dg}{dx}}{g^2}$. ∎
:::
:::

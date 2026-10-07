---
title: Multi-head attention
---

Answer every question without notes. Leave out biases unless a question names them.

::: exercise q1
A model has $d_\text{model} = 1024$ and $h = 16$ heads. What is the shape of the key matrix
$\mathbf{W}_i^K$ of one head?

::: answer
$1024 \times 64$. Each head has width $d_k = 1024 / 16 = 64$.
:::

::: solution
$$ d_k = \frac{1024}{16} = 64 $$

$\mathbf{W}_i^K$ maps a 1024-wide row to a 64-wide row, so it is $1024 \times 64$. ∎
:::
:::

::: exercise q2
A layer with $h = 16$ heads reads a sequence of $n = 128$ tokens. How many attention weights does
it compute over all its heads?

::: answer
262,144. Each head computes an $n \times n$ table.
:::

::: solution
$$ 128 \times 128 = 16{,}384 $$

$$ 16 \times 16{,}384 = 262{,}144 $$

∎
:::
:::

::: exercise q3
How many parameters does a multi-head attention layer with $d_\text{model} = 1024$ and $h = 16$
heads hold?

::: answer
4,194,304. The count is $4\, d_\text{model}^2$ for any number of heads.
:::

::: solution
$$ 16 \times 3 \times 1024 \times 64 = 3{,}145{,}728 $$

$$ 1024^2 = 1{,}048{,}576 $$

$$ 3{,}145{,}728 + 1{,}048{,}576 = 4{,}194{,}304 $$

∎
:::
:::

::: exercise q4
A layer has heads of width $d_k = 64$. Which head owns coordinate 700 of the concatenated vector,
and which coordinates does that head own?

::: answer
Head 11, which owns coordinates 641 to 704.
:::

::: solution
$$ \left\lceil \frac{700}{64} \right\rceil = \lceil 10.94 \rceil = 11 $$

Head 11 starts at $10 \times 64 + 1 = 641$ and ends at $11 \times 64 = 704$. ∎
:::
:::

::: exercise q5
Two heads of width 2 give $\mathbf{v}_1 = (1, -1)$ and $\mathbf{v}_2 = (2, 0)$ for one token. The
output matrix is

$$ \mathbf{W}^O = \begin{pmatrix} 1 & 0 \\ 0 & 1 \\ 1 & 1 \\ 0 & -1 \end{pmatrix} $$

What is the output $\mathbf{z}$?

::: answer
$\mathbf{z} = (3, 1)$. Concatenate to $(1, -1, 2, 0)$, then multiply by $\mathbf{W}^O$.
:::

::: solution
$$ \mathbf{c} = (1, -1, 2, 0) $$

$$ z_1 = 1 \times 1 + (-1) \times 0 + 2 \times 1 + 0 \times 0 = 3 $$

$$ z_2 = 1 \times 0 + (-1) \times 1 + 2 \times 1 + 0 \times (-1) = 1 $$

∎
:::
:::

::: exercise q6
One head weighs two tokens with scores $s_1$ and $s_2$. What gap $s_1 - s_2$ gives the first
token a weight of 0.75?

::: answer
$\ln 3 \approx 1.10$. Solve $1/(1 + e^{-\Delta}) = 0.75$.
:::

::: solution
$$ 1 + e^{-\Delta} = \frac{1}{0.75} = \frac{4}{3} $$

$$ e^{-\Delta} = \frac{1}{3} $$

$$ \Delta = \ln 3 = 1.099 $$

∎
:::
:::

::: exercise q7
A layer with $d_\text{model} = 1024$ has 16 heads. How many times more parameters does it hold if
every head is 1024 wide, in place of 64 wide?

::: answer
16 times. Full-width heads cost $4h\, d_\text{model}^2$; narrow heads cost $4\, d_\text{model}^2$.
:::

::: solution
$$ \frac{4 \times 16 \times 1024^2}{4 \times 1024^2} = 16 $$

∎
:::
:::

::: exercise q8
A model has $d_\text{model} = 1024$ and $h = 8$ heads. By what number does each head divide its
scores, to two decimal places?

::: answer
11.31. Each head has $d_k = 128$, and $\sqrt{128} \approx 11.31$.
:::

::: solution
$$ d_k = \frac{1024}{8} = 128 $$

$$ \sqrt{128} = 8\sqrt{2} = 11.31 $$

∎
:::
:::

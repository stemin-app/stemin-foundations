---
title: The transformer
---

Answer every question without notes. Use the natural logarithm.

::: exercise q1
In the encoder-decoder transformer, where do the queries, the keys and the values of
cross-attention come from?

::: answer
The queries come from the decoder; the keys and the values come from the final encoder output.
:::
:::

::: exercise q2
A source of $n = 12$ tokens and a target of $m = 9$ tokens pass through a model with $h = 8$ heads.
Give the shape of one head's weights in encoder self-attention, in decoder masked self-attention,
and in cross-attention.

::: answer
$12 \times 12$, $9 \times 9$ and $9 \times 12$.
:::
:::

::: exercise q3
The model gives the true tokens of four positions the probabilities $0.5$, $0.5$, $0.25$ and $1$.
What is the loss?

::: answer
$\ln 2 \approx 0.693$.
:::

::: solution
$\mathcal{L} = \tfrac{1}{4}(\ln 2 + \ln 2 + \ln 4 + 0) = \tfrac{1}{4}(4\ln 2) = \ln 2$. ∎
:::
:::

::: exercise q4
At one position, a vocabulary of 4 tokens gets the probabilities $[0.1, 0.6, 0.2, 0.1]$, and the
true token is the third. With $m = 1$, what is the gradient on the four logits?

::: answer
$[0.1, 0.6, -0.8, 0.1]$. Subtract the one-hot target.
:::
:::

::: exercise q5
A linear layer has input rows $\mathbf{X} = \begin{bmatrix} 1 & 0 \\ 0 & 2 \end{bmatrix}$ and
upstream gradient $\frac{\partial \mathcal{L}}{\partial \mathbf{Y}} = \begin{bmatrix} 1 & 1 \\ 2 & 0 \end{bmatrix}$.
Find $\frac{\partial \mathcal{L}}{\partial \mathbf{W}}$ and $\frac{\partial \mathcal{L}}{\partial \mathbf{b}}$.

::: answer
$\frac{\partial \mathcal{L}}{\partial \mathbf{W}} = \begin{bmatrix} 1 & 1 \\ 4 & 0 \end{bmatrix}$ and
$\frac{\partial \mathcal{L}}{\partial \mathbf{b}} = [3, 1]$.
:::

::: solution
$\mathbf{X}^T\frac{\partial \mathcal{L}}{\partial \mathbf{Y}} = \begin{bmatrix} 1 & 0 \\ 0 & 2 \end{bmatrix}\begin{bmatrix} 1 & 1 \\ 2 & 0 \end{bmatrix} = \begin{bmatrix} 1 & 1 \\ 4 & 0 \end{bmatrix}$.

The bias gradient sums the rows: $[1 + 2, 1 + 0] = [3, 1]$. ∎
:::
:::

::: exercise q6
A row of attention weights is $[0.25, 0.75]$ and the upstream gradient on them is $[2, 0]$. What
is the gradient on the two scores?

::: answer
$[0.375, -0.375]$.
:::

::: solution
Weighted mean: $0.25 \times 2 + 0.75 \times 0 = 0.5$.

$0.25(2 - 0.5) = 0.375$ and $0.75(0 - 0.5) = -0.375$. ∎
:::
:::

::: exercise q7
With $d_{model} = 1{,}024$ and a warmup of $w = 4{,}000$ steps, what is the peak learning rate of
the original schedule?

::: answer
About $4.9 \times 10^{-4}$. The peak is $(d_{model} \cdot w)^{-0.5} = (4{,}096{,}000)^{-0.5}$.
:::
:::

::: exercise q8
Estimate the weights of a decoder-only model with $d_{model} = 2{,}048$ and $N = 24$, embeddings
left out.

::: answer
About 1.2 billion. $12 \times 24 \times 2{,}048^2 \approx 1.21 \times 10^9$.
:::
:::

::: exercise q9
Which family uses bidirectional attention with no mask, GPT or BERT, and which kind of task suits
it?

::: answer
BERT. It suits tasks that read a whole text, such as classification or labelling each token.
:::
:::

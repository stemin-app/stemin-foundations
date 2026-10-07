---
title: Attention routes information
---

::: card
Read attention as routing. Every position can read from every other one, and the weight
$\alpha_{ij}$ sets how much flows from position $j$ to position $i$. When $\alpha_{ij}$ is high,
the output at $i$ strongly reflects the value at $j$.
:::

::: card
The routes depend on the content. Unlike an RNN, where information moves one step at a time,
attention links any two positions directly. A token at position 1 can shape position 100 in one
step, if the weights say it matters.
:::

::: card
The price is quadratic. With $n$ positions there are $n^2$ scores, one per pair of query and key,
and the work grows as $n^2 d$. Double the sequence and the cost of attention quadruples. Many
"efficient attention" variants exist to cut this cost.
:::

::: card
Trained models show typical patterns in their weight matrices ([Figure](figure:attention-patterns)).
A **diagonal** means each token attends to itself and its neighbours: local context. **Vertical
stripes** mean many tokens attend to one position, often the first token or a key word such as
the verb. **Blocks** mean groups of tokens attend within their group, following phrases or
clauses. Most weights are near zero: attention is sparse.
:::

::: figure attention-patterns
![Three patterns of attention weights](assets/attention-patterns.svg)

Each grid is a matrix of weights: a row per query position, a column per key position. A darker
cell is a larger weight.
:::

::: exercise q1
A sequence grows from 1,000 to 4,000 tokens. By what factor does the number of attention scores
grow?

::: answer
16. The count is $n^2$, and $(4{,}000 / 1{,}000)^2 = 16$.
:::
:::

::: exercise q2
In a weight matrix, column 1 is dark in every row. What does that mean?

::: answer
Every position attends strongly to position 1: a vertical stripe.
:::
:::

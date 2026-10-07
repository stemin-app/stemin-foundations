---
title: Queries, keys and values
---

::: card
Attention works like a soft lookup in a table. Three roles take part:

- the **query** is what the current position looks for, its question to the sequence;
- each **key** labels what a position offers;
- each **value** is the content a position hands over.

The query is compared with every key. The matches decide the weights, and the weights mix the
values.
:::

::: card
In vectors: one query $\mathbf{q} \in \mathbb{R}^d$, keys $\mathbf{k}_1, \ldots, \mathbf{k}_n \in \mathbb{R}^d$,
and values $\mathbf{v}_1, \ldots, \mathbf{v}_n \in \mathbb{R}^{d_v}$. Queries and keys share the
size $d$, because they meet in a dot product. Values may have another size $d_v$.
:::

::: card
A **score function** $s(\mathbf{q}, \mathbf{k})$ measures each match. A softmax turns the scores
into weights, and the weights mix the values ([attention weights](reference:attention-weights)):

$$ \alpha_i = \frac{\exp\left(s(\mathbf{q}, \mathbf{k}_i)\right)}{\sum_{j=1}^{n} \exp\left(s(\mathbf{q}, \mathbf{k}_j)\right)}, \qquad \mathbf{o} = \sum_{i=1}^{n} \alpha_i \mathbf{v}_i $$
:::

::: card
A real lookup is hard: a query either matches a key exactly or gets nothing. Attention is soft:
every key matches a little, and the best matches dominate. Because every step is smooth,
gradients flow through the lookup, and training can learn what to ask and what to offer.
:::

::: exercise q1
Queries have size 64 and values have size 32. What size must the keys have, and what size is
the output?

::: answer
The keys have size 64, to meet the queries in a dot product. The output has size 32, a mix of
values.
:::
:::

::: exercise q2
Two keys score 2 and 0 against a query. What are the two attention weights?

::: answer
About $0.881$ and $0.119$. They are $e^2 / (e^2 + 1)$ and $1 / (e^2 + 1)$.
:::
:::

::: reference attention-weights
# Attention weights

The attention weights are a softmax over the scores of one query against every key. The output
is the values mixed with those weights.

::: equation
\alpha_i = \frac{\exp\left(s(\mathbf{q}, \mathbf{k}_i)\right)}{\sum_{j=1}^{n} \exp\left(s(\mathbf{q}, \mathbf{k}_j)\right)} \qquad \mathbf{o} = \sum_{i=1}^{n} \alpha_i \mathbf{v}_i
:::

::: legend
$\mathbf{q}$: the query
$\mathbf{k}_i$: the key of position $i$
$\mathbf{v}_i$: the value of position $i$
$s$: the score function, usually the scaled dot product
$\alpha_i$: the weight of position $i$; the weights are positive and add to 1
$\mathbf{o}$: the output
:::

::: derivation
The weights are a softmax of the scores, so each is positive and they add to 1.[softmax](reference:softmax)
So $\mathbf{o}$ is a weighted average of the values, and lies inside the region they span with positive weights. ∎
:::
:::

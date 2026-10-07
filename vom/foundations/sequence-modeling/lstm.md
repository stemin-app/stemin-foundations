---
title: The LSTM cell
---

::: card
The **long short-term memory** network, or LSTM, adds a second state beside $\mathbf{h}_t$: the
**cell state** $\mathbf{c}_t$. It is a memory lane that information can travel along almost
unchanged, step after step. Three **gates** decide what enters it, what leaves it and what it
shows.
:::

::: card
Each gate is a sigmoid layer over the previous state and the input, joined into one vector
$[\mathbf{h}_{t-1}, \mathbf{x}_t]$. Its outputs lie in $(0, 1)$ and act as soft switches:

$$ \mathbf{f}_t = \sigma\left(\mathbf{W}_f[\mathbf{h}_{t-1}, \mathbf{x}_t] + \mathbf{b}_f\right), \quad \mathbf{i}_t = \sigma\left(\mathbf{W}_i[\mathbf{h}_{t-1}, \mathbf{x}_t] + \mathbf{b}_i\right), \quad \mathbf{o}_t = \sigma\left(\mathbf{W}_o[\mathbf{h}_{t-1}, \mathbf{x}_t] + \mathbf{b}_o\right) $$

The **forget** gate $\mathbf{f}_t$ chooses what to erase, the **input** gate $\mathbf{i}_t$ what to
write, the **output** gate $\mathbf{o}_t$ what to show.
:::

::: card
A tanh layer proposes new content, and the cell mixes it with the old:

$$ \tilde{\mathbf{c}}_t = \tanh\left(\mathbf{W}_c[\mathbf{h}_{t-1}, \mathbf{x}_t] + \mathbf{b}_c\right), \qquad \mathbf{c}_t = \mathbf{f}_t \odot \mathbf{c}_{t-1} + \mathbf{i}_t \odot \tilde{\mathbf{c}}_t $$

The hidden state is a gated view of the cell: $\mathbf{h}_t = \mathbf{o}_t \odot \tanh(\mathbf{c}_t)$
([LSTM cell](reference:lstm-cell)).
:::

::: card
The cell update is the key. With the forget gate near 1 and the input gate near 0, the cell is
copied: $\mathbf{c}_t \approx \mathbf{c}_{t-1}$. The old content is kept by addition, not pushed
through a matrix and a squash. A fact written at step 1 can wait in the cell until step 40.
:::

::: exercise q1
One cell entry has $c_{t-1} = 2$, $f_t = 0.9$, $i_t = 0.1$ and $\tilde{c}_t = -1$. What is $c_t$?

::: answer
1.7. $0.9 \cdot 2 + 0.1 \cdot (-1) = 1.8 - 0.1$.
:::
:::

::: exercise q2
With $c_t = 1.7$ and $o_t = 0.5$, what is $h_t$?

::: answer
About $0.468$. $0.5 \cdot \tanh(1.7) = 0.5 \cdot 0.935$.
:::
:::

::: exercise q3
An LSTM has hidden size $d$ and input size $n$. How many weights do its four layers
$\mathbf{W}_f, \mathbf{W}_i, \mathbf{W}_o, \mathbf{W}_c$ hold, biases left out?

::: answer
$4d(d + n)$. Each layer maps the joined vector of size $d + n$ to size $d$.
:::
:::

::: reference lstm-cell
# LSTM cell

An LSTM keeps a cell state that it updates by addition, under three sigmoid gates: forget, input
and output.

::: equation
\begin{aligned}
\mathbf{f}_t &= \sigma(\mathbf{W}_f[\mathbf{h}_{t-1}, \mathbf{x}_t] + \mathbf{b}_f), \quad \mathbf{i}_t = \sigma(\mathbf{W}_i[\mathbf{h}_{t-1}, \mathbf{x}_t] + \mathbf{b}_i), \quad \mathbf{o}_t = \sigma(\mathbf{W}_o[\mathbf{h}_{t-1}, \mathbf{x}_t] + \mathbf{b}_o) \\
\tilde{\mathbf{c}}_t &= \tanh(\mathbf{W}_c[\mathbf{h}_{t-1}, \mathbf{x}_t] + \mathbf{b}_c) \\
\mathbf{c}_t &= \mathbf{f}_t \odot \mathbf{c}_{t-1} + \mathbf{i}_t \odot \tilde{\mathbf{c}}_t, \qquad \mathbf{h}_t = \mathbf{o}_t \odot \tanh(\mathbf{c}_t)
\end{aligned}
:::

::: legend
$\mathbf{c}_t$: the cell state
$\mathbf{h}_t$: the hidden state
$\mathbf{f}_t, \mathbf{i}_t, \mathbf{o}_t$: the forget, input and output gates, entries in $(0, 1)$
$\tilde{\mathbf{c}}_t$: the proposed new content
$[\mathbf{h}_{t-1}, \mathbf{x}_t]$: the previous state and the input, joined into one vector
$\odot$: the product entry by entry
:::

::: derivation
The cell update is $\mathbf{c}_t = \mathbf{f}_t \odot \mathbf{c}_{t-1} + \mathbf{i}_t \odot \tilde{\mathbf{c}}_t$.
Hold the gates and the proposal fixed and differentiate by $\mathbf{c}_{t-1}$: entry $j$ of $\mathbf{c}_t$ depends only on entry $j$ of $\mathbf{c}_{t-1}$, with slope $f_{t,j}$.[Jacobian](reference:jacobian)
So the direct Jacobian along the cell is $\mathrm{diag}(\mathbf{f}_t)$, with no weight matrix in it. ∎
:::
:::

---
title: Why depth matters
---

::: card
One hidden layer is already enough in principle. The **universal approximation theorem** says
that a network with a single hidden layer and enough neurons approximates any continuous function
on a bounded region as closely as you like.

The catch is the word "enough". For some functions, enough means exponentially many neurons. A
deeper network can represent the same functions with far fewer.
:::

::: card
The **parity** function shows the gap. It takes $n$ bits and outputs 1 if an odd number of them
are 1, and 0 otherwise. For example, the parity of $(1, 0, 1)$ is 0, and the parity of
$(1, 1, 1)$ is 1.

Flip any single bit and the output flips. No input matters less than another, and no simple
boundary separates the 1s from the 0s.
:::

::: card
The parity of two bits is XOR, and two ReLU neurons compute it:[XOR from two ReLUs](reference:xor-from-relu)

$$ \text{XOR}(a, b) = \max(0, a + b) - 2\max(0, a + b - 1) $$

Check the four cases: $(0, 0)$ gives $0 - 0 = 0$; $(1, 0)$ and $(0, 1)$ give $1 - 0 = 1$;
$(1, 1)$ gives $2 - 2 = 0$.
:::

::: card
A deep network computes parity as a tree. The first layer takes the XOR of pairs, the next layer
the XOR of those results, and so on. With $n$ bits that takes $\log_2 n$ levels and $n - 1$ XOR
units, so $2(n - 1)$ neurons in total.

A single hidden layer needs roughly $2^{n-1}$ neurons. At $n = 30$ that is about
$5.4 \times 10^{8}$, against 58 for the tree.

```plot
x: { var: n, label: "number of bits $n$", from: 2, to: 30, ticks: 2, grid: true }
y: { label: "neurons", from: 1, to: 1000000000, scale: log }

draw:
  - curve: { is: 2 ^ (n - 1), accent: true, label: "one hidden layer" }
  - curve: { is: 2 * (n - 1), label: "a tree of depth $log₂ n$" }
  - point: { at: [30, 536870912], label: "$5.4 × 10⁸$" }
  - point: { at: [30, 58], label: "58" }
```
:::

::: card
In practice, depth lets a network build **hierarchical features**, each layer from the one below.
In image recognition, early layers learn edges, middle layers learn shapes, and deep layers learn
objects. In language, early layers learn character patterns, middle layers learn words and
phrases, and deep layers learn meaning.

This order of layers mirrors the structure of the data itself.
:::

::: exercise parity-value
What is the parity of the bits $(1, 1, 0, 1)$?

::: answer
1. Three of the bits are 1, and three is odd.
:::
:::

::: exercise parity-tree-size
A tree of XOR units computes the parity of 16 bits. How many XOR units does it use, and how many
levels deep is it?

::: answer
15 units in 4 levels. Each level halves the number of values: $16 \to 8 \to 4 \to 2 \to 1$.
:::

::: solution
Level 1: 8 units. Level 2: 4 units. Level 3: 2 units. Level 4: 1 unit.

Total: $8 + 4 + 2 + 1 = 15 = n - 1$, in $\log_2 16 = 4$ levels. ∎
:::
:::

::: exercise xor-relu-check
Evaluate $\max(0, a + b) - 2\max(0, a + b - 1)$ at $a = 0.5$, $b = 0.5$.

::: answer
1. Both terms see $a + b = 1$: $1 - 2 \cdot 0 = 1$.
:::
:::

::: reference xor-from-relu
# XOR from two ReLUs

Two ReLU neurons and a linear output compute the XOR of two bits.

::: equation
\text{XOR}(a, b) = \max(0, a + b) - 2\max(0, a + b - 1) \qquad a, b \in \{0, 1\}
:::

::: legend
$a, b$: the two input bits
$\max(0, \cdot)$: the ReLU
:::

::: derivation
Set $s = a + b$, which takes the values 0, 1 or 2.[ReLU](reference:relu)
$s = 0$: $\max(0, 0) - 2\max(0, -1) = 0 - 0 = 0$.
$s = 1$: $\max(0, 1) - 2\max(0, 0) = 1 - 0 = 1$.
$s = 2$: $\max(0, 2) - 2\max(0, 1) = 2 - 2 = 0$.
The output is 1 exactly when one bit is 1, which is XOR. ∎
:::
:::

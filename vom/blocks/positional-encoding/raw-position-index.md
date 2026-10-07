---
title: Why the raw index fails
---

::: card
The simplest fix is to append the position number. The word "cat" at position 3 has a 512-wide
embedding; add one more coordinate that holds 3, and the vector is 513 wide:
$[\,\text{cat embedding},\ 3\,]$. This fails in three ways.
:::

::: card
First, unseen values. A model trained on sequences of length 100 only ever sees the numbers 1 to
100 in that coordinate. Give it a sequence of length 200 and it meets 101 to 200, values it has
never seen. It has nothing to go on for position 150.
:::

::: card
Second, uneven steps. Positions 1 and 2 differ by 1, and so do positions 100 and 101. But going
from 1 to 2 is a 100% increase, and going from 100 to 101 is a 1% increase. The relative step
$1/t$ shrinks along the sequence, so early positions look far apart and late ones look crowded.

```plot
x: { var: t, label: "position $t$", from: 1, to: 100, ticks: 10, grid: true }
y: { label: "relative step to $t + 1$", from: 0, to: 1.05 }

draw:
  - curve: { is: 1 / t, accent: true }
  - point: { at: [1, 1], label: "100%" }
  - point: { at: [10, 0.1], label: "10%" }
  - point: { at: [100, 0.01], label: "1%" }
```
:::

::: card
Third, size. Position 10,000 puts the number 10,000 into the vector, beside content values near
1. In a dot product with learned weights, that one coordinate swamps the rest, and training turns
unstable.
:::

::: card
So the position vector must meet three demands. Its values stay bounded, whatever the position.
Nearby positions get similar vectors. And it is defined for every position, including positions
never seen in training.
:::

::: exercise q1
With the raw index, what is the relative step from position 50 to position 51?

::: answer
2%. The step is 1, and $1/50 = 0.02$.
:::
:::

::: exercise q2
A model with a raw position index trained on positions 1 to 512 reads a sequence of 600 tokens.
How many of its positions did it never see in training?

::: answer
88. Positions 513 to 600 are new.
:::

::: solution
$$ 600 - 512 = 88 $$

∎
:::
:::

::: exercise q3
To keep values small, you scale the index to $t / 100$ and train on positions 1 to 100. What value
does position 150 get, and which demand does the scheme still break?

::: answer
1.5, above the largest value 1.0 seen in training. It still gives unseen positions unseen values.
:::
:::

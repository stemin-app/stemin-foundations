---
title: Emergent capabilities
---

::: card
The loss falls smoothly with scale. Some abilities do not: they stay near chance, then switch on
within less than a factor of ten in model size. These are called **emergent capabilities**.
:::

::: card
The reported examples are striking. Below about 10 billion parameters, models fail at three digit
addition; above, accuracy jumps past 80%. Asked to "think step by step", small models ignore the
instruction, while models of roughly 100 billion parameters use the steps and gain 20 to 50 points
on mathematics. Unscrambling "elppa" into "apple", learning a task from examples in the prompt,
and spotting a logical fallacy all appear late in the same abrupt way.
:::

::: card
One explanation is **decomposition**. A task that needs three steps in a row succeeds only if all
three succeed. With 60% per step, the task succeeds $0.6^3 = 22\%$ of the time. With 90% per
step, $0.9^3 = 73\%$. A steady gain on each step becomes a sudden gain on the whole task.
:::

::: card
Watch it happen. The dashed curve is the accuracy of one step, rising smoothly with the log of the
model size. The blue curve is a task that needs $k$ correct steps, $p^k$. Raise $k$: the smooth
rise turns into a late, sharp switch, though nothing sudden happened underneath.

```plot
x: { var: x, label: "model size, log₁₀ N", from: 7, to: 12, ticks: 1, grid: true }
y: { label: "accuracy", from: 0, to: 1.05 }

inputs:
  - { name: k, min: 1, max: 20, default: 8, step: 1, label: "steps the task needs, k" }

let:
  p: 1 / (1 + exp(-1.5 * (x - 9)))

draw:
  - curve: { is: p, dash: true, label: "one step" }
  - curve: { is: p ^ k, accent: true, label: "whole task" }
```
:::

::: card
This feeds a debate. Some emergence may be a **measurement artifact**: exact-match accuracy scores
an answer right or wrong, so it hides steady progress until the last step falls into place. A
smoother metric, such as the log probability of the right answer, often rises gradually where
accuracy jumps. Other cases look like real qualitative change: new circuits that only work once
all their parts exist, or internal representations that cross a threshold of abstraction.
:::

::: card
Whatever the cause, emergence is hard to predict. A scaling law forecasts the loss of a ten times
larger model well. It does not say which abilities that lower loss unlocks. A capability with no
signal below its threshold gives nothing to extrapolate, so in practice new abilities are found by
training the larger model and testing it.
:::

::: exercise q1
A task needs 5 steps in a row, each right with probability 0.8. What is the chance the whole task
succeeds? And at 0.95 per step?

::: answer
About 0.33 and about 0.77. The chances are $0.8^5$ and $0.95^5$.
:::

::: solution
$0.8^5 = 0.328$.

$0.95^5 = 0.774$. ∎
:::
:::

::: exercise q2
Why can a metric of exact-match accuracy show a jump while the loss falls smoothly?

::: answer
It counts an answer as right only when every part is right, so partial progress scores zero until
the last part is in place.
:::
:::

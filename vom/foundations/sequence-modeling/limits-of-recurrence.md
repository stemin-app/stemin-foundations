---
title: The limits of recurrence
---

::: card
LSTMs, and the simpler gated recurrent unit or GRU, brought real progress in translation, speech
recognition and text generation. Three limits remain, and they are built into recurrence itself.
:::

::: card
**Sequential computation.** $\mathbf{h}_t$ needs $\mathbf{h}_{t-1}$, so a sequence of length $T$
takes $T$ steps, one after another. A GPU does thousands of operations at once, and it sits idle
while each step waits for the last. Training is slow.
:::

::: card
**Long range is still hard.** A signal between two positions must cross every step between them.
In practice LSTMs handle dependencies of tens or a few hundred tokens, and lose track beyond.
Below, the dashed line is the number of steps a signal crosses in a recurrent network, against
the distance between the two words. The blue line is the goal: one step at any distance.

```plot
x: { var: k, label: "distance between two words", from: 1, to: 100, ticks: 10, grid: true }
y: { label: "steps the signal crosses", from: 0, to: 100 }

draw:
  - curve: { is: k, dash: true, label: "recurrence" }
  - curve: { is: 1, accent: true, label: "direct link" }
```
:::

::: card
**The bottleneck.** Everything about the past must fit into one fixed vector $\mathbf{h}_t$. To
translate, an RNN encoder squeezes a whole source sentence into one vector, and the decoder must
rebuild the translation from it. Early words are read first and partly overwritten by later
ones.
:::

::: card
So the next architecture needs three things: process all positions in parallel, connect any
position to any other directly, and avoid one fixed bottleneck. **Attention** does all three. It
began as an addition to RNNs for translation. The transformer then showed that attention alone,
with no recurrence, works better and trains far faster.
:::

::: exercise q1
An RNN reads a sequence of 2,000 tokens. How many steps must run one after another?

::: answer
2,000. Each hidden state needs the one before it.
:::
:::

::: exercise q2
Name the three limits of recurrence.

::: answer
Sequential computation, weak long-range dependencies, and the fixed-size hidden state as a
bottleneck.
:::
:::

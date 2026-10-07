---
title: Why a feedforward network fails
---

::: card
Say you classify sentences as positive or negative. A feedforward network needs an input of
fixed size. There are three obvious ways to turn a sentence into one, and each one breaks.
:::

::: card
**Pad and flatten.** Fix a maximum length $T_{\max}$, pad short sentences with zeros, cut long
ones. Write each word as a one-hot vector of size $V$, and flatten the $T_{\max} \times V$ grid
into one vector. With $V = 10{,}000$ and $T_{\max} = 100$, the input has a million entries, almost
all zero.
:::

::: card
Worse, the network learns separate weights for "good" in position 1, "good" in position 2, and so
on. What it learns about a word in one place does not carry to another place. It cannot see that
"good" means the same thing wherever it stands.
:::

::: card
**Bag of words.** Count the words and ignore their order: the input is a vector of $V$ counts.
Now "The food was good, not bad" and "The food was bad, not good" have the same input, and
opposite meanings. All order is lost.
:::

::: card
**$n$-grams.** Add pairs or triples of neighbouring words as extra features. The count explodes:
$V^2$ pairs, $V^3$ triples. Most never appear in training, and a dependency longer than $n$ words
is still missed.

```plot
x: { var: n, label: "n, the length of the word groups", from: 1, to: 5, ticks: 1, grid: true }
y: { label: "possible features", from: 1000, to: 1e21, scale: log }

inputs:
  - { name: v, min: 1000, max: 50000, default: 10000, step: 1000, label: "vocabulary size V" }

draw:
  - curve: { is: v ^ n, accent: true }
  - point: { at: [2, v ^ 2], label: "pairs" }
  - point: { at: [3, v ^ 3], label: "triples" }
```
:::

::: card
The root problem: a feedforward network reads its input as an unstructured vector. It has no
notion of a sequence, of position, or of one pattern that can appear at many positions. The fix
has to build that notion into the architecture.
:::

::: exercise q1
With $V = 5{,}000$ and $T_{\max} = 40$, how many entries does the flattened one-hot input have?

::: answer
200,000. It is $T_{\max} \cdot V$.
:::
:::

::: exercise q2
Give two sentences with different meanings and the same bag of words.

::: answer
For example, "the dog chased the cat" and "the cat chased the dog". The counts match; the order
does not.
:::
:::

::: exercise q3
With $V = 10{,}000$, how many possible word pairs are there?

::: answer
$10^8$, a hundred million. It is $V^2$.
:::
:::

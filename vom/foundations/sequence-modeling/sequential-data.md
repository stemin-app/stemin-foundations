---
title: What makes a sequence hard
---

::: card
A **sequence** is an ordered list of elements, $\mathbf{x} = (x_1, x_2, \ldots, x_T)$, where $T$
is its length. In language each $x_t$ is a word or a piece of a word, a **token**. In a time
series each $x_t$ is a measurement at time $t$. Four properties make sequences hard to model.
:::

::: card
**Variable length.** A sentence may have 5 words or 50. One model must handle both without being
rebuilt.

**Order.** "Dog bites man" and "man bites dog" hold the same words and mean different things. The
order carries meaning.
:::

::: card
**Long-range dependencies.** Elements far apart can be tied together. In "The cat, which had been
sleeping peacefully on the warm windowsill all afternoon, suddenly woke up", the verb "woke"
belongs to "cat", eleven words back, and not to the nearby "windowsill" or "afternoon".
:::

::: card
**Context.** One word means different things in different places. "Bank" in "river bank" is not
"bank" in "bank account". A model must read each word in the light of the words around it.
:::

::: exercise q1
"The keys to the cabinet are on the table." Which word decides that the verb is "are" and not
"is", and which property of sequences does this show?

::: answer
"keys", four words before the verb. It is a long-range dependency: the nearer noun "cabinet" is
singular.
:::
:::

::: exercise q2
"She saw the bat fly out of the cave" and "He swung the bat at the ball". Which property of
sequences do the two sentences show?

::: answer
Context. The same word "bat" means an animal in one and a club in the other.
:::
:::

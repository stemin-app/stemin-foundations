---
title: The geometry of meaning
---

::: card
Good embeddings have a striking property. Take the vector of "king", subtract the vector of
"man", and add the vector of "woman". The result lands near the vector of "queen":

$$ \mathbf{e}_{\text{king}} - \mathbf{e}_{\text{man}} + \mathbf{e}_{\text{woman}} \approx \mathbf{e}_{\text{queen}} $$

Here $\mathbf{e}_{\text{word}}$ is the embedding of that word.
:::

::: card
Rearrange the equation and it says something about differences:

$$ \mathbf{e}_{\text{king}} - \mathbf{e}_{\text{man}} \approx \mathbf{e}_{\text{queen}} - \mathbf{e}_{\text{woman}} $$

The step from "man" to "king" is about the same vector as the step from "woman" to "queen".
Both steps point along one direction, which you can read as royalty.
:::

::: card
In two dimensions you can watch it. The horizontal axis plays royalty and the vertical axis
plays gender. Slide $s$ from 0 to 1 to carry the step from "man" to "king" over to "woman". At
$s = 1$ the point sits at $\mathbf{e}_{\text{woman}} + (\mathbf{e}_{\text{king}} - \mathbf{e}_{\text{man}})$,
next to "queen".

```plot
x: { var: x, label: "royalty", from: 0, to: 5.5, ticks: 1 }
y: { label: "gender", from: 0, to: 4.2, ticks: 1 }

inputs:
  - { name: s, min: 0, max: 1, default: 1, step: 0.05, label: "how much of the step s" }

draw:
  - param: { var: u, over: [0, 1], x: 1 + 3 * u, y: 1 + 0.3 * u }
  - param: { var: u, over: [0, 1], x: 1 + 3 * s * u, y: 3 + 0.3 * s * u, accent: true, dash: true }
  - point: { at: [1, 1], label: man }
  - point: { at: [4, 1.3], label: king }
  - point: { at: [1, 3], label: woman }
  - point: { at: [4.1, 3.5], label: queen }
  - point: { at: [1 + 3 * s, 3 + 0.3 * s] }
```
:::

::: card
To answer an analogy, you compute $\mathbf{e}_{\text{king}} - \mathbf{e}_{\text{man}} + \mathbf{e}_{\text{woman}}$
and look for the word whose vector has the highest cosine similarity with the result. The three
words of the question are left out of the search. The winner is "queen".
:::

::: card
The property comes from context. "King" and "queen" appear in similar contexts: ruling, crowns,
thrones. So their vectors are close. But "king" also shares contexts with "man" (he, his,
himself), and "queen" with "woman" (she, her, herself). Training separates the two influences
into two directions, one for gender and one for royalty, and they add.
:::

::: card
None of this is reasoning. It is a geometric result of how the vectors are trained. If the
training text keeps pairing masculine pronouns with "king" and feminine pronouns with "queen",
the differences $\mathbf{e}_{\text{king}} - \mathbf{e}_{\text{man}}$ and
$\mathbf{e}_{\text{queen}} - \mathbf{e}_{\text{woman}}$ both end up along the royalty direction.
:::

::: exercise analogy-vector
In two dimensions, $\mathbf{e}_{\text{man}} = (1, 1)$, $\mathbf{e}_{\text{woman}} = (1, 3)$ and
$\mathbf{e}_{\text{king}} = (4, 1)$. What is $\mathbf{e}_{\text{king}} - \mathbf{e}_{\text{man}} + \mathbf{e}_{\text{woman}}$?

::: answer
$(4, 3)$. Add and subtract coordinate by coordinate.
:::

::: solution
$$ (4, 1) - (1, 1) + (1, 3) = (4 - 1 + 1,\ 1 - 1 + 3) = (4, 3) $$

∎
:::
:::

::: exercise analogy-difference
The analogy $\text{king} - \text{man} + \text{woman} \approx \text{queen}$ holds. Which vector difference is
$\mathbf{e}_{\text{queen}} - \mathbf{e}_{\text{woman}}$ close to?

::: answer
$\mathbf{e}_{\text{king}} - \mathbf{e}_{\text{man}}$. Subtract $\mathbf{e}_{\text{woman}}$ from both sides of the analogy.
:::
:::

::: exercise analogy-nearest
The analogy vector of the first exercise is $(4, 3)$. Two candidates are
$\mathbf{e}_{\text{queen}} = (4, 3.2)$ and $\mathbf{e}_{\text{prince}} = (4.5, 1)$. Which has
the higher cosine similarity with $(4, 3)$?

::: answer
"Queen", with a similarity of about 1.00 against 0.91 for "prince".
:::

::: solution
The analogy vector has length $\sqrt{16 + 9} = 5$.

$$ \text{sim}((4,3), \text{queen}) = \frac{16 + 9.6}{5 \sqrt{16 + 10.24}} = \frac{25.6}{5 \times 5.122} \approx 0.9995 $$

$$ \text{sim}((4,3), \text{prince}) = \frac{18 + 3}{5 \sqrt{20.25 + 1}} = \frac{21}{5 \times 4.610} \approx 0.911 $$

"Queen" is closer. ∎
:::
:::

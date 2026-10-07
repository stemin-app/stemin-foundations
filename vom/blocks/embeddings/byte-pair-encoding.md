---
title: Byte pair encoding
---

::: card
A vocabulary of whole words has holes. Misspellings, rare words, technical terms, new slang and
the many forms of one word (run, running, runner) all need a vector. A vocabulary of 50,000
words sounds large, but it cannot cover all of them.
:::

::: card
*Subword tokenization* fills the holes. The vocabulary keeps common words whole and breaks rare
words into smaller pieces. A word the model has never seen still becomes a sequence of pieces it
knows.
:::

::: card
*Byte pair encoding*, or BPE (2015), builds such a vocabulary from the bottom up. Most modern
large language models use it or a variant, GPT-2, GPT-3, GPT-4 and LLaMA among them. The
algorithm has four steps:

1. Start with a vocabulary of single characters.
2. Count every pair of adjacent tokens in the training text.
3. Merge the most frequent pair into one new token.
4. Repeat until the vocabulary reaches the size you want.
:::

::: card
Run it on a corpus of three words: "low", "lower" and "lowest". As characters they read

$$ \text{l o w} \qquad \text{l o w e r} \qquad \text{l o w e s t} $$

The starting vocabulary is $\{\text{l}, \text{o}, \text{w}, \text{e}, \text{r}, \text{s}, \text{t}\}$.
:::

::: card
Count the adjacent pairs. $(\text{l}, \text{o})$ appears 3 times, once in each word, and so
does $(\text{o}, \text{w})$. Break the tie alphabetically and merge $(\text{l}, \text{o})$
first. The vocabulary gains $\text{lo}$, and the corpus becomes

$$ \text{lo w} \qquad \text{lo w e r} \qquad \text{lo w e s t} $$
:::

::: card
Count again. Now $(\text{lo}, \text{w})$ appears 3 times, more than any other pair. Merge it:

$$ \text{low} \qquad \text{low e r} \qquad \text{low e s t} $$

Next, $(\text{low}, \text{e})$ appears twice and every other pair once, so $\text{lowe}$
comes next ([Figure](figure:bpe-merges)).
:::

::: figure bpe-merges
![Four rows of tokens for "lowest"](assets/bpe-merges.svg)

The word "lowest" after each merge. The outlined token is the one the latest merge created.
:::

::: card
On three words, BPE soon rebuilds the words themselves. On a real corpus, endings such as
$\text{er}$ and $\text{est}$ occur in thousands of words (newer, faster, largest), so their
pairs are frequent and they become tokens of their own. The final vocabulary holds pieces like
$\text{low}$, $\text{er}$ and $\text{est}$ beside whole common words.
:::

::: card
To tokenize new text, you apply the learned merges in the order they were learned. A common word
like "lowest" may become one token, $[\text{lowest}]$. A rarer one like "lowest-ever" may
become $[\text{lowest}, \text{-}, \text{ever}]$. A new word like "transformerize" may
become $[\text{transform}, \text{er}, \text{ize}]$. Every piece is familiar, though the
word is not.
:::

::: exercise bpe-first-merge
A corpus holds the words "hug", "hugs" and "bug". Which pair does BPE merge first?

::: answer
$(\text{u}, \text{g})$, which appears 3 times. Count the adjacent pairs in all three words.
:::

::: solution
$\text{h u g}$ gives $(\text{h}, \text{u})$ and $(\text{u}, \text{g})$.

$\text{h u g s}$ gives $(\text{h}, \text{u})$, $(\text{u}, \text{g})$ and $(\text{g}, \text{s})$.

$\text{b u g}$ gives $(\text{b}, \text{u})$ and $(\text{u}, \text{g})$.

Counts: $(\text{u}, \text{g})$ 3, $(\text{h}, \text{u})$ 2, $(\text{g}, \text{s})$ 1, $(\text{b}, \text{u})$ 1. ∎
:::
:::

::: exercise bpe-vocabulary-size
On the corpus "hug", "hugs", "bug", BPE makes two merges. How many tokens does the vocabulary hold
after them?

::: answer
7. It starts with 5 characters, and each merge adds one token.
:::

::: solution
The characters are $\text{b}, \text{g}, \text{h}, \text{s}, \text{u}$: 5 tokens.

The first merge adds $\text{ug}$. The corpus becomes $\text{h ug}$, $\text{h ug s}$, $\text{b ug}$.

The pair $(\text{h}, \text{ug})$ now appears twice, so the second merge adds $\text{hug}$.

$$ 5 + 2 = 7 $$

∎
:::
:::

::: exercise bpe-merge-count
A byte-level BPE starts from 256 byte tokens and adds one token per merge. How many merges give
a vocabulary of 50,256 tokens?

::: answer
50,000. Subtract the 256 starting tokens.
:::
:::

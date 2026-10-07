---
title: Adapters, prefixes and the choice
---

::: card
An **adapter** is a small trainable module placed between frozen layers. It projects down from
$d$ to $r$ dimensions with $\mathbf{W}_{down} \in \mathbb{R}^{r \times d}$, applies a nonlinearity such
as [ReLU](reference:relu), projects back up with $\mathbf{W}_{up} \in \mathbb{R}^{d \times r}$, and adds the
result to its input through a [residual connection](reference:residual-connection):

$$ \text{Adapter}(\mathbf{x}) = \mathbf{x} + \mathbf{W}_{up} \, \text{ReLU}(\mathbf{W}_{down}\, \mathbf{x}) $$
:::

::: card
Adapters sit after the attention sublayer, after the feed-forward sublayer, or after both. Start
$\mathbf{W}_{up}$ at zero and the adapter returns $\mathbf{x}$ unchanged, so training begins at the
pretrained model, as with LoRA.

The difference shows at inference. An adapter adds matrix products in sequence, on every
forward pass, for good. LoRA merges into $\mathbf{W}$ and adds nothing. So LoRA is usually preferred.
:::

::: card
**Prefix tuning** changes the input, not the weights. It puts $m$ learned vectors in front of the
embedded tokens:

$$ \text{Input} = [\mathbf{p}_1, \ldots, \mathbf{p}_m, \mathbf{x}_1, \ldots, \mathbf{x}_n] $$

Every model weight stays frozen. Only the $\mathbf{p}_i$ train, one set per task. The prefix acts as a
learned context that steers the model: for summarization it might encode "summarize the
following".
:::

::: card
A hard prompt is real text, such as "Summarize:". A **soft prompt** is a set of vectors that need
not match any token. Gradient descent tunes them directly, so they can beat any prompt you could
write in words.

The prefix costs $m \times d$ numbers, more if a prefix is added at every layer. That is very few,
but the method is less expressive than LoRA on some tasks.
:::

::: card
Pick full fine-tuning when you have 100,000 examples or more, when the task is far from
pretraining (a new language, say), when you need one task only and can store the copy, and when
performance outweighs cost.

Pick LoRA with 1,000 to 100,000 examples, when you need many task versions of one base model, when
general skills must survive, and when inference speed matters.
:::

::: card
Pick adapters when you want modules to swap and compare across many tasks, and their extra
forward cost is acceptable. Pick prefix tuning with 100 to 1,000 examples, for a variation of
something the model already does well, at the smallest footprint.

Fine-tune nothing when you have no task data and good prompts already do the job.
:::

::: card
A deployed model usually passes through several stages:

1. pretraining on general text, at massive compute;
2. continued pretraining on domain text, such as medicine, law or code (optional);
3. instruction tuning, at moderate compute;
4. task-specific fine-tuning, at small compute;
5. alignment with human preferences, by RLHF or DPO.

A common pattern runs full fine-tuning for instruction tuning, where the behaviour must change a
lot, and LoRA for the task stage, to keep the instruction following.
:::

::: exercise adapter-count
An adapter has $d = 4096$ and $r = 64$. Ignoring biases, how many numbers does it train?

::: answer
524,288. The two projections hold $dr$ each, $2dr$ in all.
:::

::: solution
$\mathbf{W}_{down}$: $64 \times 4096 = 262{,}144$

$\mathbf{W}_{up}$: $4096 \times 64 = 262{,}144$

Total: $2 \times 262{,}144 = 524{,}288$ ∎
:::
:::

::: exercise prefix-count
A prefix of $m = 20$ vectors is added at the input of a model with $d = 4096$. How many numbers
train?

::: answer
81,920. Prefix tuning trains $m \times d$.
:::
:::

::: exercise adapter-identity
Why does an adapter with $\mathbf{W}_{up} = 0$ leave the model unchanged?

::: answer
Its output is $\mathbf{x} + \mathbf{0} = \mathbf{x}$. The residual connection passes the input through.
:::
:::

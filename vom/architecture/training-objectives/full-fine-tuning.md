---
title: Fine-tuning every weight
---

::: card
**Fine-tuning** adapts a pretrained model to one task, one domain or one behaviour. You do not
start from random weights. You start from weights that already encode syntax, meaning, facts
about the world, patterns of reasoning and the flow of an argument.

A model that knows English grammar does not relearn it to classify sentiment.
:::

::: card
Pretraining finds weights $\theta_{pre}$ that minimize the loss on a large general corpus.
Fine-tuning starts there and moves to nearby weights $\theta_{fine}$ that minimize the loss on a
small task dataset. Refining is easier than building.

The saving is large: fine-tuning needs 100 to 10,000 times less data than pretraining. A model
pretrained on a trillion tokens may adapt well on 10,000 examples.
:::

::: card
The loss has the same form as in pretraining, on different data:

$$ \mathcal{L}_{fine} = -\sum_{(x, y) \in \mathcal{D}_{task}} \log P_\theta(y \mid x) $$

For classification, $y$ is a label and $P_\theta(y \mid x)$ comes from a classification head added
to the model. For generation, $y$ is a target sequence and the sum runs over its tokens.
:::

::: card
Three things differ from pretraining. The learning rate is 10 to 100 times smaller, about
$10^{-5}$ to $10^{-6}$, so updates are gentle. The run is thousands of steps, one to three passes
over the data. The data is small and focused: 10,000 labelled movie reviews, or 5,000 clinical
question and answer pairs.
:::

::: card
**Full fine-tuning** changes the values of every matrix learned in pretraining, and nothing else:

- the embedding $\mathbf{W}_E \in \mathbb{R}^{V \times d}$;
- in each layer, $\mathbf{W}_Q, \mathbf{W}_K, \mathbf{W}_V, \mathbf{W}_O \in \mathbb{R}^{d \times d}$;
- in each layer, $\mathbf{W}_1 \in \mathbb{R}^{d \times 4d}$ and $\mathbf{W}_2 \in \mathbb{R}^{4d \times d}$;
- the scales and shifts of layer normalization;
- the output $\mathbf{W}_{out} \in \mathbb{R}^{d \times V}$, often tied to $\mathbf{W}_E$.

The number of layers, the heads, the widths and the activations stay as they were.
:::

::: card
Each step is [gradient descent](reference:gradient-descent) from where pretraining stopped:

$$ \theta \leftarrow \theta - \eta \, \nabla_\theta \mathcal{L}_{fine}(\theta), \qquad \theta \text{ starts at } \theta_{pre} $$

Fine-tune on legal text and the embedding of "plaintiff" shifts a little, attention leans toward
legal terms, and the feed-forward layers treat formal language differently. Millions of small
changes add up to a new behaviour.
:::

::: card
Full fine-tuning can change anything, so it adapts best when the task is far from pretraining.
It also stores a complete model for every task. A 70B model at 2 bytes per weight takes 140 GB.
Ten tasks take 1.4 TB. With every weight free, it also overfits a small dataset fast.
:::

::: card
Fine-tune on task A and the model can lose what it could do before. This is **catastrophic
forgetting**: the new gradients overwrite information stored in the weights.

Tune a general model on legal documents and it may excel at legal language while it stumbles on
casual chat or code. $\theta_{pre}$ sat in a region that served many tasks. $\theta_{fine}$ serves one,
and may sit outside that region.
:::

::: card
Five defences limit forgetting. A lower learning rate keeps $\theta$ close to $\theta_{pre}$. Early
stopping watches general benchmarks and halts in time. **Replay** mixes pretraining text into the
task data. Parameter-efficient methods freeze most weights. And a penalty can pull the weights
back toward $\theta_{pre}$:
[Penalty toward the pretrained weights](reference:pretrained-weight-penalty)

$$ \mathcal{L} = \mathcal{L}_{fine}(\theta) + \lambda \, \lVert \theta - \theta_{pre} \rVert^2 $$
:::

::: card
See the penalty in one dimension. Put $\theta_{pre} = 0$ and let the task alone want $\theta = 3$,
with $\mathcal{L}_{fine} = (\theta - 3)^2$. Raise $\lambda$ and the minimum of the sum slides from 3
toward 0, to $\theta^* = \frac{3}{1 + \lambda}$.

```plot
x: { var: th, label: "$θ$", from: -1, to: 4, ticks: 1, grid: true }
y: { label: "loss", from: 0, to: 12 }

inputs:
  - { name: lam, min: 0, max: 5, default: 1, step: 0.1, label: "the penalty weight λ" }

draw:
  - curve: { is: (th - 3)^2, dash: true, label: task }
  - curve: { is: lam * th^2, dash: true, label: penalty }
  - curve: { is: (th - 3)^2 + lam * th^2, accent: true, label: sum }
  - vline: { at: 3 / (1 + lam), dash: true }
  - point: { at: [3 / (1 + lam), 9 * lam / (1 + lam)], label: "$θ*$" }
```
:::

::: exercise ft-penalty-minimum
With $\mathcal{L}_{fine} = (\theta - 3)^2$, $\theta_{pre} = 0$ and $\lambda = 2$, where is the minimum of
$\mathcal{L}_{fine} + \lambda \theta^2$?

::: answer
$\theta^* = 1$. Set the derivative to zero: $\theta^* = \frac{3}{1 + \lambda}$.
:::

::: solution
$\frac{d}{d\theta}\left[(\theta - 3)^2 + 2\theta^2\right] = 2(\theta - 3) + 4\theta = 6\theta - 6$

$6\theta - 6 = 0 \implies \theta^* = 1$ ∎
:::
:::

::: exercise ft-storage
You fully fine-tune a 7B model for 5 tasks and store each copy at 2 bytes per weight. How much
storage do the copies take?

::: answer
70 GB. Each copy is $7 \times 10^9 \times 2$ bytes $= 14$ GB.
:::
:::

::: exercise ft-layer-count
A layer has $d = 4096$, four $d \times d$ attention matrices, and the feed-forward pair
$d \times 4d$ and $4d \times d$. Ignoring biases and normalization, how many weights does full
fine-tuning update in the layer?

::: answer
$12 d^2 = 201{,}326{,}592$. The attention holds $4d^2$ and the feed-forward pair $8d^2$.
:::

::: solution
$d^2 = 4096^2 = 16{,}777{,}216$

Attention: $4d^2 = 67{,}108{,}864$

Feed-forward: $d \cdot 4d + 4d \cdot d = 8d^2 = 134{,}217{,}728$

Total: $12 d^2 = 201{,}326{,}592$ ∎
:::
:::

::: reference pretrained-weight-penalty
# Penalty toward the pretrained weights

A quadratic penalty on the distance from the pretrained weights limits catastrophic forgetting.
For a quadratic task loss in one weight, the minimum moves toward the pretrained value by the
factor $\frac{1}{1 + \lambda}$.

::: equation
\mathcal{L} = \mathcal{L}_{fine}(\theta) + \lambda \lVert \theta - \theta_{pre} \rVert^2, \qquad \theta^* = \theta_{pre} + \frac{a - \theta_{pre}}{1 + \lambda} \ \text{ for } \ \mathcal{L}_{fine} = (\theta - a)^2
:::

::: legend
$\theta$: the weights being fine-tuned
$\theta_{pre}$: the pretrained weights
$\lambda$: the weight of the penalty
$a$: the minimum of the task loss alone
$\theta^*$: the minimum of the penalized loss
:::

::: derivation
Goal: the minimum of $(\theta - a)^2 + \lambda(\theta - \theta_{pre})^2$.
Differentiate: $\frac{d\mathcal{L}}{d\theta} = 2(\theta - a) + 2\lambda(\theta - \theta_{pre})$.[Derivative](reference:derivative)
Set it to zero: $(1 + \lambda)\theta = a + \lambda\theta_{pre}$.
Solve: $\theta^* = \frac{a + \lambda\theta_{pre}}{1 + \lambda} = \theta_{pre} + \frac{a - \theta_{pre}}{1 + \lambda}$.
The second derivative is $2(1 + \lambda) > 0$, so this is a minimum. ∎
:::
:::

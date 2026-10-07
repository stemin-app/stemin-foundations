---
title: Spending a compute budget
---

::: card
Training a model with $N$ parameters on $D$ tokens costs about

$$ C \approx 6\,N\,D\ \text{FLOPs} $$

Each token costs about $2N$ FLOPs on the forward pass, one multiply and one add per parameter,
and about $4N$ on the backward pass.
[Training compute](reference:training-compute)
:::

::: card
Fix the budget $C$ and the two resources trade against each other: $D = C / (6N)$. A larger
model sees fewer tokens. Too small a model wastes the budget on data it cannot use. Too large a
model stops before it has learned enough. Somewhere between lies the model size with the lowest
loss.
:::

::: card
Each curve below is one budget: the loss of every model size you could train with it. The law
here is a made-up one with equal exponents, $L = 1.7 + 400\,N^{-0.34} + B\,D^{-0.34}$, with $B$ set so that the best split is 20
tokens per parameter. Drag the budget. The lowest point slides to larger models, and the whole curve drops.

```plot
x: { var: N, label: "$N$", from: 1e7, to: 1e12, scale: log, ticks: 1, grid: true }
y: { label: "$L$ in nats", from: 1.5, to: 4, ticks: 0.5, grid: true }

inputs:
  - { name: lc, min: 19, max: 24, default: 21, step: 0.5, label: "the budget log₁₀ C" }

let:
  Cv: 10^lc
  Bc: 400 * 20^0.34
  No: sqrt(Cv / 120)

draw:
  - hline: { at: 1.7, dash: true, label: "L∞" }
  - curve: { is: 1.7 + 400 * N^(-0.34) + Bc * (Cv / (6 * N))^(-0.34), accent: true }
  - point: { at: [No, "1.7 + 800 * No^(-0.34)"], label: "best size" }
```
:::

::: card
In 2020 Kaplan and colleagues found that the best size grows fast, $N_\text{opt} \propto C^{0.73}$:
spend extra compute mostly on a bigger model. In 2022 Hoffmann and colleagues, in the Chinchilla
paper, found that the two should grow equally:

$$ N_\text{opt} \propto C^{0.5} \qquad D_\text{opt} \propto C^{0.5} $$
:::

::: card
Here are the two answers side by side, both drawn through Chinchilla's own run, 70 billion
parameters at $5.88 \times 10^{23}$ FLOPs. The exponent $a$ sets $N_\text{opt} \propto C^a$, and
the tokens take the rest, $D = C/(6N)$. At $a = 0.5$ the lines run parallel, 20 tokens for every
parameter at every budget. At Kaplan's 0.73 the model line climbs and the token line flattens.

```plot
x: { var: C, label: "$C$ in FLOPs", from: 1e19, to: 1e26, scale: log, ticks: 1, grid: true }
y: { label: "count", from: 1e7, to: 1e14, scale: log, ticks: 1, grid: true }

inputs:
  - { name: a, min: 0.3, max: 0.8, default: 0.5, step: 0.01, label: "the exponent a" }

let:
  Nopt: 7e10 * (C / 5.88e23)^a

draw:
  - curve: { is: Nopt, accent: true, label: "parameters" }
  - curve: { is: C / (6 * Nopt), dash: true, label: "tokens" }
  - point: { at: [5.88e23, 7e10], label: "Chinchilla" }
```
:::

::: card
The practical rule is to train on about 20 tokens per parameter. A 7 billion parameter model
should see about 140 billion tokens. A 70 billion parameter model should see about 1.4 trillion.

Put $D = 20N$ into $C = 6ND$ and you get $C = 120\,N^2$, so

$$ N_\text{opt} = \sqrt{\frac{C}{120}} \qquad D_\text{opt} = 20\,N_\text{opt} $$

A budget of $10^{23}$ FLOPs buys about 29 billion parameters on 580 billion tokens.
:::

::: card
The rule overturned the practice of its day. GPT-3 has 175 billion parameters and trained on 300
billion tokens, under two tokens per parameter. Chinchilla has 70 billion parameters and trained
on 1.4 trillion tokens. It matched or beat GPT-3 with two and a half times fewer parameters, and
beat DeepMind's own Gopher, 280 billion parameters, with four times fewer.
:::

::: card
Fewer parameters matter after training. Every query runs the whole model, so the cost of
inference grows with $N$. A compute-optimal model reaches the same loss with fewer parameters,
and it is cheaper to serve. You pay more in training, once, to pay less at every query.
:::

::: card
The split follows from the loss with a floor. Minimize
$L_\infty + (N_c/N)^{\alpha_N} + (D_c/D)^{\alpha_D}$ with $D = C/(6N)$, and the best size is

$$ N_\text{opt} \propto C^{\alpha_D / (\alpha_N + \alpha_D)} \qquad D_\text{opt} \propto C^{\alpha_N / (\alpha_N + \alpha_D)} $$

Equal exponents give $C^{0.5}$ for both. Kaplan's 0.73 and Chinchilla's 0.5 come from different
experiments and different fits, not from different mathematics.
[Compute-optimal allocation](reference:compute-optimal-allocation)
:::

::: exercise compute-optimal-budget
You have $6 \times 10^{22}$ FLOPs. Under the 20 tokens per parameter rule, how large a model
should you train, and on how many tokens?

::: answer
About 22 billion parameters on about 450 billion tokens. Use $N = \sqrt{C/120}$ and $D = 20N$.
:::

::: solution
$$ N = \sqrt{\frac{6 \times 10^{22}}{120}} = \sqrt{5 \times 10^{20}} = 2.24 \times 10^{10} $$

$$ D = 20 \times 2.24 \times 10^{10} = 4.47 \times 10^{11} $$

Check: $6 \times 2.24 \times 10^{10} \times 4.47 \times 10^{11} = 6.0 \times 10^{22}$. ∎
:::
:::

::: exercise compute-optimal-gpt3
GPT-3 has 175 billion parameters and trained on 300 billion tokens. What did it cost in FLOPs,
and what model size would the 20 tokens per parameter rule pick for the same budget?

::: answer
About $3.15 \times 10^{23}$ FLOPs, and about 51 billion parameters on 1 trillion tokens.
:::

::: solution
$$ C = 6 \times 1.75 \times 10^{11} \times 3 \times 10^{11} = 3.15 \times 10^{23}\ \text{FLOPs} $$

$$ N = \sqrt{\frac{3.15 \times 10^{23}}{120}} = \sqrt{2.63 \times 10^{21}} = 5.12 \times 10^{10} $$

$$ D = 20 \times 5.12 \times 10^{10} = 1.02 \times 10^{12} $$

∎
:::
:::

::: exercise compute-optimal-hundredfold
The budget grows 100 times. By what factor does the best model size grow under Chinchilla's
$C^{0.5}$, and under Kaplan's $C^{0.73}$?

::: answer
10 times under Chinchilla, about 29 times under Kaplan. Raise 100 to the exponent.
:::

::: solution
Chinchilla: $100^{0.5} = 10$.

Kaplan: $100^{0.73} = 10^{1.46} = 28.8$.

Under Kaplan the tokens grow only $100 / 28.8 = 3.5$ times. ∎
:::
:::

::: reference training-compute
# Training compute

Training a dense transformer with $N$ parameters on $D$ tokens costs about six FLOPs per
parameter per token.

::: equation
C \approx 6\,N\,D
:::

::: legend
$C$: the training compute, in FLOPs
$N$: the number of parameters
$D$: the number of training tokens
:::

::: derivation
Most of the work is matrix products, one weight per multiply and add.[Matrix-vector product](reference:matrix-vector-product)
Forward pass, per token: each parameter takes one multiply and one add, $2N$ FLOPs.
Backward pass, per token: each weight matrix gives two products, one for the gradient of the input and one for the gradient of the weight, $4N$ FLOPs.[Backpropagation](reference:backpropagation)
Total per token: $2N + 4N = 6N$.
Over $D$ tokens: $C \approx 6ND$.
Attention over the context and the embeddings add a smaller share, which the rule leaves out. ∎
:::
:::

::: reference compute-optimal-allocation
# Compute-optimal allocation

For a fixed budget $C = 6ND$ and a loss with one power term per resource, the best model size
and the best dataset grow as powers of the budget. The two exponents add to one.

::: equation
N_\text{opt} \propto C^{\frac{\alpha_D}{\alpha_N + \alpha_D}} \qquad D_\text{opt} \propto C^{\frac{\alpha_N}{\alpha_N + \alpha_D}}
:::

::: legend
$N_\text{opt}$: the parameter count with the lowest loss for the budget
$D_\text{opt}$: the matching number of tokens, $C / (6 N_\text{opt})$
$C$: the training compute, in FLOPs
$\alpha_N$: the exponent of the parameter term
$\alpha_D$: the exponent of the data term
:::

::: derivation
Start from the loss with a floor.[Loss with a floor](reference:chinchilla-loss)
Write it as $L = L_\infty + A N^{-\alpha_N} + B D^{-\alpha_D}$ with $A = N_c^{\alpha_N}$ and $B = D_c^{\alpha_D}$.
Spend the budget: $D = C / (6N)$.[Training compute](reference:training-compute)
Substitute: $L(N) = L_\infty + A N^{-\alpha_N} + B \left(\frac{6N}{C}\right)^{\alpha_D}$.
Set the derivative to zero: $\frac{dL}{dN} = -\alpha_N A N^{-\alpha_N - 1} + \alpha_D B \left(\frac{6}{C}\right)^{\alpha_D} N^{\alpha_D - 1} = 0$.[Derivative](reference:derivative)
Multiply by $N$: $\alpha_N A N^{-\alpha_N} = \alpha_D B \left(\frac{6}{C}\right)^{\alpha_D} N^{\alpha_D}$.
Collect the powers of $N$: $N^{\alpha_N + \alpha_D} = \frac{\alpha_N A}{\alpha_D B} \left(\frac{C}{6}\right)^{\alpha_D}$.
Take the root: $N_\text{opt} \propto C^{\alpha_D / (\alpha_N + \alpha_D)}$.
Then $D_\text{opt} = C / (6 N_\text{opt}) \propto C^{1 - \alpha_D/(\alpha_N + \alpha_D)} = C^{\alpha_N / (\alpha_N + \alpha_D)}$. ∎
:::
:::

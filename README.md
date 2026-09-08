# Synaptic Plasticity as Short-Term Memory - an interactive explainer
### DataForge 2026 · Pathway Track submission

## The claim
A network whose connection strengths update online, after every input, using a
local Hebbian rule (Δw ∝ activity(i) × activity(j)) can store several recent
patterns directly in its weights and later reconstruct one from a partial cue -
but its capacity is limited, so storing too many patterns makes them
**interfere** and corrupts recall of all of them. Making the stored activity
**sparse and non-negative** - the choice Dragon Hatchling (BDH) makes -
measurably reduces that interference.

## Intended learner & prerequisites
- Audience: an ML/CS student or engineer who knows what a neural network
  weight matrix is and has heard of attention, but has not necessarily seen
  Hopfield networks, fast-weight programmers, or BDH before.
- Prerequisites: basic linear algebra (dot products, matrices); no RL, no
  training required to understand this artifact - nothing in it is trained.

## Learning objectives
By the end, the learner should be able to:
1. State, in their own words, what it means for "memory" to live in connection
   *weights* rather than in a token history or a cache.
2. Predict what happens to recall accuracy as more patterns are stored in the
   same fixed-size network, and explain *why* (interference / crosstalk).
3. Explain why constraining activity to be sparse and non-negative reduces
   that interference, and connect this to BDH's reported ~5% active,
   non-negative, monosemantic synapses.
4. Name at least one limitation of both the toy sandbox and of treating BDH's
   within-context memory as if it were durable, cross-session learning.

## Architecture of the artifact
Single self-contained HTML file (`hebbian-memory-explainer.html`), vanilla
JS + canvas, no external dependencies, no network calls, no build step, opens
directly in a browser.

- **Section 1 (store):** learner picks one of six hand-designed 7×5 bipolar
  icon patterns and stores it via a live outer-product Hebbian update
  `W ← λW + x·xᵀ`. The weight matrix is rendered as a live heatmap.
- **Section 2 (recall):** learner corrupts a chosen stored pattern by a
  controllable fraction and runs synchronous settling `x ← f(W·x)`, comparing
  the network's reconstruction against the ground-truth pattern side by side,
  with per-pixel overlap highlighting.
- **Section 3 (capacity sweep):** a real, freshly-computed-on-click Monte
  Carlo simulation (random patterns, 6 trials per point, 1-12 patterns
  stored) plotting recall accuracy in dense vs. sparse (k-winners-take-all)
  retrieval mode. Nothing here is precomputed - every run gives slightly
  different numbers because it uses fresh randomness, which is disclosed on
  the page.
- **BDH module:** grounded in the primary Dragon Hatchling paper and the
  BDH-CQ report; explicitly separates what is a real, cited claim about BDH
  from what is this sandbox's own simplified stand-in mechanism.

**Nothing in this artifact is precomputed, synthetic, or animated** - every
number the learner sees is computed live in the browser from the interaction
they just performed. The only thing explicitly labeled as illustrative rather
than official is the k-winners-take-all retrieval rule used for "sparse mode,"
which is a teaching simplification of BDH's own (differently defined)
sparsity mechanism, not a reproduction of it.

## Role of BDH / BDH-CQ
Dragon Hatchling replaces a Transformer's key-value cache with a synaptic
graph whose connection strengths are updated via a Hebbian-style rule as the
model reads - its working memory literally *is* its wiring at that moment.
The paper reports two properties this explainer directly targets:
sparse, non-negative activation (~5% of neurons active, varying with
predictability), and monosemantic synapses (individual connections
selective for one concept). BDH-CQ's contextual memory for in-context
learning is built from the same family of additive, per-demonstration update,
with adaptation living in recurrent state rather than in trained weights.
Full detail and sourcing is inside the artifact's "Where this actually shows
up in BDH" section and in the one-page concept summary PDF.

## What's live vs. illustrative
| Component | Status |
|---|---|
| Hebbian outer-product weight update | Live computation |
| Weight-matrix heatmap | Live rendering of the actual matrix |
| Corruption + settling/recall | Live computation, fresh randomness each run |
| Capacity sweep chart | Live Monte Carlo simulation, recomputed on click |
| BDH-specific numbers (∼5% active, monosemantic synapses, ARC-AGI results) | Cited facts from the primary papers, not reproduced or run by this artifact |
| "Sparse retrieval" (k-WTA) mechanism | Illustrative teaching simplification, explicitly labeled as not official BDH |

## How to reproduce / run
No install needed. Open `hebbian-memory-explainer.html` in any modern browser.
No server, no dependencies, no API keys, no data files.

## Primary papers (2022–2026) used or cited
1. Kosowski, Uznański, Chorowski, Stamirowska, Bartoszkiewicz — *The Dragon
   Hatchling: The Missing Link between the Transformer and Models of the
   Brain*, arXiv:2509.26507 (2025).
2. Pathway — *BDH-CQ: In-Context Learning with Recurrent Latent Reasoning*,
   arXiv:2608.09888 (2026).
3. Schlag, Irie, Schmidhuber — *Linear Transformers Are Secretly Fast Weight
   Programmers*, ICML / arXiv:2102.11174 (2021) — formal basis for treating
   an outer-product update as a Hebbian associative memory write. (Note:
   2021, included as the foundational reference for the mechanism generalized
   by the two 2022+ papers below.)
4. Irie, Schlag, Csordás, Schmidhuber — *A Modern Self-Referential Weight
   Matrix That Learns to Modify Itself*, ICML 2022.
5. Chaudhary — *Enabling Robust In-Context Memory and Rapid Task Adaptation
   in Transformers with Hebbian and Gradient-Based Plasticity*,
   arXiv:2510.21908 (2025).
6. *Hebbian Fast Weights in Vision Transformers for Few-Shot Learning*,
   arXiv:2605.02920 (2026).

## AI assistance disclosure
We conceptualized, designed, and coded this interactive explainer entirely from scratch. To make sure our explanations were as clear, engaging, and accessible as possible, we used AI to help brainstorm phrasing and polish the final text. That said, there is no AI hallucination in our math or our research-we personally read the primary papers, verified the technical claims, and stand completely behind the science presented here.

## Credits & licenses
-Code: 100% original vanilla HTML/JS/CSS. No third-party libraries were used.

-Art: The icons (Plus, X, Box, Arrow, Diamond, T) are our own hand-drawn 7×5 pixel designs.

-Concepts: Cited papers are linked below and are only paraphrased for educational purposes.

## Limitations (disclosed on the artifact itself too)
- 35 units is a toy scale; real associative-memory capacity scaling
  arguments don't fully transfer to a network this small.
- Synchronous settling can oscillate; the demo caps iterations and reports
  whatever it lands on.
- "Sparse retrieval" is a k-winners-take-all teaching stand-in, not BDH's
  actual activation function — the real mechanism and its exact reported
  statistics live only in the primary paper.

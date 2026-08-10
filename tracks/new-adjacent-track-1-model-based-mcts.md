# Track 1 (New & Adjacent): Model-based RL / MCTS variants for combinatorial or continuous control

Papers published within roughly the last 6-12 months on model-based RL and MCTS variants applied to combinatorial or continuous control, adjacent to the user's MuZero-for-power-grid-topology-control PhD work.

---

## 2026-08-10 (Monday) - Track 1: Model-based RL / MCTS variants for combinatorial or continuous control
**Title:** TransZero: Parallel Tree Expansion in MuZero using Transformer Networks
**Authors:** Emil Malmsten, Wendelin Böhmer
**Venue/Year:** BNAIC/BeNeLearn 2025 (Belgian-Dutch Conference on Artificial Intelligence / Benelux Conference on Machine Learning), accepted oral; arXiv preprint 2509.11233, September 2025 (Delft University of Technology)
**Link:** https://arxiv.org/abs/2509.11233

**Summary:** MuZero-style planning builds its search tree strictly sequentially: at each simulation, the recurrent dynamics network must unroll one latent state at a time before MCTS can descend further, and standard MCTS's PUCT action-selection formula depends on visitation counts that only exist once earlier simulations have completed - a hard sequential bottleneck that limits how much planning can be parallelized on modern hardware. TransZero removes both obstacles. First, it replaces MuZero's recurrent dynamics network with a transformer that, given a root latent state and a candidate action sequence, emits an entire sequence of future latent-state embeddings in a single forward pass rather than one step at a time. Second, it introduces a Mean-Variance Constrained (MVC) evaluator, an alternative to PUCT that scores actions from the mean and variance of value estimates instead of visitation counts, removing the sequential count-based dependency and letting whole subtrees be expanded and evaluated in parallel. Combined, these let TransZero build and evaluate multiple branches of the search tree simultaneously instead of node by node. On MiniGrid and LunarLander, TransZero matches MuZero's sample efficiency while achieving up to an eleven-fold wall-clock speedup during planning, showing that tree-search parallelism - not just neural-network parallelism - can be reclaimed from an architecture normally thought of as inherently sequential.

**Why this, why now:** This attacks the exact planning-time bottleneck the user's own MuZero-for-grid-topology-control work inherits - recurrent, one-node-at-a-time dynamics unrolling and count-based PUCT selection - by replacing both with a transformer dynamics model and a variance-based evaluator, offering a concrete architectural path to more simulations per wall-clock second without touching the underlying MDP. The MVC evaluator's shift from raw visitation counts to mean/variance-based action scoring also pairs naturally with the user's prior-head calibration and uncertainty-quantification front, since it treats planning-time action selection as an explicit uncertainty-estimation problem rather than a frequency count.

---

# Track 8 (old & foundational): Value equivalence and model-based RL theory

What a learned model for planning must actually capture: value equivalence, decision-aware model learning, and model-based RL theory

---

## 2026-08-27 (Thursday) - Track 8: Value equivalence and model-based RL theory
**Title:** The Value Equivalence Principle for Model-Based Reinforcement Learning
**Authors:** Christopher Grimm, André Barreto, Satinder Singh, David Silver
**Venue/Year:** Advances in Neural Information Processing Systems 33 (NeurIPS 2020); arXiv:2011.03506
**Link:** https://proceedings.neurips.cc/paper/2020/file/3bb585ea00014b0e3ebe4c6dd165a358-Paper.pdf

**Summary:** Model-based RL conventionally learns its model by maximum likelihood - fit the transition kernel to predict next states as accurately as possible, then plan inside it. The paper's argument is that this objective is misspecified whenever the model class cannot contain the true environment, which is always: a capacity-limited model can spend all its parameters on salient but decision-irrelevant dynamics and end up an excellent density estimator and a useless planner. The alternative is to define model equality only where planning actually looks. Two models are *value equivalent* with respect to a set of policies and a set of functions if they induce the same Bellman updates - if applying either model's Bellman operator for any policy in the set to any function in the set gives the same result. This replaces a single learning target with an equivalence class over model space. The core theory characterizes how that class behaves: it shrinks monotonically as the policy and function sets grow, collapsing onto the true model only when both are exhaustive, so every restriction of those sets is an explicit, quantified purchase of model simplicity. The authors then generalize to order-k value equivalence, requiring agreement after k applications of the Bellman operator, yielding a nested hierarchy that trades planning depth against model freedom. Practically, the VE loss replaces the MLE loss with a Bellman-residual-style objective; with neural transition models on catch and cart-pole, VE-trained models beat MLE-trained models of matched capacity, and the gap widens as capacity shrinks.

**Why this, why now:** This is the primary source for the result that a model used for planning does not have to predict the environment - it only has to induce the correct Bellman operators on the functions and policies that planning actually queries - which is the formal license for training a compact learned model in a domain like power grid topology control, where the underlying physics is far too rich to model densely and most of it never enters a planning decision. The order-k hierarchy is the useful practical object: it turns the trade-off between planning depth and model fidelity into an explicit design knob rather than an accident of whichever loss was chosen.

**Connection to other papers:** It is the theoretical justification, written after the fact, for a family of methods that had already abandoned reconstruction - value iteration networks, the Predictron, TreeQN and MuZero - showing they are all implicitly selecting a value-equivalent model rather than a likely one, and it therefore sits directly beneath the MuZero-derived work in the model-based/MCTS track (e.g. TransZero, logged 2026-08-10). It also inherits its machinery from the Bellman-residual line of Baird's "Residual Algorithms" (logged 2026-08-06): both replace a naive regression target with an operator-consistency criterion, Baird for the value function under fixed dynamics, Grimm et al. for the dynamics under a fixed set of value functions. The follow-up "Proper Value Equivalence" (Grimm et al., NeurIPS 2021) closes the hierarchy by taking k to infinity and showing that class is sufficient for optimal planning.

---



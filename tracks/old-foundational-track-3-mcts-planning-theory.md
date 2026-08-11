# Track 3 (old & foundational): MCTS and planning theory

UCT, bandit-based tree search, and the theoretical lineage behind AlphaGo/AlphaZero/MuZero-style planning.

---

## 2026-08-11 (Tuesday) - Track 3: MCTS and planning theory
**Title:** Bandit Based Monte-Carlo Planning
**Authors:** Levente Kocsis, Csaba Szepesvári
**Venue/Year:** European Conference on Machine Learning (ECML 2006), Springer LNCS vol. 4212, pp. 282-293
**Link:** https://doi.org/10.1007/11871842_29

**Summary:** Planning in large MDPs by expanding the full lookahead tree is intractable, and the natural alternative - Monte-Carlo rollouts from the current state - throws away structure by not concentrating samples on promising lines of play. Prior Monte-Carlo tree methods either wasted simulations on clearly-bad branches or relied on ad hoc heuristics for allocating search effort. This paper introduces UCT (UCB applied to Trees): each node in the search tree is itself treated as a multi-armed bandit problem, and the child to descend into at every level is chosen via the UCB1 rule, balancing the empirical mean return of an action against an exploration bonus shrinking with visit count. The tree is grown incrementally - a path is sampled root-to-leaf by recursively applying UCB1, a new node is expanded at the frontier, and a Monte-Carlo rollout (using a default/random policy) estimates the return, which is backpropagated to update every ancestor's statistics. Because a node's "arms" (its children's value estimates) keep drifting as descendants are explored, node-level regret is non-stationary and standard bandit regret bounds don't directly apply; the paper's core theoretical contribution is proving UCT is nonetheless consistent - the root value estimate and the induced action converge to optimal with high probability - and deriving finite-sample bounds on the convergence rate. Experiments on synthetic P-game trees and the stochastic "Sailing" shortest-path domain show UCT substantially outperforming alternative Monte-Carlo planning strategies.

**Why this, why now:** UCT is the literal ancestor of the tree-search core inside AlphaGo/AlphaZero/MuZero's PUCT variant - MuZero swaps UCT's raw rollout-based value/visit statistics for a learned value network, prior network, and dynamics model, but the exploration-exploitation node-selection rule and its consistency proof originate exactly here, so reading the primary source pins down which parts of MuZero's planning loop inherit a real theoretical guarantee (the UCB-style selection) versus which are unproven heuristic substitutions (learned priors and values standing in for rollout returns) - directly useful for reasoning rigorously about MuZero's search behavior on the power-grid topology-control problem.

---

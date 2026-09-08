# Track 3 (old & foundational): MCTS and planning theory

UCT, bandit-based tree search, and the theoretical lineage behind AlphaGo/AlphaZero/MuZero-style planning.

---

## 2026-09-08 (Tuesday) - Track 3: MCTS and planning theory
**Title:** A Sparse Sampling Algorithm for Near-Optimal Planning in Large Markov Decision Processes
**Authors:** Michael Kearns, Yishay Mansour, Andrew Y. Ng
**Venue/Year:** Machine Learning, vol. 49, no. 2-3, pp. 193-208, 2002 (original version: IJCAI-99)
**Link:** https://doi.org/10.1023/A:1017932429737

**Summary:** Classical planning and RL methods - value iteration, policy iteration, tabular Q-learning - have running times that scale at least linearly in the number of states, which rules them out for the enormous or continuous state spaces that arise in practice. This paper asks what is achievable when the only access to the MDP is a *generative model*: a black box that, given a state-action pair, returns a sampled next state and reward. The answer is a randomized online planning algorithm that, for a query state s, builds a sampled lookahead tree of depth H in which every node samples exactly C next states per action, and then backs values up recursively: Q-hat(s,a) is the immediate reward plus gamma times the empirical average of V-hat over the C sampled successors, with V-hat the max over actions. The action returned at the root is argmax Q-hat. The central result is that with H on the order of (1/(1-gamma)) log(V_max/epsilon) and C polynomial in V_max/epsilon and H, the returned action is epsilon-optimal with high probability - and the total number of generative-model calls, roughly (|A| C)^H, is completely independent of the size of the state space. The failure probability is controlled by a union bound over a tree whose per-level sampling error is compounded but geometrically discounted. The authors also prove a lower bound showing the exponential dependence on the horizon cannot be removed for algorithms in this access model, so the curse of dimensionality is traded for a curse of horizon rather than eliminated. No value function is stored; planning is per-state and on demand.

**Why this, why now:** The (|A| C)^H cost structure is the precise statement of why search depth in a large discrete action space is bought with the *per-node action branching factor*, which makes shrinking the candidate action set at each node - rather than deepening the tree - the load-bearing lever for tree search on combinatorial control problems such as power-grid topology control; it also makes explicit what a fixed simulation budget spread uniformly across actions buys you, which is the baseline any non-uniform or worker-parallel allocation of rollouts has to beat.

**Connection to other papers:** This is the direct theoretical predecessor of UCT (Kocsis and Szepesvari 2006, logged 2026-08-11): both assume only generative-model access and both achieve state-space-independent planning, but sparse sampling allocates a fixed C samples per action uniformly and gets a finite-sample worst-case guarantee, whereas UCT reallocates the same budget adaptively via UCB1 and gets asymptotic consistency instead. The lower bound here is also the reason later work attacks the horizon rather than the state count - via learned value functions truncating H, or model-approximation criteria such as the value-equivalence principle (logged 2026-08-27) that change what the sampled model must be accurate about.

---

## 2026-08-11 (Tuesday) - Track 3: MCTS and planning theory
**Title:** Bandit Based Monte-Carlo Planning
**Authors:** Levente Kocsis, Csaba Szepesvári
**Venue/Year:** European Conference on Machine Learning (ECML 2006), Springer LNCS vol. 4212, pp. 282-293
**Link:** https://doi.org/10.1007/11871842_29

**Summary:** Planning in large MDPs by expanding the full lookahead tree is intractable, and the natural alternative - Monte-Carlo rollouts from the current state - throws away structure by not concentrating samples on promising lines of play. Prior Monte-Carlo tree methods either wasted simulations on clearly-bad branches or relied on ad hoc heuristics for allocating search effort. This paper introduces UCT (UCB applied to Trees): each node in the search tree is itself treated as a multi-armed bandit problem, and the child to descend into at every level is chosen via the UCB1 rule, balancing the empirical mean return of an action against an exploration bonus shrinking with visit count. The tree is grown incrementally - a path is sampled root-to-leaf by recursively applying UCB1, a new node is expanded at the frontier, and a Monte-Carlo rollout (using a default/random policy) estimates the return, which is backpropagated to update every ancestor's statistics. Because a node's "arms" (its children's value estimates) keep drifting as descendants are explored, node-level regret is non-stationary and standard bandit regret bounds don't directly apply; the paper's core theoretical contribution is proving UCT is nonetheless consistent - the root value estimate and the induced action converge to optimal with high probability - and deriving finite-sample bounds on the convergence rate. Experiments on synthetic P-game trees and the stochastic "Sailing" shortest-path domain show UCT substantially outperforming alternative Monte-Carlo planning strategies.

**Why this, why now:** UCT is the literal ancestor of the tree-search core inside AlphaGo/AlphaZero/MuZero's PUCT variant - MuZero swaps UCT's raw rollout-based value/visit statistics for a learned value network, prior network, and dynamics model, but the exploration-exploitation node-selection rule and its consistency proof originate exactly here, so reading the primary source pins down which parts of MuZero's planning loop inherit a real theoretical guarantee (the UCB-style selection) versus which are unproven heuristic substitutions (learned priors and values standing in for rollout returns) - directly useful for reasoning rigorously about MuZero's search behavior on the power-grid topology-control problem.

---

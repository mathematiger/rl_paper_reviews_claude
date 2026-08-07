# Track 5 (new & adjacent): Graph-structured / infrastructure RL

Power grids, scheduling, and network control - Grid2Op/L2RPN-adjacent, Flatland-adjacent - viewed through graph-structured RL architectures neighboring the user's own MuZero/Grid2Op work.

---

## 2026-08-07 (Friday) - Track 5: Graph-structured / infrastructure RL
**Title:** Power Grid Control with Graph-Based Distributed Reinforcement Learning
**Authors:** Carlo Fabrizio, Gianvito Losapio, Marco Mussi, Alberto Maria Metelli, Marcello Restelli
**Venue/Year:** Workshop on Machine Learning for Sustainable Power Systems, ECML-PKDD 2025; arXiv preprint 2509.02861, September 2025 (Politecnico di Milano)
**Link:** https://arxiv.org/abs/2509.02861

**Summary:** Real-time power grid control faces a scaling problem: traditional human- and optimization-based dispatch struggles as renewable integration grows and networks expand, while most learned controllers (including standard Grid2Op agents) reason over the full centralized state and action space, which scales poorly. This paper proposes a graph-based, hierarchical multi-agent architecture: a network of distributed low-level agents, one per power line, is coordinated by a single high-level manager agent. Unlike prior decentralized RL work for grids, which typically factorizes only the action space while still conditioning each agent on a global observation, this method also factorizes the observation space - each low-level agent's local view is constructed via a Graph Neural Network (GNN) that encodes the surrounding topology, giving it a structured, informative but genuinely local observation rather than the whole grid state. To make this harder decentralized-credit-assignment problem learnable, the framework layers in imitation learning (bootstrapping from a rule-based/expert policy) and potential-based reward shaping (which provably preserves the optimal policy while densifying an otherwise sparse grid-control reward) to accelerate convergence and stabilize training. Evaluated on the Grid2Op simulator, the approach consistently outperforms the standard RL baseline used in the field and is markedly more computationally efficient at inference than the simulation-based "Expert" greedy-search agent, while retaining competitive control quality.

**Why this, why now:** This sits directly adjacent to the user's own Grid2Op/PPtopogym work but takes the opposite architectural route from MuZero - decentralized, hierarchical, GNN-structured local observations per line instead of a single centralized model-based planner - so it's a useful contrast point for how much of MuZero's representation-network job (encoding grid topology into a usable state) could instead be handled explicitly via graph structure, and its potential-based reward shaping component is a concrete, theoretically-grounded technique relevant to the user's reward-machine/non-Markovian-reward front for densifying Grid2Op's sparse reward without changing the optimal policy.

---

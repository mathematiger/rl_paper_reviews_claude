# Track 4 (New & Adjacent): Reward shaping / reward machines / non-Markovian rewards

Papers published within roughly the last 6-12 months on reward shaping, reward machines, or non-Markovian reward structures, adjacent to the user's reward-machines front for non-Markovian reward in Grid2Op-style environments.

---

## 2026-08-19 (Wednesday) - Track 4: Reward shaping / reward machines / non-Markovian rewards
**Title:** Pushdown Reward Machines for Reinforcement Learning
**Authors:** Giovanni Varricchione, Toryn Q. Klassen, Natasha Alechina, Mehdi Dastani, Brian Logan, Sheila A. McIlraith
**Venue/Year:** International Conference on Principles of Knowledge Representation and Reasoning (KR 2025); arXiv:2508.06894, August 2025
**Link:** https://arxiv.org/abs/2508.06894

**Summary:** Standard reward machines (RMs) encode non-Markovian reward as a finite-state automaton over an abstraction of the trajectory, which lets RL exploit task structure but caps expressiveness at regular languages - tasks needing unbounded counting or nested structure (e.g. "pick up exactly as many keys as doors remain," or balanced-bracket-style subtask nesting) cannot be represented exactly. This paper introduces pushdown reward machines (pdRMs), replacing the RM's finite automaton with a deterministic pushdown automaton, so the reward-triggering condition can depend on a stack in addition to the current automaton state - extending expressible non-Markovian reward from regular to deterministic context-free languages while keeping the same "automaton observes labelled trajectory, emits reward" interface RL algorithms already exploit. Because the stack is in principle unbounded, the authors define two policy classes: one with full access to the entire stack contents, and one restricted to only the top-k stack symbols (a bounded, practically learnable state representation). They give a formal procedure for checking when a top-k-restricted policy provably achieves the same optimal state values as the full-stack policy, characterizing exactly when the cheaper, bounded representation loses nothing. Experiments on tasks requiring nested/counting temporal structure show pdRM-augmented agents solving problems that plain finite-state RMs cannot represent, while the top-k variant matches full-stack performance whenever the equivalence condition holds.

**Why this, why now:** This is a direct expressiveness extension of the exact reward-machine formalism underlying the user's non-Markovian reward front - it identifies precisely which class of task structures (nested/counting behavior) a standard finite-automaton RM cannot represent and gives a principled, bounded-memory (top-k stack) way to add that expressiveness without discarding the automaton-state-conditioned reward interface that would need to plug into MuZero's planning loop.

---

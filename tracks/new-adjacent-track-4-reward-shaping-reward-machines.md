# Track 4 (New & Adjacent): Reward shaping / reward machines / non-Markovian rewards

Papers published within roughly the last 6-12 months on reward shaping, reward machines, or non-Markovian reward structures, adjacent to the user's reward-machines front for non-Markovian reward in Grid2Op-style environments.

---

## 2026-09-02 (Wednesday) - Track 4: Reward shaping / reward machines / non-Markovian rewards
**Title:** Reinforcement Learning with Symbolic Reward Machines
**Authors:** Thomas Krug, Daniel Neider (TU Dortmund University; Research Center Trustworthy Data Science and Security)
**Venue/Year:** arXiv preprint arXiv:2603.03068, March 2026
**Link:** https://arxiv.org/abs/2603.03068

**Summary:** Reward machines encode a non-Markovian reward as a finite automaton, but the automaton does not read the environment observation - it reads a stream of propositional labels produced by a *labeling function* that the practitioner must hand-write for every environment and every task. This paper argues that this labeling function, rather than the automaton, is the real barrier to using reward machines in practice: it is manual work per task, it presumes the designer already knows which high-level events matter, and it breaks the standard agent-environment interaction scheme, since a stock Gym-style environment emits observations and rewards, not labels. The proposal, Symbolic Reward Machines (SRMs), fuses the reward machine with a symbolic automaton: transitions are no longer triggered by abstract symbols from a fixed alphabet but by *guards*, symbolic formulas evaluated directly on the raw environment observation. The automaton state still carries the task memory (which subtask is done), so the non-Markovian reward structure and the state-augmentation trick that RL algorithms exploit both survive intact - only the abstraction layer changes. Two algorithms accompany the formalism: QSRM, which learns a policy against a given SRM while consuming only the standard environment output, so existing environments work out of the box; and LSRM, which automatically infers the SRM itself. Evaluated on OfficeWorld and WaterWorld, the SRM methods beat plain RL baselines and match label-based reward machine methods, while additionally yielding a human-readable description of the inferred task structure.

**Why this, why now:** The practical obstacle to putting a reward machine on a power grid control task is not automaton expressiveness but the labeling function - deciding, by hand, which predicates over a continuous, high-dimensional observation (line loadings, topology vectors, thermal margins) constitute the "events" the automaton transitions on. Guards as symbolic formulas over the raw observation are exactly that hand-crafted predicate layer made explicit and learnable, and LSRM's inference of the guards is the part that would remove the manual step.

**Connection to other papers:** It is the same formalism as Icarte et al.'s reward machines (JAIR 2022) with the labeling function dissolved into transition guards, and its LSRM half continues the automaton-inference line of Xu et al.'s JIRP. It is orthogonal to the pushdown reward machines logged on 2026-08-19: that work extends what an RM can *express* (regular to deterministic context-free) while keeping the labeling interface, whereas this one keeps regular expressiveness and attacks the interface itself. It also contrasts with the noisy-abstraction and neural-reward-machine line, which keeps the labeling function but makes it probabilistic or learned, rather than replacing it with symbolic guards over the observation.

---

## 2026-08-19 (Wednesday) - Track 4: Reward shaping / reward machines / non-Markovian rewards
**Title:** Pushdown Reward Machines for Reinforcement Learning
**Authors:** Giovanni Varricchione, Toryn Q. Klassen, Natasha Alechina, Mehdi Dastani, Brian Logan, Sheila A. McIlraith
**Venue/Year:** International Conference on Principles of Knowledge Representation and Reasoning (KR 2025); arXiv:2508.06894, August 2025
**Link:** https://arxiv.org/abs/2508.06894

**Summary:** Standard reward machines (RMs) encode non-Markovian reward as a finite-state automaton over an abstraction of the trajectory, which lets RL exploit task structure but caps expressiveness at regular languages - tasks needing unbounded counting or nested structure (e.g. "pick up exactly as many keys as doors remain," or balanced-bracket-style subtask nesting) cannot be represented exactly. This paper introduces pushdown reward machines (pdRMs), replacing the RM's finite automaton with a deterministic pushdown automaton, so the reward-triggering condition can depend on a stack in addition to the current automaton state - extending expressible non-Markovian reward from regular to deterministic context-free languages while keeping the same "automaton observes labelled trajectory, emits reward" interface RL algorithms already exploit. Because the stack is in principle unbounded, the authors define two policy classes: one with full access to the entire stack contents, and one restricted to only the top-k stack symbols (a bounded, practically learnable state representation). They give a formal procedure for checking when a top-k-restricted policy provably achieves the same optimal state values as the full-stack policy, characterizing exactly when the cheaper, bounded representation loses nothing. Experiments on tasks requiring nested/counting temporal structure show pdRM-augmented agents solving problems that plain finite-state RMs cannot represent, while the top-k variant matches full-stack performance whenever the equivalence condition holds.

**Why this, why now:** This is a direct expressiveness extension of the exact reward-machine formalism underlying the user's non-Markovian reward front - it identifies precisely which class of task structures (nested/counting behavior) a standard finite-automaton RM cannot represent and gives a principled, bounded-memory (top-k stack) way to add that expressiveness without discarding the automaton-state-conditioned reward interface that would need to plug into MuZero's planning loop.

---

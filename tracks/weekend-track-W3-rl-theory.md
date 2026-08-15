# Track W3 (weekend): RL theory with inspirational/deep theoretical background

Elegant proofs, surprising theoretical connections, and foundational frameworks in RL theory - curiosity-driven weekend reading, not tied to the user's active research fronts.

---

## 2026-08-15 (Saturday) - Track W3: RL theory with inspirational/deep theoretical background
**Title:** Linearly-Solvable Markov Decision Problems
**Authors:** Emanuel Todorov
**Venue/Year:** Advances in Neural Information Processing Systems 19 (NIPS 2006)
**Link:** https://papers.nips.cc/paper/3002-linearly-solvable-markov-decision-problems

**Summary:** Standard MDPs are solved via the Bellman optimality equation, which is nonlinear because of the max/argmax over actions - this is exactly what forces iterative fixed-point solvers (value/policy iteration) instead of direct linear algebra, and what makes reusing solutions across related tasks hard. Todorov identifies a broad, useful subclass where this nonlinearity vanishes entirely. Instead of choosing discrete actions, the controller in a linearly-solvable MDP (LMDP) directly reshapes the transition probabilities of a given passive/uncontrolled Markov chain, paying a control cost equal to the KL divergence between controlled and uncontrolled next-state distributions. Because KL divergence is convex, the per-state minimization over next-state distributions admits a closed form; substituting it back into the Bellman equation removes the max altogether. Applying the exponential transform z = exp(-v/lambda) to the optimal value function then turns the Bellman equation into a genuinely linear eigenvector problem: the resulting "desirability function" z is the principal eigenvector of a matrix combining the uncontrolled dynamics with exponentiated state costs, solvable by standard linear algebra (e.g. power iteration) rather than dynamic programming. This yields an O(n) algorithm for shortest-path-like tasks via a largest-eigenvalue computation, and - because desirabilities combine linearly - lets solutions to elementary tasks be linearly recombined into solutions for new composite tasks without re-solving the MDP.

**Why this, why now:** One of the most elegant results in RL theory: reframing control as reshaping a Markov chain under a KL-divergence cost converts the notoriously nonlinear Bellman optimality equation into a linear eigenvector problem that is closed-form and compositional. It seeded an entire lineage - path-integral control, KL-control, and the control-as-inference view underlying maximum-entropy RL (e.g. soft actor-critic) - by showing that the "hard" nonlinearity in optimal control is an artifact of the action parameterization rather than something fundamental to the problem.

---

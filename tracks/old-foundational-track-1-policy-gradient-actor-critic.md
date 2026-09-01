# Track 1 (old & foundational): Policy gradient / actor-critic foundations

Primary sources behind policy gradient methods and actor-critic architectures: the policy gradient theorem, compatible function approximation, and actor-critic convergence results.

---

## 2026-09-01 (Tuesday) - Track 1: Policy gradient / actor-critic foundations
**Title:** Actor-Critic Algorithms
**Authors:** Vijay R. Konda, John N. Tsitsiklis
**Venue/Year:** Advances in Neural Information Processing Systems 12 (NIPS 1999), MIT Press, 2000, pp. 1008-1014
**Link:** https://proceedings.neurips.cc/paper/1999/hash/6449f44a102fde848669bdd9eb6b76fa-Abstract.html

**Summary:** Before this paper, the two halves of policy optimisation each had a known defect. Actor-only methods estimate the gradient from Monte Carlo returns, so their variance grows with the horizon and every update discards the previous one's estimate. Critic-only methods learn a value function by TD with function approximation, but a good approximate value function carries no guarantee that the greedy policy read off it is any good. Actor-critic schemes had combined the two heuristically since Barto-Sutton-Anderson (1983) without a convergence proof under function approximation. Konda and Tsitsiklis supply one. Working in the average-reward setting over a parameterised family of randomised stationary policies mu_theta, with general (finite, countable, or continuous) state and action spaces, they show the gradient of average reward is an inner product <Q_theta, psi_theta> under the stationary state-action distribution, where psi_theta(x,u) = grad_theta log mu_theta(u|x). The consequence is the paper's central design insight: the actor only ever consumes the *projection* of Q onto span{psi_theta}, so the critic need not approximate Q well globally - its features should simply span the subspace that the actor's own parameterisation prescribes. Architecturally this yields a two-timescale stochastic approximation: a linear TD(lambda) critic on the psi features updated on the fast timescale, and an actor stepping along the critic's gradient estimate on the slow timescale, so the critic always sees a quasi-static policy. Two convergence theorems follow - the TD(1) variant converges to a genuine stationary point of average reward, while for lambda < 1 the iterates settle into a neighbourhood whose size is governed by the TD approximation error and shrinks as lambda approaches 1.

**Why this, why now:** This is the primary source for *why* an actor-critic architecture converges at all under function approximation, and its subspace argument says something sharp and practical: the critic's job is only to be accurate along the directions the actor can actually move in, which is the theoretical reason a critic that looks mediocre by value-error metrics can still drive good policy improvement on a large structured action space such as power grid topology control. It is also the exact theoretical counterpoint to gradient-free population-based policy search, which needs no critic and no projection argument but buys that simplicity with sample complexity.

**Connection to other papers:** It is the convergence-theory sibling of Sutton, McAllester, Singh and Mansour's policy gradient theorem paper (logged 2026-08-04, same NIPS 1999 proceedings): both arrive independently at the compatible-features condition, but Sutton et al. present it as a bias-free substitution result while Konda and Tsitsiklis derive it as an orthogonal-projection statement and pair it with an actual two-timescale stochastic-approximation proof. Its constraint on the critic's feature space is what Kakade's natural policy gradient (2001) later reinterprets geometrically via the Fisher metric, and the two-timescale actor-critic template it certifies is the direct ancestor of the value-head-plus-policy-head architectures used throughout modern deep RL.

---


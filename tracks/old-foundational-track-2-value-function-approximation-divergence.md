# Track 2 (old & foundational): Value-based RL foundations and function-approximation divergence results

Classic results on why value-based RL with function approximation can fail - divergence, the deadly triad, and the fixes proposed for them.

---

## 2026-08-06 (Thursday) - Track 2: Value-based RL foundations and function-approximation divergence results
**Title:** Residual Algorithms: Reinforcement Learning with Function Approximation
**Authors:** Leemon C. Baird III
**Venue/Year:** Proceedings of the Twelfth International Conference on Machine Learning (ICML 1995), Tahoe City, CA, July 9-12, 1995, pp. 30-37
**Link:** http://leemon.com/papers/1995b.pdf (ACM DL record: https://dl.acm.org/doi/10.5555/3091622.3091627)

**Summary:** Before this paper, it was folklore that combining bootstrapped TD-style updates with function approximation could be unstable, but the failure mode was not pinned down constructively. Baird supplies a minimal, concrete counterexample: a seven-state, two-action MDP with a 15-parameter linear value approximator, where an exact solution to the Bellman equations exists yet standard Q-learning/TD updates diverge to infinity from almost any starting weights - even though the transition dynamics are simple and fully known. The culprit is identified as the combination of bootstrapping, off-policy-style updating, and function approximation (later popularized as the "deadly triad"): the update direction used by direct TD methods is not the gradient of any objective, so nothing guarantees the weights stay bounded. Baird's fix is the "residual gradient" family: instead of the semi-gradient TD update, perform true stochastic gradient descent on the mean-squared Bellman residual, using two independent samples of the successor state (a "double sampling" trick) to obtain an unbiased gradient estimate. This is provably convergent under standard stochastic-approximation conditions, at the cost of converging to a different, sometimes worse, fixed point than TD and being noticeably slower in practice. The paper also proposes a "residual" interpolation between the direct and residual-gradient updates to partially recover speed while retaining stability guarantees.

**Why this, why now:** This is the primal source for why MuZero's learned value/reward heads are not automatically safe just because they're trained by gradient descent - the paper's counterexample isolates exactly the bootstrapping + function-approximation combination MuZero's TD-style value targets rely on, giving formal grounding for the prior-head calibration diagnostics (ECE) and recalibration front, since divergence/instability in the underlying value estimates is precisely what calibration checks are meant to catch downstream.

---

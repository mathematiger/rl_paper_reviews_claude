# Paper Curriculum Log

Daily research-paper picks for PhD work on MuZero applied to power grid
topology control (Grid2Op/L2RPN). Monday/Wednesday/Friday = new & adjacent
papers; Tuesday/Thursday = old & foundational papers. Most recent entry
first.

---

## 2026-08-06 (Thursday) - Track 2: Value-based RL foundations and function-approximation divergence results
**Title:** Residual Algorithms: Reinforcement Learning with Function Approximation
**Authors:** Leemon C. Baird III
**Venue/Year:** Proceedings of the Twelfth International Conference on Machine Learning (ICML 1995), Tahoe City, CA, July 9-12, 1995, pp. 30-37
**Link:** http://leemon.com/papers/1995b.pdf (ACM DL record: https://dl.acm.org/doi/10.5555/3091622.3091627)

**Summary:** Before this paper, it was folklore that combining bootstrapped TD-style updates with function approximation could be unstable, but the failure mode was not pinned down constructively. Baird supplies a minimal, concrete counterexample: a seven-state, two-action MDP with a 15-parameter linear value approximator, where an exact solution to the Bellman equations exists yet standard Q-learning/TD updates diverge to infinity from almost any starting weights - even though the transition dynamics are simple and fully known. The culprit is identified as the combination of bootstrapping, off-policy-style updating, and function approximation (later popularized as the "deadly triad"): the update direction used by direct TD methods is not the gradient of any objective, so nothing guarantees the weights stay bounded. Baird's fix is the "residual gradient" family: instead of the semi-gradient TD update, perform true stochastic gradient descent on the mean-squared Bellman residual, using two independent samples of the successor state (a "double sampling" trick) to obtain an unbiased gradient estimate. This is provably convergent under standard stochastic-approximation conditions, at the cost of converging to a different, sometimes worse, fixed point than TD and being noticeably slower in practice. The paper also proposes a "residual" interpolation between the direct and residual-gradient updates to partially recover speed while retaining stability guarantees.

**Why this, why now:** This is the primal source for why MuZero's learned value/reward heads are not automatically safe just because they're trained by gradient descent - the paper's counterexample isolates exactly the bootstrapping + function-approximation combination MuZero's TD-style value targets rely on, giving formal grounding for the prior-head calibration diagnostics (ECE) and recalibration front, since divergence/instability in the underlying value estimates is precisely what calibration checks are meant to catch downstream.

---

## 2026-08-05 (Wednesday) - Track 4: Reward shaping / reward machines / non-Markovian rewards
**Title:** Model-Based Reinforcement Learning in Discrete-Action Non-Markovian Reward Decision Processes
**Authors:** Alessandro Trapasso, Luca Iocchi, Fabio Patrizi
**Venue/Year:** arXiv preprint 2512.14617, December 2025 (submitted to AAAI 2026); Sapienza University of Rome / Fondazione Bruno Kessler
**Link:** https://arxiv.org/abs/2512.14617

**Summary:** Non-Markovian Reward Decision Processes (NMRDPs) generalize MDPs by letting the reward depend on the whole history rather than just the current state-action pair - the natural setting once a reward machine (a finite automaton tracking task progress) is used to encode temporally-extended objectives. Prior work mostly handled this by flattening the problem into the automaton-augmented "product MDP" and running standard model-free RL on it, discarding the structural separation between environment dynamics and task/reward dynamics and losing sample-efficiency guarantees. This paper introduces QR-Max, a model-based algorithm that explicitly factorizes learning into two optimistic (R-Max-style) models - one over the environment's Markovian transitions, one over the reward machine's automaton transitions - and shows this factorization yields the first PAC-MDP guarantee for discrete-action NMRDPs: with high probability the agent reaches an epsilon-optimal policy within a number of steps polynomial in the relevant problem parameters, rather than needing to learn the full unfactored product transition function. The authors also present Bucket-QR-Max, extending the approach to continuous state spaces via a SimHash-based discretizer that preserves the same factorized optimistic-exploration structure without hand-tuned gridding or function approximation, and show faster, more stable learning than flattened baselines.

**Why this, why now:** This is a direct hit on the user's reward-machines front - it gives a model-based (R-Max-style optimism, product-MDP) route to provably sample-efficient learning under non-Markovian reward exactly by factorizing "environment model" from "task/automaton model," which is structurally the same factorization MuZero already performs (learned dynamics model vs. learned value/reward prediction) and suggests a concrete way to attach reward-machine automaton state to MuZero's planning loop with sample-efficiency guarantees rather than ad hoc reward shaping.

---

## 2026-08-04 (Tuesday) - Track 1: Policy gradient / actor-critic foundations
**Title:** Policy Gradient Methods for Reinforcement Learning with Function Approximation
**Authors:** Richard S. Sutton, David McAllester, Satinder Singh, Yishay Mansour
**Venue/Year:** Advances in Neural Information Processing Systems 12 (NIPS 1999), MIT Press, 2000
**Link:** https://proceedings.neurips.cc/paper/1999/file/464d828b85b0bed98e80ade0a5c43b0f-Paper.pdf

**Summary:** Value-based RL with function approximation had a known failure mode: small changes in estimated action values can flip a near-deterministic greedy policy discontinuously, which can prevent convergence or cause oscillation/divergence when combined with function approximation. This paper instead represents the policy directly as its own differentiable function approximator and updates its parameters by gradient ascent on expected reward. The central result, the policy gradient theorem, shows the gradient of expected reward decomposes into a sum over states (weighted by the policy's stationary distribution) of the score function ∇log π(a|s) times the true action-value Qπ(s,a) - notably with no dependence on the gradient of the state-distribution itself, which would otherwise be intractable. The paper then proves a compatible function approximation result: if a critic approximating Qπ is linear in the same features as ∇log π and fit by minimizing mean-squared error, substituting it for the true Qπ introduces no bias into the gradient estimate. Architecturally this yields the modern actor-critic template - a parametrized actor updated via the policy gradient, paired with a compatible-features linear critic trained by TD - and the paper proves this converges to a locally optimal policy, generalizing and formalizing Williams's REINFORCE and earlier heuristic actor-critic schemes.

**Why this, why now:** This is the primary source underpinning every actor-critic method the user's standing interests touch, and it also frames the CMA-ES-vs-MuZero comparison precisely: this paper's gradient-based policy improvement (using ∇log π and a compatible critic) is the theoretical counterpoint to the gradient-free, population-based search CMA-ES performs on the same policy-parameter space.

---

## 2026-08-03 (Monday) - Track 2: Conformal prediction for sequential decision-making
**Title:** Conformal Policy Control
**Authors:** Drew Prinster, Clara Fannjiang, Ji Won Park, Kyunghyun Cho, Anqi Liu, Suchi Saria, Samuel Stanton
**Venue/Year:** arXiv preprint 2603.02196, March 2026
**Link:** https://arxiv.org/abs/2603.02196

**Summary:** The paper tackles safe exploration: an agent has a trusted, safe reference policy and wants to deploy a new, optimized-but-untested policy that may perform better but hasn't been validated for safety. Prior conservative approaches either require committing to a specific model class and hand-tuned hyperparameters, or only give guarantees for monotonic bounded loss/constraint functions - a poor fit for many real risk metrics. The authors introduce Conformal Policy Control (CPC), which treats the safe reference policy as a probabilistic regulator over the candidate policy. Using held-out rollout data from the safe policy, split-conformal calibration is used to compute an interpolation/mixing coefficient between the safe and candidate policies such that the deployed mixture provably respects a user-declared risk threshold with finite-sample, distribution-free guarantees - critically, the theory extends conformal risk control to non-monotonic bounded loss functions, broadening applicability beyond earlier conformal-safety methods. No assumptions about the candidate policy's model class or internal hyperparameters are needed; only black-box evaluation on the calibration rollouts. The result is a general recipe for turning any safe policy + any candidate policy into a risk-controlled deployment policy, letting an operator dial exploration aggressiveness against a formal safety budget.

**Why this, why now:** This is a direct extension of the user's conformal-prediction front (split-conformal, APS/RAPS for recall-maximizing candidate action selection) from calibrating *prediction sets* to calibrating *policy deployment itself* - the same split-conformal machinery could gate MuZero's candidate topology-change actions against a safe "do-nothing" fallback with a provable finite-sample risk bound, rather than only filtering the action-prior via conformal sets.

---

## 2026-07-31 (Friday) - Track 6: Exploration strategies (evolutionary baselines vs RL)
**Title:** Evolution Strategies at Scale: LLM Fine-Tuning Beyond Reinforcement Learning
**Authors:** Xin Qiu, Yulu Gan, Conor F. Hayes, Qiyao Liang, Yinggan Xu, Roberto Dailey, Elliot Meyerson, Babak Hodjat, Risto Miikkulainen
**Venue/Year:** arXiv preprint 2509.24372, September 2025 (Cognizant AI Lab / UT Austin)
**Link:** https://arxiv.org/abs/2509.24372

**Summary:** RL fine-tuning of LLMs (PPO/GRPO-style methods) suffers from high-variance policy-gradient estimates, sensitivity to long-horizon or delayed rewards, reward hacking, and run-to-run instability. Evolution Strategies (ES) is a gradient-free, population-based alternative, but has been dismissed as unable to scale to billion-parameter models, since naive ES requires materializing a full parameter perturbation per population member. The authors introduce a memory-efficient, algorithmically simplified ES variant that reconstructs perturbations from RNG seeds on the fly (the classic ES seed trick) rather than storing them, and build a distributed system on Ray (orchestration) and vLLM (rollout inference) so population evaluation parallelizes across many GPUs with no backpropagation, optimizer states, or stored activations. This gives the first successful full-parameter ES fine-tuning of LLMs up to 8B parameters without any dimensionality reduction. Across benchmarks, ES matches or beats PPO/GRPO baselines while being markedly more sample-efficient in some settings, more tolerant of long-horizon/delayed rewards, more robust across different base models, less prone to reward hacking, and more stable across seeds. Architecturally: a central server broadcasts current weights plus seeds, workers perturb-and-evaluate via vLLM, and the server aggregates returns through a rank-weighted ES update - a zeroth-order optimizer standing in for the policy-gradient step.

**Why this, why now:** The user is building a CMA-ES baseline against MuZero for grid topology control, and this paper runs exactly that comparison - population-based, gradient-free black-box optimization vs. gradient-based RL - at scale, isolating precisely the axes (sample efficiency, reward-hacking susceptibility, stability, tolerance to delayed/sparse reward) that matter for judging whether CMA-ES is competitive with MuZero on Grid2Op's sparse, cascading-failure-driven reward signal.

---

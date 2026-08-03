# Paper Curriculum Log

Daily research-paper picks for PhD work on MuZero applied to power grid
topology control (Grid2Op/L2RPN). Monday/Wednesday/Friday = new & adjacent
papers; Tuesday/Thursday = old & foundational papers. Most recent entry
first.

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

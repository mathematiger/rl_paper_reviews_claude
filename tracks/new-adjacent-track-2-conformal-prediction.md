# Track 2 (New & Adjacent): Conformal prediction for sequential decision-making

Papers published within roughly the last 6-12 months applying conformal prediction to sequential decision-making or RL, adjacent to the user's recall-maximizing candidate action selection front (split-conformal, APS/RAPS) and calibrated probabilistic models.

---

## 2026-08-31 (Monday) - Track 2: Conformal prediction for sequential decision-making
**Title:** Conformal Prediction Beyond the Horizon: Distribution-Free Inference for Policy Evaluation
**Authors:** Feichen Gan, Youcun Lu, Yingying Zhang, Yukun Liu
**Venue/Year:** NeurIPS 2025 (poster); arXiv:2510.26026, October 2025
**Link:** https://arxiv.org/abs/2510.26026

**Summary:** Policy evaluation usually reports a point estimate of the expected return, but a decision maker who cares about downside risk wants an interval around the return that an individual trajectory will actually realize. Conformal prediction supplies exactly that kind of distribution-free interval, yet its exchangeability assumption fails hard in the infinite-horizon RL setting: the target quantity (the discounted return from a state) is never observed in finite data, transitions inside a trajectory are temporally dependent, and off-policy evaluation adds a distribution shift between the behaviour policy that generated the calibration data and the target policy being evaluated. This paper builds a unified conformal framework that handles all three obstacles for on-policy and off-policy infinite-horizon evaluation. The architecture has two modular pieces. First, a *pseudo-return* construction: the unobserved infinite-horizon return is replaced by a truncated rollout of observed rewards plus a bootstrapped tail supplied by a distributional RL model, so the conformity score has a concrete target whose truncation bias is controllable by horizon length. Second, a *time-aware calibration* strategy that draws calibration points from experience replay via weighted subsampling, thinning temporally adjacent samples and reweighting for the policy shift so that approximate exchangeability is restored. The theory gives coverage lower bounds that explicitly absorb both distributional-model misspecification and importance-weight estimation error, quantified in Wasserstein distance rather than assumed away. Experiments on synthetic MDPs and Mountain Car show markedly better empirical coverage than intervals read directly off a distributional RL head.

**Why this, why now:** The load-bearing mechanism here is calibrating a learned return/value head with split-conformal machinery on data that is temporally dependent and off-policy — the same obstacle that arises when calibration data comes from a replay buffer of correlated grid trajectories rather than an i.i.d. holdout, and the paper's weighted-subsampling fix plus its misspecification-aware coverage bound are directly reusable there.

**Connection to other papers:** It sits downstream of Zhang et al.'s *Conformal Off-Policy Prediction* (AISTATS 2023) and the finite-horizon *Conformal Off-Policy Evaluation in MDPs* line, extending them from finite horizons and observed outcomes to the infinite-horizon case via the pseudo-return trick. It is complementary to the action-conditional risk-averse work logged on 2026-08-17: that paper conditions the guarantee on the action to shape a feasible decision set, whereas this one conditions on a state and calibrates the return distribution itself.

---

## 2026-08-17 (Monday) - Track 2: Conformal prediction for sequential decision-making
**Title:** Conformal Risk-Averse Decision Making with Action Conditional Guarantee
**Authors:** Zihan Zhu, Shayan Kiyani, George Pappas, Hamed Hassani
**Venue/Year:** International Conference on Machine Learning (ICML 2026); arXiv:2606.05551, June 2026 (University of Pennsylvania)
**Link:** https://arxiv.org/abs/2606.05551

**Summary:** Conformal prediction wraps a model's output into a prediction set with a distribution-free coverage guarantee, and recent decision-theoretic work (Kiyani et al., ICML 2025) showed such sets can be translated into optimal risk-averse decision policies - but only with a *marginal* safety guarantee, averaged over the whole action space. A policy built this way can badly under-protect on some actions while wastefully over-protecting on others, since the marginal average hides per-action violations. This paper introduces action-conditional conformal prediction: instead of one pooled coverage guarantee, the prediction-set construction is conditioned explicitly on the action the decision maker is about to take, so the distribution-free coverage guarantee holds separately for each action rather than only on average across all of them. The authors show these action-conditional prediction sets act as a tractable proxy for the feasible decision region of an agent optimizing *action-conditional* value-at-risk, letting different actions carry their own appropriately calibrated risk budget instead of sharing one global bound. To fit this from finite calibration data without assuming a parametric loss model, they derive a principled estimator based on minimizing the pinball (quantile) loss per action, extending conditional-validity conformal techniques to this action-conditional setting. The paper positions this as closing a real gap in the Kiyani et al. framework: marginal guarantees can pass in aggregate while silently failing on specific, possibly safety-critical, actions.

**Why this, why now:** This extends the exact mechanism behind the user's recall-maximizing candidate action selection front - conformal-set construction (split-conformal/APS/RAPS) gating which actions are admitted - from a single pooled coverage/recall guarantee over the whole action space to a guarantee conditioned on each candidate action individually, which matters directly for Grid2Op's large, heterogeneous discrete action space where rare topology-change actions could otherwise be silently under-covered by a marginal-only conformal set even as the aggregate recall target is met.

---

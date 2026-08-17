# Track 2 (New & Adjacent): Conformal prediction for sequential decision-making

Papers published within roughly the last 6-12 months applying conformal prediction to sequential decision-making or RL, adjacent to the user's recall-maximizing candidate action selection front (split-conformal, APS/RAPS) and calibrated probabilistic models.

---

## 2026-08-17 (Monday) - Track 2: Conformal prediction for sequential decision-making
**Title:** Conformal Risk-Averse Decision Making with Action Conditional Guarantee
**Authors:** Zihan Zhu, Shayan Kiyani, George Pappas, Hamed Hassani
**Venue/Year:** International Conference on Machine Learning (ICML 2026); arXiv:2606.05551, June 2026 (University of Pennsylvania)
**Link:** https://arxiv.org/abs/2606.05551

**Summary:** Conformal prediction wraps a model's output into a prediction set with a distribution-free coverage guarantee, and recent decision-theoretic work (Kiyani et al., ICML 2025) showed such sets can be translated into optimal risk-averse decision policies - but only with a *marginal* safety guarantee, averaged over the whole action space. A policy built this way can badly under-protect on some actions while wastefully over-protecting on others, since the marginal average hides per-action violations. This paper introduces action-conditional conformal prediction: instead of one pooled coverage guarantee, the prediction-set construction is conditioned explicitly on the action the decision maker is about to take, so the distribution-free coverage guarantee holds separately for each action rather than only on average across all of them. The authors show these action-conditional prediction sets act as a tractable proxy for the feasible decision region of an agent optimizing *action-conditional* value-at-risk, letting different actions carry their own appropriately calibrated risk budget instead of sharing one global bound. To fit this from finite calibration data without assuming a parametric loss model, they derive a principled estimator based on minimizing the pinball (quantile) loss per action, extending conditional-validity conformal techniques to this action-conditional setting. The paper positions this as closing a real gap in the Kiyani et al. framework: marginal guarantees can pass in aggregate while silently failing on specific, possibly safety-critical, actions.

**Why this, why now:** This extends the exact mechanism behind the user's recall-maximizing candidate action selection front - conformal-set construction (split-conformal/APS/RAPS) gating which actions are admitted - from a single pooled coverage/recall guarantee over the whole action space to a guarantee conditioned on each candidate action individually, which matters directly for Grid2Op's large, heterogeneous discrete action space where rare topology-change actions could otherwise be silently under-covered by a marginal-only conformal set even as the aggregate recall target is met.

---

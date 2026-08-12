# Track 3 (New & Adjacent): Calibration of learned value/policy heads, uncertainty quantification in deep RL

Papers published within roughly the last 6-12 months on calibration of learned value/policy heads and uncertainty quantification in deep RL, adjacent to the user's MuZero-for-power-grid-topology-control PhD work.

---

## 2026-08-12 (Wednesday) - Track 3: Calibration of learned value/policy heads, uncertainty quantification in deep RL
**Title:** Auditing the Risk Claims of Distributional Reinforcement Learning
**Authors:** Hari Prasad
**Venue/Year:** arXiv preprint 2607.11607, July 2026
**Link:** https://arxiv.org/abs/2607.11607

**Summary:** Distributional RL methods (QR-DQN, C51, IQN) learn full return distributions that are increasingly read at face value - for interpretability, risk-sensitive action selection (e.g. CVaR-greedy control), and safety monitoring - but this paper asks whether those risk claims actually hold, or are just plausible-looking training artifacts. The audit combines three pieces: a decision-relevant screening metric, the "excess Wasserstein gap" between the top two actions' predicted return distributions, which quantifies the probability mass by which first-order stochastic dominance is violated; ground truth obtained via snapshot-restart Monte Carlo rollouts in the actual environment; and a statistical harness (permutation nulls, bootstrap refutation, false-discovery-rate control) that the paper argues is necessary because without it the audit itself manufactures false conclusions. Applied across QR-DQN, C51, and IQN on MinAtar (33 runs, including a pretrained 10M-frame checkpoint), 40-95% of the strongest claimed risk trade-offs are refuted at 95% confidence, and where a risk claim is placed is statistically indistinguishable from a truth-blind baseline - essentially no individual claim is confirmable. Results are environment-dependent: the QR-DQN head is genuinely informative in Breakout but anti-predictive in Seaquest. Ensembling attenuates but does not calibrate the estimates, and post-hoc recalibration only "passes" the audit by shrinking claims to non-informative.

**Why this, why now:** This is a direct methodological warning for the user's own prior/value-head calibration diagnostics (ECE) and post-hoc affine/isotonic recalibration front - the paper's central finding, that naive recalibration can "pass" a surface-level calibration check only by nullifying the very risk information it's meant to preserve, argues for auditing recalibrated MuZero value/prior-head outputs against ground-truth rollouts rather than trusting an ECE-style metric alone before feeding them into conformal-prediction-based action selection.

---

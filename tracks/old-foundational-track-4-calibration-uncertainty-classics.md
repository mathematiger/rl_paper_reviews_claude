# Track 4 (old & foundational): Calibration and uncertainty classics

Platt scaling, isotonic/Bayesian calibration, and modern neural-net calibration diagnostics - the classic papers behind confidence calibration.

---

## 2026-08-13 (Thursday) - Track 4: Calibration and uncertainty classics
**Title:** On Calibration of Modern Neural Networks
**Authors:** Chuan Guo, Geoff Pleiss, Yu Sun, Kilian Q. Weinberger
**Venue/Year:** Proceedings of the 34th International Conference on Machine Learning (ICML 2017), PMLR vol. 70, pp. 1321-1330; arXiv:1706.04599, June 2017
**Link:** https://arxiv.org/abs/1706.04599

**Summary:** A classifier's confidence should track its true probability of being correct, and older, shallower networks (e.g. LeNet) were reasonably well calibrated in this sense. This paper shows that modern deep networks (ResNets, wide/deep highway nets) are not: despite higher accuracy, they are systematically overconfident, and the paper isolates depth, width, reduced weight decay, and batch normalization as the architectural/training factors that drive miscalibration up even as accuracy improves - accuracy and calibration have become decoupled. The paper formalizes and popularizes Expected Calibration Error (ECE), a binned summary of the gap between predicted confidence and empirical accuracy visualized via reliability diagrams, as the now-standard scalar diagnostic for this gap. It then benchmarks post-hoc calibration methods - histogram binning, isotonic regression, Bayesian Binning into Quantiles, Platt/matrix/vector scaling - against a method the authors introduce, temperature scaling: a single scalar T dividing the pre-softmax logits, fit by minimizing NLL on held-out validation data, which leaves the model's predictions (argmax, accuracy) exactly unchanged while sharply reducing ECE. Despite having only one fitted parameter, temperature scaling matches or beats every more expressive alternative across vision and NLP benchmarks, showing that modern overconfidence is close to a uniform logit-scale miscalibration rather than a shape distortion needing nonparametric correction.

**Why this, why now:** This is the origin paper for both the ECE diagnostic and the affine-style post-hoc recalibration the user already runs on MuZero's prior/value heads - temperature scaling is exactly the single-parameter special case of the affine recalibration family that the TMLR "Taxonomy of Calibration" paper's beta-calibration-as-GLM framing generalizes, making this the primary source for why a one-parameter fix can close most of the overconfidence gap that ECE measures.

---

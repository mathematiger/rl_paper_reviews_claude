# Track 4 (old & foundational): Calibration and uncertainty classics

Platt scaling, isotonic/Bayesian calibration, and modern neural-net calibration diagnostics - the classic papers behind confidence calibration.

---

## 2026-09-10 (Thursday) - Track 4: Calibration and uncertainty classics
**Title:** Beyond temperature scaling: Obtaining well-calibrated multi-class probabilities with Dirichlet calibration
**Authors:** Meelis Kull, Miquel Perelló-Nieto, Markus Kängsepp, Telmo Silva Filho, Hao Song, Peter Flach
**Venue/Year:** Advances in Neural Information Processing Systems 32 (NeurIPS 2019); arXiv:1910.12656, October 2019
**Link:** https://proceedings.neurips.cc/paper/2019/hash/8ca01ea920679a0fe3728441494041b9-Abstract.html (arXiv: https://arxiv.org/abs/1910.12656)

**Summary** (150-250 words): "Calibrated" is not one property once there are more than two classes, and this paper makes that precise. It separates *confidence calibration* (the top-1 probability matches top-1 accuracy — what standard ECE measures), *classwise calibration* (every coordinate of the predicted vector matches its class frequency), and full *multiclass/canonical calibration* (the entire probability vector is correct), showing these coincide in the binary case but form a strictly increasing hierarchy beyond it. Temperature scaling, the reigning default, only targets the weakest of the three: one scalar cannot fix per-class bias. The paper derives a natively multiclass alternative by assuming the uncalibrated score vector is class-conditionally Dirichlet-distributed; the induced calibration map is linear in the log-probabilities followed by a softmax — i.e. exactly one linear layer applied to log **p**, the multiclass generalization of beta calibration and a canonical-link GLM in disguise. Its full-matrix form has k² + k parameters (10,100 at k = 100) and overfits the calibration split, so the paper introduces ODIR (Off-Diagonal and Intercept Regularisation): the diagonal is left free to absorb per-class bias while off-diagonal weights and the intercept are penalized separately, since they act multiplicatively vs. additively. With ODIR, both Dirichlet calibration and plain matrix scaling beat temperature scaling on classwise-ECE, log-loss and Brier score across many datasets and model classes, while the paper also argues classwise-ECE, not confidence-ECE, is the diagnostic that should be reported.

**Why this, why now:** This is the primary source for the claim that the affine/log-linear recalibration family is a GLM on log-probabilities — the same structure the TMLR taxonomy work builds beta calibration on — and it is the paper that says the ECE you compute on a multi-class policy head is the weakest of three inequivalent notions, with classwise-ECE the one that actually catches per-action bias. ODIR is also a concrete answer to the practical problem that any recalibrator richer than a single temperature will overfit a small held-out split.

**Connection to other papers:** It is the direct sequel to Guo et al.'s *On Calibration of Modern Neural Networks* (Track 4, 2026-08-13): it accepts that paper's ECE/temperature-scaling framing, then shows both the diagnostic and the fix are the degenerate top-1 case of a richer problem. Methodologically it lifts Kull, Silva Filho & Flach's binary beta calibration (two Beta densities → bivariate logistic regression on ln s and ln(1−s)) to k Dirichlet densities → a linear map on log **p**, so matrix scaling and temperature scaling both fall out as constrained special cases of one parametric family.

---

## 2026-08-13 (Thursday) - Track 4: Calibration and uncertainty classics
**Title:** On Calibration of Modern Neural Networks
**Authors:** Chuan Guo, Geoff Pleiss, Yu Sun, Kilian Q. Weinberger
**Venue/Year:** Proceedings of the 34th International Conference on Machine Learning (ICML 2017), PMLR vol. 70, pp. 1321-1330; arXiv:1706.04599, June 2017
**Link:** https://arxiv.org/abs/1706.04599

**Summary:** A classifier's confidence should track its true probability of being correct, and older, shallower networks (e.g. LeNet) were reasonably well calibrated in this sense. This paper shows that modern deep networks (ResNets, wide/deep highway nets) are not: despite higher accuracy, they are systematically overconfident, and the paper isolates depth, width, reduced weight decay, and batch normalization as the architectural/training factors that drive miscalibration up even as accuracy improves - accuracy and calibration have become decoupled. The paper formalizes and popularizes Expected Calibration Error (ECE), a binned summary of the gap between predicted confidence and empirical accuracy visualized via reliability diagrams, as the now-standard scalar diagnostic for this gap. It then benchmarks post-hoc calibration methods - histogram binning, isotonic regression, Bayesian Binning into Quantiles, Platt/matrix/vector scaling - against a method the authors introduce, temperature scaling: a single scalar T dividing the pre-softmax logits, fit by minimizing NLL on held-out validation data, which leaves the model's predictions (argmax, accuracy) exactly unchanged while sharply reducing ECE. Despite having only one fitted parameter, temperature scaling matches or beats every more expressive alternative across vision and NLP benchmarks, showing that modern overconfidence is close to a uniform logit-scale miscalibration rather than a shape distortion needing nonparametric correction.

**Why this, why now:** This is the origin paper for both the ECE diagnostic and the affine-style post-hoc recalibration the user already runs on MuZero's prior/value heads - temperature scaling is exactly the single-parameter special case of the affine recalibration family that the TMLR "Taxonomy of Calibration" paper's beta-calibration-as-GLM framing generalizes, making this the primary source for why a one-parameter fix can close most of the overconfidence gap that ECE measures.

---

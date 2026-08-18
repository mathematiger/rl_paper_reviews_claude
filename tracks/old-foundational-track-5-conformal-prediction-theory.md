# Track 5 (old & foundational): Conformal prediction theory

Distribution-free coverage guarantees - the classic papers behind conformal prediction, before and after it had that name.

---

## 2026-08-18 (Tuesday) - Track 5: Conformal prediction theory (distribution-free coverage guarantees)
**Title:** Learning by Transduction
**Authors:** A. Gammerman, V. Vovk, V. Vapnik
**Venue/Year:** Proceedings of the Fourteenth Conference on Uncertainty in Artificial Intelligence (UAI 1998), pp. 148-155, Morgan Kaufmann
**Link:** https://arxiv.org/abs/1301.7375

**Summary:** Standard classifiers of the time (including Vapnik's own SVM) output only a bare label prediction with no distribution-free, per-instance measure of how much to trust it - existing confidence measures either relied on parametric assumptions about the data or gave only asymptotic, population-level guarantees. This paper introduces a transductive alternative: rather than fitting one model to the training set and applying it to a new point, treat the new point's label as unknown and, for each candidate label value, temporarily add the completed (object, candidate-label) pair to the training set and refit. A "strangeness" (nonconformity) measure - derived here from the completed set's SVM solution (e.g. Lagrange multipliers/margin) - scores how atypical each point, including the new one, looks relative to the rest of this augmented set. Ranking the new point's strangeness against all others in the augmented set yields a p-value for that candidate label; repeating this over all candidate labels produces a full p-value vector, from which a confidence region (all labels above a significance threshold) is read off. Under only an i.i.d./exchangeability assumption on the data - no parametric model, no asymptotics - the resulting p-values are shown to be valid in finite samples: the true label's p-value is uniformly distributed, so thresholding controls the region's error rate exactly at any sample size. Experiments on USPS handwritten digits show these regions are empirically well-calibrated at their declared significance levels.

**Why this, why now:** This is the literal ancestor of the split-conformal/APS/RAPS machinery behind the user's recall-maximizing candidate action selection front - nonconformity score -> rank against a calibration set -> p-value -> finite-sample-valid confidence/prediction set is exactly this paper's recipe, predating the term "conformal prediction" itself, and reading it pins down which parts of the modern method (any nonconformity measure works; validity needs only exchangeability, not distributional assumptions) are the original, load-bearing theoretical guarantee.

---

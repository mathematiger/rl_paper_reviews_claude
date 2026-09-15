# Old & Foundational - Track 5: Conformal prediction theory (distribution-free coverage guarantees)

Foundational papers on conformal prediction and distribution-free finite-sample coverage guarantees. Most recent entry first.

---

## 2026-09-15 (Tuesday) - Track 5: Conformal prediction theory (distribution-free coverage guarantees)
**Title:** The limits of distribution-free conditional predictive inference
**Authors:** Rina Foygel Barber, Emmanuel J. Candès, Aaditya Ramdas, Ryan J. Tibshirani
**Venue/Year:** Information and Inference: A Journal of the IMA, 10(2):455-482, 2021 (arXiv preprint 2019)
**Link:** https://arxiv.org/abs/1903.04684 (journal version: https://doi.org/10.1093/imaiai/iaaa017)

**Summary:** Conformal prediction buys finite-sample validity under exchangeability alone, but only *marginally*: coverage holds on average over test points, not for the particular input at hand. This paper asks how much of that gap can be closed without distributional assumptions, and answers mostly negatively. Its central theorem: if a procedure guarantees (1-alpha) coverage conditional on X = x, uniformly over all distributions, then for any distribution with a non-atomic feature marginal the interval it returns has infinite expected length at almost every x. Exact conditional coverage is therefore not merely hard but vacuous - the only distribution-free procedures achieving it are the trivial ones. Earlier special-case versions of this (Vovk; Lei and Wasserman) are generalized here to arbitrary procedures, including randomized ones. The paper then maps the space of relaxations. Requiring coverage conditional on membership in a *collection of subsets* is achievable precisely when the collection is coarse enough - finitely many groups of non-vanishing probability - which is what Mondrian/group-conditional conformal already does. Requiring approximate conditional coverage in shrinking neighborhoods of x, or uniformly over distributions in a neighborhood of P, collapses back onto the impossibility: length again degenerates toward the trivial construction. Positive results survive only once assumptions return (smoothness of the conditional law, or a restricted distribution class), where split-conformal intervals with well-chosen scores are shown to be near-optimal in length.

**Why this, why now:** It draws the exact line under the split-conformal / APS-RAPS pipeline: the coverage certificate on a calibration set is a population-level statement, so any claim that a candidate set covers the true label *in this particular state* is buying something the theory provably cannot sell distribution-free - and the paper names the only two legal ways to recover it, coarse group-conditional calibration or an assumption-backed score whose adaptivity is doing the real work.

**Connection to other papers:** Direct sequel to Gammerman-Vovk-Vapnik's transduction paper (Track 5, 2026-08-18), which established that exchangeability alone yields finite-sample-valid p-values - this paper shows that guarantee is not upgradeable to a per-instance one, so conditional adaptivity must come from the nonconformity score's design rather than from the validity theorem. It also frames the calibration papers of the Track 3/4 lines as complementary: those chase per-instance reliability by accepting model assumptions, precisely the currency this impossibility result says must be spent.

---


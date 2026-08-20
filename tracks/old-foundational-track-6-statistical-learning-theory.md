# Old & Foundational - Track 6: Statistical learning theory (VC theory, Rademacher complexity, bias-variance decompositions)

Primary sources establishing the classical machinery of statistical learning theory. Most recent entry first.

---

## 2026-08-20 (Thursday) - Track 6: Statistical learning theory (VC theory, Rademacher complexity, bias-variance decompositions)
**Title:** Rademacher and Gaussian Complexities: Risk Bounds and Structural Results
**Authors:** Peter L. Bartlett, Shahar Mendelson
**Venue/Year:** Journal of Machine Learning Research, Vol. 3, pp. 463-482, November 2002
**Link:** https://www.jmlr.org/papers/v3/bartlett02a.html

**Summary:** Classical generalization bounds (VC-dimension-based) are data-independent - they depend only on the worst case over the whole hypothesis class and ignore the actual sample or distribution, which can make them extremely loose in practice. This paper develops Rademacher complexity (and its Gaussian analogue) as a data-dependent measure of a function class's capacity: for a sample of size n, it is the expected supremum, over the class, of the empirical correlation between the class's outputs and i.i.d. random sign (Rademacher) variables - intuitively, how well the class can fit pure noise on this specific sample. The central theorem bounds the true risk of any function in a class by its empirical risk plus a term proportional to the class's empirical Rademacher complexity plus a confidence term, holding with high probability for bounded loss functions, without assuming a fixed hypothesis a priori - the bound adapts to the data instead of being fixed by the class alone. The paper's second contribution is a toolbox of structural results - a contraction (Lipschitz composition) lemma, and complexity bounds for convex hulls, sums, and compositions of function classes - that let Rademacher complexity be computed or bounded for composite architectures (kernel machines, neural networks, boosted/convex combinations, decision trees) by decomposing them into simpler pieces, rather than requiring a bespoke VC-dimension calculation for each new architecture.

**Why this, why now:** This is the origin paper for the Rademacher complexity generalization bounds explicitly invoked in the user's own TMLR "Taxonomy of Calibration" paper (Rademacher complexity bounds vs. O(1/N) variational rates) - reading the primary source clarifies exactly what the data-dependent complexity term measures and how the contraction/structural lemmas let it be bounded compositionally, which is the machinery needed to state the Rademacher-based rate rigorously against the variational (Raginsky-Recht) rate it's being compared to.

---

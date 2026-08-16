# Track W4: GNN theory with inspirational/deep theoretical background

Expressivity, over-smoothing/over-squashing, connections to Weisfeiler-Leman, spectral theory, geometric deep learning. Most recent entry first.

---

## 2026-08-16 (Sunday) - Track W4: GNN theory with inspirational/deep theoretical background
**Title:** How Powerful are Graph Neural Networks?
**Authors:** Keyulu Xu, Weihua Hu, Jure Leskovec, Stefanie Jegelka
**Venue/Year:** International Conference on Learning Representations (ICLR 2019); arXiv:1810.00826, October 2018
**Link:** https://arxiv.org/abs/1810.00826

**Summary:** Message-passing GNNs had achieved strong empirical results by 2018, but there was little theoretical understanding of what graph structures they can and cannot tell apart - practitioners picked aggregation functions (mean, max, sum) largely by trial and error. This paper builds a theoretical framework anchoring GNN expressiveness to the classical Weisfeiler-Lehman (WL) graph isomorphism test: it proves that any message-passing GNN is at most as powerful as 1-WL at distinguishing non-isomorphic graphs, and derives the exact condition under which a GNN achieves this upper bound - the neighbor-aggregation function must be injective over multisets of neighbor features. Framed this way, mean- and max-pooling aggregators (as used in GCN and GraphSAGE) are provably unable to distinguish simple structures that differ only in neighbor multiplicity or distribution, while a sum aggregator composed with a suitable MLP can be. The paper turns this into a concrete architecture, the Graph Isomorphism Network (GIN): sum-aggregate neighbor features, pass the result through an MLP, and combine with the previous node representation, iterated across layers with a WL-style multiset-hashing analogy. Empirically, GIN matches or exceeds prior GNN variants on graph classification benchmarks, closing much of the gap between theoretical expressive power and observed performance.

**Why this, why now:** A rare case of a deep theoretical argument - grounding neural graph representation learning in the decades-older combinatorial WL isomorphism test - that directly explains and predicts empirical architecture choices (sum vs. mean/max aggregation) rather than just describing them after the fact; it is the foundational reference point for the entire GNN-expressivity literature (over-smoothing, over-squashing, higher-order WL-GNNs) that followed it.

---

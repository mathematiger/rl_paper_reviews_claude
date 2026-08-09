# Track W2 (weekend, influential & inspirational): Influential GNN breakthroughs

Landmark graph neural network architectures and results - e.g. spectral GNNs, message passing neural networks, GAT, GNN expressivity results. Chosen purely for being influential or intellectually exciting, not tied to the user's specific research fronts.

---

## 2026-08-09 (Sunday) - Track W2: Influential GNN breakthroughs
**Title:** Graph Attention Networks
**Authors:** Petar Veličković, Guillem Cucurull, Arantxa Casanova, Adriana Romero, Pietro Liò, Yoshua Bengio
**Venue/Year:** ICLR 2018; arXiv:1710.10903
**Link:** https://arxiv.org/abs/1710.10903

**Summary:** Prior graph neural networks split into two camps: spectral methods (e.g. Kipf & Welling's GCN) that filter node features using the graph Laplacian's eigenbasis, which ties the learned filters to one fixed graph and blocks generalization to unseen graph structures (no inductive setting); and non-spectral, convolution-like methods that typically aggregate neighbor features with fixed or degree-normalized weights, giving every neighbor equal a-priori importance regardless of relevance. Graph Attention Networks (GAT) resolve both issues with masked self-attention: each node computes an attention coefficient over its immediate neighbors via a shared linear feature transformation followed by a single-layer feedforward attention mechanism, softmax-normalized within the neighborhood, and then aggregates neighbor features weighted by these learned, content-dependent coefficients. Because attention is computed purely from local node-pair features rather than any global graph operator (no eigendecomposition, no fixed adjacency structure baked into the weights), the same parameters transfer to graphs of arbitrary size and topology, making GAT naturally inductive. Multiple independent attention heads are concatenated per layer, mirroring Transformer-style multi-head attention, which stabilizes training. GAT matched or beat prior state of the art on transductive citation-network benchmarks (Cora, Citeseer, Pubmed) and substantially outperformed GraphSAGE-style baselines on the inductive protein-protein interaction (PPI) benchmark, while being straightforward to parallelize across node-neighbor pairs.

**Why this, why now:** GAT is one of the most-cited GNN papers ever and the paper that made attention - rather than fixed spectral or degree-based weighting - the default way to aggregate graph neighborhoods; it directly seeded later graph-transformer architectures and is a clean illustration of how a Transformer-style idea transfers to irregular, non-Euclidean structure. It's a good weekend read for its conceptual elegance: one attention mechanism, shared across arbitrary neighborhoods, unifies inductive and transductive graph learning without touching spectral graph theory.

---

# Track W2 (weekend, influential & inspirational): Influential GNN breakthroughs

Landmark graph neural network architectures and results - e.g. spectral GNNs, message passing neural networks, GAT, GNN expressivity results. Chosen purely for being influential or intellectually exciting, not tied to the user's specific research fronts.

---

## 2026-08-23 (Sunday) - Track W2: Influential GNN breakthroughs
**Title:** Semi-Supervised Classification with Graph Convolutional Networks
**Authors:** Thomas N. Kipf, Max Welling
**Venue/Year:** International Conference on Learning Representations (ICLR 2017); arXiv:1609.02907, September 2016
**Link:** https://arxiv.org/abs/1609.02907

**Summary:** Before this paper, learning on graph-structured data with neural networks meant either full spectral graph convolutions - filtering node features via the eigendecomposition of the graph Laplacian, which is computationally expensive (dense eigendecomposition, non-local filters) and ties the learned filters to one fixed graph - or ad hoc combinatorial/regularization schemes for semi-supervised learning on graphs, such as label propagation via a graph Laplacian regularizer, that assume connected nodes likely share a label and can be too restrictive an assumption. Kipf and Welling derive a graph convolutional network (GCN) as a first-order (linear) Chebyshev polynomial approximation of spectral graph convolutions, which collapses the expensive spectral filter into a simple, local operation: each layer aggregates a node's own features with its immediate neighbors' features through a symmetrically-normalized adjacency matrix, followed by a learned linear transform and a nonlinearity. Stacking K such layers lets information propagate K hops, and because the propagation rule is just a sparse matrix multiplication, the whole model scales linearly in the number of edges rather than requiring eigendecomposition. Trained end-to-end with a semi-supervised classification loss evaluated only on labeled nodes, the model implicitly regularizes using the unlabeled graph structure through the propagation rule itself, with no explicit graph-Laplacian regularization term needed. On citation-network benchmarks (Cora, Citeseer, Pubmed) and a knowledge-graph dataset, this simplified, shallow (2-3 layer) architecture outperformed prior spectral and label-propagation methods by a significant margin while being far cheaper to train.

**Why this, why now:** This is arguably the single most-cited paper in the GNN literature and the one that turned graph neural networks from a niche spectral-methods topic into a mainstream, easily-trained architecture - the propagation rule it derives (normalized-adjacency aggregate + linear transform + nonlinearity) is the direct ancestor of nearly every message-passing GNN that followed, including GraphSAGE, GAT (covered in this track previously), and GIN. It's a clean example of how a heavyweight theoretical object (spectral graph convolution via Laplacian eigenbasis) collapses, under a well-chosen approximation, into a remarkably simple and scalable neural network layer - the same "elegant simplification of expensive machinery" move that recurs throughout deep learning.

---

## 2026-08-09 (Sunday) - Track W2: Influential GNN breakthroughs
**Title:** Graph Attention Networks
**Authors:** Petar Veličković, Guillem Cucurull, Arantxa Casanova, Adriana Romero, Pietro Liò, Yoshua Bengio
**Venue/Year:** ICLR 2018; arXiv:1710.10903
**Link:** https://arxiv.org/abs/1710.10903

**Summary:** Prior graph neural networks split into two camps: spectral methods (e.g. Kipf & Welling's GCN) that filter node features using the graph Laplacian's eigenbasis, which ties the learned filters to one fixed graph and blocks generalization to unseen graph structures (no inductive setting); and non-spectral, convolution-like methods that typically aggregate neighbor features with fixed or degree-normalized weights, giving every neighbor equal a-priori importance regardless of relevance. Graph Attention Networks (GAT) resolve both issues with masked self-attention: each node computes an attention coefficient over its immediate neighbors via a shared linear feature transformation followed by a single-layer feedforward attention mechanism, softmax-normalized within the neighborhood, and then aggregates neighbor features weighted by these learned, content-dependent coefficients. Because attention is computed purely from local node-pair features rather than any global graph operator (no eigendecomposition, no fixed adjacency structure baked into the weights), the same parameters transfer to graphs of arbitrary size and topology, making GAT naturally inductive. Multiple independent attention heads are concatenated per layer, mirroring Transformer-style multi-head attention, which stabilizes training. GAT matched or beat prior state of the art on transductive citation-network benchmarks (Cora, Citeseer, Pubmed) and substantially outperformed GraphSAGE-style baselines on the inductive protein-protein interaction (PPI) benchmark, while being straightforward to parallelize across node-neighbor pairs.

**Why this, why now:** GAT is one of the most-cited GNN papers ever and the paper that made attention - rather than fixed spectral or degree-based weighting - the default way to aggregate graph neighborhoods; it directly seeded later graph-transformer architectures and is a clean illustration of how a Transformer-style idea transfers to irregular, non-Euclidean structure. It's a good weekend read for its conceptual elegance: one attention mechanism, shared across arbitrary neighborhoods, unifies inductive and transductive graph learning without touching spectral graph theory.

---

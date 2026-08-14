# Track 6 (new & adjacent): Exploration strategies in deep RL

Diverse rollout workers, intrinsic motivation, and evolutionary baselines (e.g. CMA-ES vs RL) viewed through recent work neighboring the user's own multi-worker exploration architecture.

---

## 2026-08-14 (Friday) - Track 6: Exploration strategies (evolutionary baselines vs RL)
**Title:** K-Myriad: Jump-starting Reinforcement Learning with Unsupervised Parallel Agents
**Authors:** Vincenzo De Paola, Mirco Mutti, Riccardo Zamboni, Marcello Restelli
**Venue/Year:** arXiv preprint arXiv:2601.18580, January 2026 (Politecnico di Milano)
**Link:** https://arxiv.org/abs/2601.18580

**Summary:** Standard parallelization in deep RL (A3C/IMPALA-style multi-worker rollout collection) speeds up training of a single policy by having many workers sample from the same distribution, wasting the diversity available in a large worker pool. This paper asks whether a pool of parallel workers should instead be specialized before task reward is even introduced, as an unsupervised pretraining step. Building on the authors' earlier ICML 2025 work on maximum-state-entropy exploration for parallel agents (which combined per-agent state entropy with an inter-agent diversity term via a centralized policy-gradient objective), K-Myriad scales this to large populations ("myriads") of agents: it optimizes a portfolio of distinct policies that jointly maximize the collective state entropy induced by the whole population, pushing each worker toward a different, non-redundant region of state space rather than duplicating coverage. The resulting portfolio of specialized exploration policies is then used to jump-start downstream RL - each worker's checkpoint becomes a warm-start initialization for task-specific fine-tuning. On high-dimensional continuous-control benchmarks with large-scale parallelization, the population converges to a broad set of qualitatively distinct behaviors, and initializing task learners from this diverse portfolio improves training efficiency and yields more heterogeneous solutions than initializing from identically-distributed workers.

**Why this, why now:** The user's multi-worker exploration architecture (scripted prefixes, epsilon-greedy TD-proxy exploration, temperature-sampling workers) already hand-assigns each worker a different exploration behavior; K-Myriad reframes exactly that design choice as an optimization problem - learning worker specialization to maximize collective state-entropy coverage instead of hand-tuning per-worker exploration parameters - making it a natural reference point for treating worker diversity itself as a trained objective in the MuZero/Grid2Op multi-worker setup.

---

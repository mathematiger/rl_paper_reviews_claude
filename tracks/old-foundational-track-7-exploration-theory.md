# Track 7 (old & foundational): Exploration theory

UCB/bandit foundations, posterior sampling, and bootstrapped/ensemble approaches to directed exploration.

---

## 2026-08-25 (Tuesday) - Track 7: Exploration theory (UCB/bandit foundations, bootstrapped/ensemble exploration)
**Title:** Deep Exploration via Bootstrapped DQN
**Authors:** Ian Osband, Charles Blundell, Alexander Pritzel, Benjamin Van Roy
**Venue/Year:** Advances in Neural Information Processing Systems 29 (NIPS 2016), pp. 4026-4034; arXiv:1602.04621
**Link:** https://proceedings.neurips.cc/paper_files/paper/2016/file/8d8818c8e140c64c743113f563cf750f-Paper.pdf

**Summary:** Deep RL agents of the DQN era explored by *dithering*: epsilon-greedy or Boltzmann action noise injected independently at every timestep. The paper's core criticism is that such perturbations are temporally incoherent - each random action is immediately "undone" by the next greedy one - so the agent never commits to a multi-step plan whose only purpose is to resolve uncertainty. In environments where informative reward lies many steps behind a sequence of uninformative ones, dithering needs data exponential in horizon, whereas *deep* (temporally-extended) exploration needs only polynomial. The proposed fix is randomized value functions in the spirit of Thompson sampling, but without any tractable posterior: bootstrapped DQN attaches K bootstrap heads to a shared convolutional torso, each head trained on its own bootstrap-resampled data stream (via per-transition binary masks stored in the replay buffer) and its own random initialization, which together act as a cheap approximate posterior over Q. At the start of each episode one head is sampled uniformly and followed greedily for the whole episode - the commitment that makes exploration temporally extended - while gradients from all heads are normalized and shared through the torso, so the ensemble costs only ~20% over DQN and parallelizes trivially. On a scalable chain MDP designed to require deep exploration, learning time scales polynomially rather than exponentially in chain length; on the Arcade Learning Environment, bootstrapped DQN improves learning speed and final score across most Atari games.

**Why this, why now:** This is the primary source for the mechanism behind the user's multi-worker exploration architecture: it argues precisely that a population of workers each *committed* for a whole episode to one sampled value function explores fundamentally better than the same workers each running epsilon-greedy TD-proxy or temperature-sampled dithering, and it gives the bootstrap-mask machinery for making those workers genuinely diverse rather than merely independently noisy. The ensemble also doubles as the cheapest usable uncertainty estimate over a learned value head, which is exactly the object the user's calibration diagnostics are measuring - here it is put to work driving behavior rather than only being scored.

**Connection to other papers:** This is the ensemble/posterior-sampling branch of exploration theory, standing opposite the optimism-under-uncertainty branch of UCB1 and its tree-search descendant UCT (Kocsis & Szepesvari, logged 2026-08-11): both aim at directed rather than random exploration, but UCB adds a deterministic confidence bonus to the value estimate while bootstrapped DQN instead *samples* a value function and acts greedily in it. It is also the deep-network stand-in for posterior-sampling RL (PSRL), replacing an intractable Bayesian posterior over MDPs with a bootstrap ensemble.

---

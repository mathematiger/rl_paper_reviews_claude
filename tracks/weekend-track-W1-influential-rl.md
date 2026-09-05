# Track W1 (weekend, influential & inspirational): Influential RL breakthroughs

Landmark results and systems that reshaped reinforcement learning - e.g. TD-Gammon, DQN, AlphaGo-lineage, landmark benchmarks. Chosen purely for being influential or intellectually exciting, not tied to the user's specific research fronts.

---

## 2026-09-05 (Saturday) - Track W1: Influential RL breakthroughs
**Title:** Grandmaster level in StarCraft II using multi-agent reinforcement learning
**Authors:** Oriol Vinyals, Igor Babuschkin, Wojciech M. Czarnecki, Michaël Mathieu, Andrew Dudzik, Junyoung Chung, David H. Choi, Richard Powell, Timo Ewalds, Petko Georgiev, et al. (DeepMind; David Silver senior author)
**Venue/Year:** Nature 575, 350-354 (2019)
**Link:** https://www.nature.com/articles/s41586-019-1724-z

**Summary** (≈220 words): StarCraft II had been the standing counterexample to the "deep RL solves games" narrative: imperfect information, a combinatorial action space (~10^26 legal actions per step), thousands-of-steps horizons, and no single dominant strategy. Every prior agent had leaned on simplified rules, hand-built sub-systems, or superhuman interfaces. AlphaStar attacks it with general-purpose learning under human-like constraints (camera view, capped action rate), playing all three races on the live Battle.net ladder. The agent is first trained by supervised imitation on human replays, then improved with off-policy actor-critic RL (V-trace, TD(λ), and UPGO, a self-imitation update that pushes the policy toward better-than-expected trajectories). The network puts a transformer torso over the variable-length set of units, a deep LSTM core over the observation sequence, an auto-regressive policy head with a pointer network for variable-length targeting, scatter connections to fuse spatial and non-spatial features, and a centralised value baseline that sees the opponent's observations during training only. The central idea, though, is the *league*: naive self-play chases strategic cycles (rock-paper-scissors dynamics) rather than converging. The league runs three agent types — main agents trained by prioritized fictitious self-play against the whole population weighted by win-rate, main exploiters that attack only the current main agent, and league exploiters (periodically reset) that hunt global blind spots. The result: Grandmaster rating, above 99.8% of ranked human players.

**Why this, why now:** It is the cleanest demonstration that in non-transitive multi-agent domains "train against yourself" is the wrong objective, and that deliberately maintaining a population of adversaries whose job is to *lose* the ladder but *expose* your flaws is what makes learning converge — an idea that has since propagated far outside games. The engineering (transformer over units, auto-regressive pointer heads for enormous structured action spaces) is also a nice template for any setting where the action is a composite object rather than a scalar.

**Connection to other papers:** It is the direct heir of TD-Gammon's self-play recipe (Track W1, 2026-08-08), but replaces plain self-play with a league precisely because backgammon's near-transitive strategy space does not carry over to StarCraft; and it inherits the DQN premise (Track W1, 2026-08-22) of learning control from raw game observations while swapping value-based off-policy learning for large-scale actor-critic with V-trace/UPGO.

---

## 2026-08-22 (Saturday) - Track W1: Influential RL breakthroughs
**Title:** Playing Atari with Deep Reinforcement Learning
**Authors:** Volodymyr Mnih, Koray Kavukcuoglu, David Silver, Alex Graves, Ioannis Antonoglou, Daan Wierstra, Martin Riedmiller
**Venue/Year:** NeurIPS Deep Learning Workshop 2013 / arXiv:1312.5602 (DeepMind, December 2013)
**Link:** https://arxiv.org/abs/1312.5602

**Summary:** Before this paper, applying RL to high-dimensional sensory input (raw pixels) was widely believed to be unstable when combined with nonlinear function approximators like neural networks: RL's data is sequential and correlated (violating the i.i.d. assumption behind most deep learning theory), and the target distribution shifts as the policy improves, unlike the fixed labels of supervised learning. The authors introduce DQN (Deep Q-Network): a convolutional neural network that takes raw, minimally preprocessed pixel frames as input and outputs an estimated Q-value (expected discounted future reward) for each possible action, trained with a variant of Q-learning via stochastic gradient descent on the TD error. The key architectural innovation is experience replay - transitions (state, action, reward, next state) are stored in a memory buffer and sampled randomly in minibatches for training, which breaks the temporal correlation between consecutive updates and lets each transition be reused many times, greatly improving data efficiency and stability. Applied with a single fixed architecture and hyperparameters across seven Atari 2600 games from the Arcade Learning Environment (no per-game tuning), the network takes stacked, downsampled, grayscaled frames as input, passes them through convolutional layers followed by fully connected layers, and outputs one Q-value per legal action, with the highest-valued action selected during play. DQN outperformed all prior RL approaches on six of the seven games and surpassed a human expert on three, using no hand-crafted features whatsoever.

**Why this, why now:** This is the paper that opened deep RL as a field and is the direct ancestor of the AlphaGo/AlphaZero/MuZero lineage the user's PhD sits in - the first demonstration that a single CNN could learn a control policy end-to-end from raw sensory input using nothing but Q-learning and experience replay, no hand-engineered features or per-task tuning. It's an inspirational touchstone for how a deceptively simple recipe (conv net + replay buffer + TD error) triggered the deep RL explosion that led, a few years later, to AlphaGo and eventually MuZero.

---

## 2026-08-08 (Saturday) - Track W1: Influential RL breakthroughs
**Title:** Temporal Difference Learning and TD-Gammon
**Authors:** Gerald Tesauro
**Venue/Year:** Communications of the ACM, Vol. 38, No. 3, pp. 58-68, March 1995
**Link:** https://dl.acm.org/doi/10.1145/203330.203343

**Summary:** Backgammon had long been a proving ground for AI game-playing programs, but hand-crafted evaluation functions plateaued below top human strength. Tesauro's earlier system, Neurogammon, used supervised learning from a corpus of expert-annotated positions and reached strong intermediate play, but scaling further required more labeled data than was available. TD-Gammon instead learns entirely through self-play using temporal-difference learning: a feedforward neural network with a single hidden layer of roughly 40-80 sigmoid units takes a largely raw encoding of the board (198 input units, mostly simple counts of checkers per point plus a few hand-designed features) and outputs a predicted probability of winning from that position. After each move, the network's weights are updated via TD(λ) (λ≈0.7) toward the temporal difference between successive position evaluations, with the actual game outcome providing the final error signal - no human-labeled targets are needed. Move selection during play is a shallow 1-ply search: the network scores each legal successor position and the best is chosen. Starting from random weights and knowledge-free raw features, the program reached Neurogammon-level (strong intermediate) play purely from self-play; adding a modest set of hand-crafted input features pushed it to expert-level and, after 1.5 million self-play games, to a draw against reigning world champion Bill Robertie, rivaling the best human players.

**Why this, why now:** TD-Gammon is the direct ancestor of the AlphaGo/AlphaZero/MuZero lineage the user's PhD sits in - the first system to show a learned value function trained purely by TD-driven self-play, paired with shallow lookahead, could reach superhuman play with no hand-labeled data. It's an inspirational touchstone for how little machinery (one hidden layer, no explicit tree search, no replay buffer) was needed to discover strategies - including unconventional opening plays now considered established theory - that surprised human experts.

---

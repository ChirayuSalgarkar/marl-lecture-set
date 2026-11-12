# Plan: 17 lectures following the book

Each deck is a self-contained 75-minute guest lecture. This table only orders the book material; the slides never refer to other lectures.

Book: S. V. Albrecht, F. Christianos, L. Schäfer, *Multi-Agent Reinforcement Learning: Foundations and Modern Approaches*, MIT Press, 2024 (free at marl-book.com).

Style: picture-first, few words per slide (see the `marl-lecture` skill). Lecture 2 predates this and is still in the old text-heavy style.

Budget per 75-minute lecture: 30–40 content slides, which is about 10–15 book pages of dense theory (Ch. 4, 6, 9) or 25–35 pages of lighter material (Ch. 1, 3, 5, 10, 11).

| # | Lecture | Book sections | Load | Derivations to do in class | Status |
| --- | --- | --- | --- | --- | --- |
| 1 | Introduction and models of interaction | Ch. 1; Ch. 3 | ~34 pp, light | Discount = continuation probability; backward induction in finite PD; belief filter (Eq. 3.5) | in revision: Ch. 1 part redone picture-first; Ch. 3 part still old style |
| 2 | Solution concepts I | 4.1–4.5 | ~10 pp, dense | U_i over histories; best-response vertex claim; maxmin ≤ minmax; minimax LP; indifference; Nash existence sketch; ε-Nash counterexample | done |
| 3 | Solution concepts II | 4.6–4.11 | ~18 pp, dense | CE constraints and LP; Nash ⊆ CE ⊆ CCE; Chicken traffic-light CE; Pareto, welfare, fairness; regret definitions; PPAD | todo |
| 4 | First steps and challenges | Ch. 5 | ~26 pp, medium | Induced MDP for agent i; non-stationary Q-learning target; Climbing game averages; self-play vs mixed-play | todo |
| 5 | Joint-action learning | 6.1–6.2 | ~12 pp, dense | Value iteration for games; Shapley contraction; minimax-Q, Nash-Q, CE-Q; limitations | todo |
| 6 | Agent modeling | 6.3 | ~13 pp, dense | Fictitious play and its convergence classes; JAL-AM; Bayesian learning; value of information | todo |
| 7 | Policy-based learning | 6.4 | ~11 pp, dense | IGA dynamics in 2×2 games (cycling field); WoLF; WoLF-PHC; GIGA | todo |
| 8 | No-regret learning and Part I review | 6.5 | ~8 pp, dense | Unconditional/conditional regret matching → CCE/CE | todo |
| 9 | Deep RL refresher | Ch. 2, 7, 8 (review) | review | Policy gradient theorem; why concurrent training (8.2.8) breaks DQN assumptions; observations vs histories (8.3) | todo |
| 10 | Deep MARL: modes and independent learning | 9.1–9.3 | ~11 pp, medium | CTCE/DTDE/CTDE; independent learning as concurrent single-agent RL | todo |
| 11 | Multi-agent policy gradients | 9.4 | ~12 pp, dense | MAPG theorem; centralized critics; COMA baseline is unbiased; equilibrium selection with critics | todo |
| 12 | Value decomposition I | 9.5.1–9.5.3 | ~13 pp, dense | IGM; VDN and QMIX satisfy IGM | todo |
| 13 | Value decomposition II | 9.5.4–9.5.5 | ~11 pp, dense | Limits of monotonic mixing; QTRAN and beyond | todo |
| 14 | Agent modeling and homogeneous agents | 9.6–9.7 | ~15 pp, medium | Deep agent models; parameter and experience sharing (importance weights) | todo |
| 15 | Self-play in zero-sum games | 9.8 | ~9 pp, medium | UCB in MCTS; AlphaZero loss | todo |
| 16 | Population-based training | 9.9 | ~11 pp, dense | PSRO as double oracle; meta-solvers; AlphaStar league | todo |
| 17 | MARL in practice | Ch. 10–11 | ~32 pp, light | None; code walkthrough and benchmark tour | todo |

## Material already drafted for later lectures

An earlier combined deck (before the book-based split) had slides that belong later. Reuse them when building these lectures:

| Material | Goes to |
| --- | --- |
| Correlated and coarse correlated equilibria; Chicken traffic-light CE; Nash ⊆ CE ⊆ CCE ellipses; Pareto frontier plot for PD | Lecture 3 |
| Four challenges; Climbing game heat map; difference rewards → COMA advantage | Lecture 4 |
| Shapley operator contraction proof; stage-game characterization of Markov-game Nash | Lecture 5 |
| Matching Pennies gradient-cycling vector field (u² + w² conserved) | Lecture 7 |
| CTCE / DTDE / CTDE diagram; CTDE critic/actor diagram; comparison table | Lecture 10 |

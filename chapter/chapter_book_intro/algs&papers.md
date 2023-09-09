

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-04 20:48:28
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-10 03:49:52
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# 涉及的算法以及对应的论文总结

## chapter_basics_of_RL

- MDP：Bellman, R. (1957). A Markovian decision process. Journal of Mathematics and Mechanics, 6(5), 679–684. URL: http://www.jstor.org/stable/24900506

## chapter_RL_basics_algs

- Q-learning: ![Watkins, C. J., & Dayan, P. (1992). Q-learning. Machine Learning, 8(3–4), 279–292.](../../papers_PDF/cjch.pdf) URL:https://www.gatsby.ucl.ac.uk/~dayan/papers/cjch.pdf
- Sarsa:
  - ![On-Line Q-Learning Using Connectionist Systems](../../papers_PDF/rummery_tr166.pdf) http://mi.eng.cam.ac.uk/reports/svr-ftp/auto-pdf/rummery_tr166.pdf
  - 【SARSA was not actually called SARSA by Rummery and Niranjan in their 1994 paper “On-Line Q-Learning Using Connectionist Systems” . The authors preferred “Modified Connectionist Q-Learning.” The alternative was suggested by Richard Sutton and it appears that SARSA stuck.】
  - ![Rummery, G. A., & Niranjan, M. (1994). On-line Q-learning using connectionist systems (Vol. 37). Cambridge, England: University of Cambridge, Department of Engineering.](../../papers_PDF/sutton-96.pdf) http://incompleteideas.net/papers/sutton-96.pdf
- Double Q learning: ![Double Q learning](../../papers_PDF/NIPS-2010-double-q-learning-Paper.pdf) https://proceedings.neurips.cc/paper/3964-double-q-learning.pdf

## chapter_basics_of_DRL&DQN_algs

- DQN：[Deep Q-Network (DQN)](https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf)
- Double DQN： [Double DQN](https://arxiv.org/pdf/1509.06461.pdf)
- Dueling DQN： [Dueling DQN](https://arxiv.org/pdf/1511.06581.pdf)
- Branching DQN：[Branching DQN](https://arxiv.org/pdf/1711.08946.pdf)
- Categorical DQN (C51)：[Categorical DQN (C51)](https://arxiv.org/pdf/1707.06887.pdf)
- QRDQN[Quantile Regression DQN (QRDQN)](https://arxiv.org/pdf/1710.10044.pdf)
- Rainbow DQN (Rainbow)：[Rainbow DQN (Rainbow)](https://arxiv.org/pdf/1710.02298.pdf)

## ？？？

- X[Implicit Quantile Network (IQN)](https://arxiv.org/pdf/1806.06923.pdf)
- X[Fully-parameterized Quantile Function (FQF)](https://arxiv.org/pdf/1911.02140.pdf)

## chapter_continuous_control

- PG：[Policy Gradient (PG)](https://papers.nips.cc/paper/1713-policy-gradient-methods-for-reinforcement-learning-with-function-approximation.pdf)
- NPG：[Natural Policy Gradient (NPG)](https://proceedings.neurips.cc/paper/2001/file/4b86abe48d358ecf194c56c69108433e-Paper.pdf)
- DDPG：[Deep Deterministic Policy Gradient (DDPG)](https://arxiv.org/pdf/1509.02971.pdf)
- TD3：[Twin Delayed DDPG (TD3)](https://arxiv.org/pdf/1802.09477.pdf)

## chapter_actor-critic_algs

- A2C：[Advantage Actor-Critic (A2C)](https://openai.com/blog/baselines-acktr-a2c/)
- PER：[Prioritized Experience Replay (PER)](https://arxiv.org/pdf/1511.05952.pdf)

## chapter_policy_advanced_techniques

- TRPO：[Trust Region Policy Optimization (TRPO)](https://arxiv.org/pdf/1502.05477.pdf)
- PPO: [Proximal Policy Optimization Algorithms](../../papers_PDF/PPO.pdf) https://arxiv.org/abs/1707.06347.pdf
- GAE: [Generalized Advantage Estimator (GAE)](https://arxiv.org/pdf/1506.02438.pdf)

## chapter_offline_RL

- SAC：[Soft Actor-Critic (SAC)](https://arxiv.org/pdf/1812.05905.pdf)


## ？？？

- X[Randomized Ensembled Double Q-Learning (REDQ)](https://arxiv.org/pdf/2101.05982.pdf)
- X[Discrete Soft Actor-Critic (SAC-Discrete)](https://arxiv.org/pdf/1910.07207.pdf)


## chapter_define_reward_difficulty

- Vanilla Imitation Learning
- [Generative Adversarial Imitation Learning (GAIL)](https://arxiv.org/pdf/1606.03476.pdf)

## ？？？

- X[Batch-Constrained deep Q-Learning (BCQ)](https://arxiv.org/pdf/1812.02900.pdf)
- X[Conservative Q-Learning (CQL)](https://arxiv.org/pdf/2006.04779.pdf)
- X[Twin Delayed DDPG with Behavior Cloning (TD3+BC)](https://arxiv.org/pdf/2106.06860.pdf)
- X[Discrete Batch-Constrained deep Q-Learning (BCQ-Discrete)](https://arxiv.org/pdf/1910.01708.pdf)
- X[Discrete Conservative Q-Learning (CQL-Discrete)](https://arxiv.org/pdf/2006.04779.pdf)
- X[Discrete Critic Regularized Regression (CRR-Discrete)](https://arxiv.org/pdf/2006.15134.pdf)




## ？？？

- X[Posterior Sampling Reinforcement Learning (PSRL)](https://www.ece.uvic.ca/~bctill/papers/learning/Strens_2000.pdf)

## chapter_sparse_reward

- ICM：[Intrinsic Curiosity Module (ICM)](https://arxiv.org/pdf/1705.05363.pdf)
- HER：[Hindsight Experience Replay (HER)](https://arxiv.org/pdf/1707.01495.pdf)

## chapter_app_board_game

- AlphaGo Zero：[Mastering Chess and Shogi by Self-Play with a General Reinforcement Learning Algorithm](../../papers_PDF/) https://arxiv.org/abs/1712.01815

## chapter_app_chatbot

- RLHF: [reinforcement learning from human preferences. arxiv: 1706.03741. S.
Casper, et. al. Open problems and fundamental limitations of
reinforcement learning from human feedback. arxiv: 2307.15217.](../../papers_PDF/2307.15217.pdf) https://arxiv.org/pdf/2307.15217
-



[1]: https://github.com/thu-ml/tianshou/blob/master/README.md

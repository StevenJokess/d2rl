

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-26 16:55:09
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-20 02:06:37
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# Index

## 内容范围

- 不涉及状态信号的构造，变化或者学习。我们采用这种方式并不是因为状态表征不重要，而是为了完全专注于决策问题上。换句话说，我们主要关心的事不是设计状态信号，而是基于任何可用的状态信号决定采取什么动作。（我们实际上在最后的17.3章节中会简要的涉及状态的设计和构造）。言下之意就是我们不搞特征工程。
- 大多数强化学习方法都是围绕值函数估计构造的，但是严格来说，为了解决强化学习问题这并不是必须的。比如，遗传算法，遗传规划，进化算法（比如Cross-entropy Method）模拟退火以及其他的优化算法都被用来解决强化学习问题，但是并不涉及值函数。
- 像是进化算法，或者策略搜索方法很多情况下很有效。但是最终表明这个类别最好的方法倾向于以某种方式引入值函数（作者指的是行为-判别器（Actor-Critic）方法，也就是既有策略搜索，也有值函数）。

[1]: https://zhuanlan.zhihu.com/p/53306971

```toc
:maxdepth: 2

intro

MAB
MDP
MC
off-policy_MC
DP(policy&value_iteration)
TD(Sarsa&Q-learning)
Dyna-Q

DQN
more DQNs
REINFORCE in policy-based
Eligibility Trace
Actor-Critic
A2C&A3C
TRPO
PPO
DDPG&TD3
SAC

Sparse_Reward
IL
model_intro
model-based
MCTS
MPC
offline_RL(BCQ&CQL)
Unsupervised RL
GoRL&HER

MARL
IPPO
MADDPG


conclusion&perspective
resource
more_resources
ChatGPT(RLHF)
author_thinking
my_thinking

```

[1]: https://github.com/d2l-ai/d2l-en/edit/master/chapter_reinforcement-learning/index.md

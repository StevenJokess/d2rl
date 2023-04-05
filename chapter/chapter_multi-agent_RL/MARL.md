

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-17 18:02:50
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-05 20:00:29
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 多智能体强化学习

## 算法分类

在多智能体强化学习中，我们一般将强化学习算法分为三类[1]：TODO

## 博弈论的引入

智能体间复杂的关系和智能体之间的影响让 MARL 变得极其复杂。这个时候，博弈论的引入就会让建模变得轻松很多，在这种情况下，我们可以把智能车看作不同的玩家，而此时 NE 则代表不同智能车之间的平衡点。具体来说，MARL 可以分成三种「游戏」：
1. 静态游戏 (static games)：
静态游戏中，智能体无法知道其他智能体所做的决策，故而我们可以认为所有智能体的决策是同步，相互之间不受影响。
2. 动态游戏 (stage games)：
动态游戏中有很多不同的阶段，每个阶段都是一个游戏（stage game），上面提到的囚徒困境就可以看作其中一个阶段的游戏。
3. 重复游戏 (repeated games)：
如果一个 MARL 系统中各个阶段的游戏都很相似，那么就可以被称为重复游戏。

Independent Learning(IL) : SARL算法在多智能体情况下的自然扩展。 IL将来自团队一侧的每个智能体视为不与其他人通信的独立实体。他们每个人都完全根据当地的观察做出决定。IL在完全可观察时易于实现且计算效率高，但在部分可观察下缺乏协作。示例算法包括：

- IQL : Independent Deep Q-Learning，DQN 的简单多智能体扩展。
- IA2C : Independent A2C，A2C 的简单多智能体扩展
- IPG : Independent Policy Gradient，PG 的简单多智能体扩展。
- IPPO : Independent Policy Gradient，PG 的简单多智能体扩展。

Centralised Critic (CC) : CC 专注于学习从集中状态到价值估计的批评网络映射。 具体来说，CC 通常包括一个集中式协调器，该协调器收集来自个人的局部观察并评估全局多智能体状态，每个智能体都在该状态下优化其执行。 CC 属于 Centralised-Training-Decentralised-Executing (CDTE)，一个必不可少的 MARL 框架。 示例算法包括：

- MATRPO : TRPO 与集中评论设置。
- MAPPO : PPO 与集中评论设置。
- MAA2C : A2C 与集中评论设置。
- COMA : 具有反事实多智能体策略梯度的 MAA2C。
- Value Decomposition (VD) : VD 是 CDTE 框架下的一种替代方法，它侧重于对个体的全局价值进行因式分解（或从个体价值函数构成全局价值）。 信用分配是其核心。 示例算法包括：
- VDN : IQL 的值分解变体。 VDN 将团队价值函数分解为智能体价值函数。
- QMIX : QMIX 采用了一个网络，该网络将联合动作值估计为仅以局部观察为条件的每个智能体值的复杂非线性组合。
- VDA2C : A2C 的价值分解变体。
- VDPPO : PPO 的价值分解变体。

## 环境分类

[1]: http://www.jidiai.cn/algorithm#marl_title
[2]: https://developer.aliyun.com/article/818419?spm=a2c6h.12873639.article-detail.55.7fa137a8RUrUg3

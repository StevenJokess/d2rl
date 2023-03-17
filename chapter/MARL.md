

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-17 18:02:50
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-17 18:02:58
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 多智能体强化学习

算法分类
在多智能体强化学习中，我们一般将强化学习算法分为三类[1]：

Independent Learning(IL) : SARL算法在多智能体情况下的自然扩展。 IL将来自团队一侧的每个智能体视为不与其他人通信的独立实体。他们每个人都完全根据当地的观察做出决定。IL在完全可观察时易于实现且计算效率高，但在部分可观察下缺乏协作。示例算法包括：
IQL : Independent Deep Q-Learning，DQN 的简单多智能体扩展。
IA2C : Independent A2C，A2C 的简单多智能体扩展
IPG : Independent Policy Gradient，PG 的简单多智能体扩展。
IPPO : Independent Policy Gradient，PG 的简单多智能体扩展。
Centralised Critic (CC) : CC 专注于学习从集中状态到价值估计的批评网络映射。 具体来说，CC 通常包括一个集中式协调器，该协调器收集来自个人的局部观察并评估全局多智能体状态，每个智能体都在该状态下优化其执行。 CC 属于 Centralised-Training-Decentralised-Executing (CDTE)，一个必不可少的 MARL 框架。 示例算法包括：
MATRPO : TRPO 与集中评论设置。
MAPPO : PPO 与集中评论设置。
MAA2C : A2C 与集中评论设置。
COMA : 具有反事实多智能体策略梯度的 MAA2C。
Value Decomposition (VD) : VD 是 CDTE 框架下的一种替代方法，它侧重于对个体的全局价值进行因式分解（或从个体价值函数构成全局价值）。 信用分配是其核心。 示例算法包括：
VDN : IQL 的值分解变体。 VDN 将团队价值函数分解为智能体价值函数。
QMIX : QMIX 采用了一个网络，该网络将联合动作值估计为仅以局部观察为条件的每个智能体值的复杂非线性组合。
VDA2C : A2C 的价值分解变体。
VDPPO : PPO 的价值分解变体。
环境分类

[1]: http://www.jidiai.cn/algorithm#marl_title

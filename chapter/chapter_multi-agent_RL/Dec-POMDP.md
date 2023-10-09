

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-11 20:05:07
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-10 03:54:11
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Dec-POMDP

去中心化的部分可观测马尔科夫模型（Decentralized partially observable Markov decision progress，DEC-POMDP），是研究不确定性情况下多主体协同决策的重要模型。

由于其求解难度是 NEXP-complete，迄今为止尚没有有效的算法能求出其最优解，但是可以用强化学习来近似求解。

在多智能体强化学习中一种比较典型的学习模式为中心式训练，分布式执行，即在训练时利用所共享的信息来帮助更有效的分布式执行。然而，围绕如何最好地利用集中培训仍然存在着许多挑战。

其中一个挑战是如何表示和使用大多数强化学习方法学习的动作值函数。一方面，正确地捕捉主体行为的影响，需要一个集中的行动价值函数，它决定了全球状态和联合行动的条件。

另一方面，当存在多个 agent 时，这样的函数很难学习，即使可以学习，也无法提供明显的方法来提取分散的策略，允许每个智能体根据单个观察结果选择单个操作。[1]

## Decentralized POMDP


Decentralized POMDP是用来给合作式多智能体任务建模的标准方法，简称DEC-POMDP，表示为 $\mathcal{G}=<\mathcal{S}, \mathcal{U}, P, r, \mathcal{Z}, O, N, \gamma>$ :
- $i \in N:=1, \ldots, N$ : N个agents.
- $s \in S$ : 环境的真实状态
- $u_i \in U$ : 每个agent的动作
- $u:=\left[u_i\right]_{i=1}^N \in \mathcal{U}^N:$ 联合动作向量
- $P\left(s^{\prime} \mid s, u\right): \mathcal{S} \times \mathcal{U}^N \times \mathcal{S} \mapsto[0,1]:$ 状态转移方程
- $r(s, u): \mathcal{S} \times \mathcal{U}^N \mapsto \mathbb{R}$ : 共享的联合奖励函数
- $\gamma \in[0,1)$ : 折扣因子
- $z=O(s, i): \mathcal{S} \times \mathcal{N} \mapsto \mathcal{Z}:$ 每个agent自己的部分观察
- $\tau_i \in \mathcal{T}:=(\mathcal{Z} \times \mathcal{U})^*:$ 每个agent的动作-观察历史


[1]: https://blog.csdn.net/wzduang/article/details/115874734?spm=1001.2014.3001.5502
[2]: https://mayi1996.top/2020/08/13/QTRAN-Learning-to-Factorize-with-Transformation-for-Cooperative-Multi-Agent-Reinforcement-Learning/

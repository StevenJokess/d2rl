

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-12 20:42:25
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-12 20:44:32
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# QMIX

VDN 对于智能体之间的关系有较强的假设，但是，这样的假设并不一定适合所有合作式多智能体问题。在 2018 年的 ICML 会议上，有研究者提出了改进的方法 QMIX。

QMIX 在 VDN 的基础上实现了两点改进：1）在训练过程中加入全局信息进行辅助；2）采用混合网络对单智能体的局部值函数进行合并（而不是简单的线性相加）。

在 QMIX 方法中，首先假设了全局 Q 值和局部 Q 值之间满足这样的关系：最大化全局 Q_tot 值对应的动作，是最大化各个局部 Q_a 值对应动作的组合，即

$\underset{\mathbf{u}}{\operatorname{argmax}} Q_{\text {tot }}(\boldsymbol{\tau}, \mathbf{u})=\left(\begin{array}{c}\operatorname{argmax}_{u^1} Q_1\left(\tau^1, u^1\right) \\ \vdots \\ \operatorname{argmax}_{u^n} Q_n\left(\tau^n, u^n\right)\end{array}\right)$

在这样的约束条件下，既能够使用集中式的学习方法来处理环境不稳定性问题以及考虑多智能体的联合动作效应（全局 Q 值的学习），又能够从中提取出个体策略实现分布式的控制（基于局部 Q 值的行为选择）。进一步地，该约束条件可转化为全局 Q 值和局部 Q 值之间的单调性约束关系：

令全局 Q 值和局部 Q 值之间满足该约束关系的函数表达式有多种，VDN 方法的加权求和就是其中一种，但简单的线性求和并没有充分考虑到不同个体的特性，对全体行为和局部行为之间的关系的描述有一定的局限性。QMIX 采用了一个混合网络模块（mixing network）作为整合 Qa 生成 Q_tot 的函数表达式，它能够满足上述的单调性约束。

在 QMIX 方法设计的网络结构中，每个智能体都拥有一个 DRQN 网络（绿色块），该网络以个体的观测值作为输入，使用循环神经网络来保留和利用历史信息，输出个体的局部 Qi 值。

所有个体的局部 Qi 值输入混合网络模块（蓝色块），在该模块中，各层的权值是利用一个超网络（hypernetwork）以及绝对值计算产生的：绝对值计算保证了权值是非负的、使得局部 Q 值的整合满足单调性约束；利用全局状态 s 经过超网络来产生权值，能够更加充分和灵活地利用全局信息来估计联合动作的 Q 值，在一定程度上有助于全局 Q 值的学习和收敛。

结合 DQN 的思想，以 Q_tot 作为迭代更新的目标，在每次迭代中根据 Q_tot 来选择各个智能体的动作，有：

最终学习收敛到最优的 Q_tot 并推出对应的策略，即为 QMIX 方法的整个学习流程。

[1]:https://www.thepaper.cn/newsDetail_forward_9829763

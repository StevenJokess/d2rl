
论文概述
2022 - Kuba - Multi-Agent Constrained Policy Optimisation

该论文将 trust region learning 推广至了MARL (multi-agent reinforcement learning)：

其提出并证明了multi-agent advantage decomposition lemma，并基于此提出了多智能体的 sequential policy update scheme (update the policy of agent one by one)

而后，基于单智能体上的TRPO和PPO算法，基于新颖的多智能体策略更新方案，作者构建了针对多智能体的trust region算法：HATRPO (Heterogenous-Agent Trust Region Policy Optimisation) 和 HAPPO (Heterogeneous-Agent Proximal Policy Optimisation)

作者证明了该算法的单调改进性 (monotonic improvement)。且该算法 no parameters sharing，也 no any restrictive assumptions on decomposibility of the joint value function.

关于homogenous (同质的) 和heterogenous (异质的)：
Homogenous, sharing the same action space and policy parameters, which largely limits their applicability and harm the performance
Heterogenous, not need agents to share parameters, having their own action space
对比这两个词，再理解下HATRPO (H -> heterogenous) 的含义。将借由顺次更新各个智能体的策略实现这一设想


那么 homogenous，参数共享有什么缺点吗？
其实将 trust region learning 从 single-agent 推广到 MARL 已经有了一些先例，比如 MAPPO。但是它的推广方式十分简单，"equip all agents with one shared set of parameters and use agents' aggregated trajectories to conduct policy optimisation at every iteration"。它学习一个基于global state的centralized value function和一个 sharing policy（各个agent通过局部观测和共享策略做动作），而且并不能从理论上保证单调递增
那么MARL中参数共享 (共享策略空间)可能导致什么问题呢？
看一个例子，

证明如下（通过举反例证明还蛮有意思的；这个证明过程比较好懂）：

则在这个例子中，parameter sharing can lead to a suboptimal outcome that is exponentially-worse with the increasing number of agents.
该算法在 SMAC task 和 MuJoCo task 上为SOTA

还是那个学习顺序，建议先读透Natural PG，再看TRPO。有了TRPO的基础看PPO会很容易。然后再看这篇HATRPO、HAPPO。

这个推进关系上，每个算法都做了改进和变动，如果略过中间一环或者略过一环的某个推导过程，接下来的算法可能真的吃不透。例如，TRPO中并没有那么细致地讲NPG的推导过程；PPO虽延续了TRPO的思想，但是不再复述TRPO中步步推导的目标函数，而是直接讲其改进了。初学者学习trust region PG时，最好从地基起。
符号定义
MARL基本符号

作者新引入两个定义：Multi-agent Q-value Function 和 Multi-agent Advantage Function

Q-value Function
The multi-agent state-action value function Q for an arbitrary ordered agent subset
is defined as

where
 refers to its complement and
 refers to the
 agent in the ordered subset.

complement应该翻译成补集吧，
 应该是指除了这1:m个agents外的agents，即


直观理解下，
 represents the average return if agents
 take a joint action
 at state s.
按照我的理解，再白话一点 (部分用词不准确，大概是那个意思qaq)，agents
 的动作是固定的：
 ，Q是在该状态s、该动作组
 条件下的average return。
 是变量，不确定
 采取什么动作，所以针对它求Q的期望
结合上面两段话，理解Q
Advantage Funciton
The multi-agent advantage function A of subsets
 is defined as

where
 and
 are disjoint subsets.

Advantage Function指 "relative advantage"，要减去 baseline.
在single agent时，
 ，减掉的baseline是状态s的状态函数
在multi agent时，对
 这么理解：

Decomposition Lemma
Multi-Agent Advantage Decomposition Lemma (pivotal)

In any cooperative Markov games, given a joint policy Π, for any state s, and any agent subset , the below equations holds.

​The lemma shows that the joint advantage function can be decomposed into a summation of each agent's local advantages in the process of sequential update

解释一下，


Multi-Agent Advantage Decomposition Lemma证明如下：

ps：从直观上理解从第一行到第二行的意思
Trust Region Learning
将首先写下 trust region learning 应用于 single agent 和 multi agent 时的差异的符号表示和推导顺序，并回顾下 single agent 上的 objective 推导过程，而后顺畅推广至 multi agents中

这里名词执意写 single agent 和 multi agents 而不写 TRPO 和 HATRPO 的原因是 trust region in single agent (描述于 那篇著名论文) / multi agents (描述于本篇论文) 是一种思想，一种scheme，而 TRPO 和 PPO 是前者思想的实现方式，HATRPO 和 HAPPO 是后者思想的实现方式
表示差异

​ps：notion definition不同，一方面是本文作者下了multi agents时的 Q-value Function 和 Advantage Function 的新定义（已写于上一part），另一方面不同论文表示意思相同时可能用了不同符号（部分总结于这儿）。为了下面对比两篇论文数学公式时候顺畅一点，才写了这一部分。其实没什么影响，只是很正常地不同论文中表示不同
Trust Region in Single Agent

Trust Region in Multi Agents



推导过程中一些严格的数学证明：


关于 J(Π) 单调递增的严格证明：


证明中用到的 Lemma8 及其证明：

然后作者又研究了一下该算法的收敛：

This definition characterises the equilibrium point at convergence for cooperative MARL tasks. Based on this, we have the following result that describes the asymptotic convergent behaviour towards NE.


​Nash equilibrium (纳什平衡)：平衡了，任何人都利益最大化，认为遵循协议行事强于违背协议。参考链接1，参考链接2 , 参考链接3


Proposition2中涉及到的Corollary 1又涉及到蛮多推导和定义：




​使用归纳法证明，好长，不放了


最后的证明结果是，

终于得到了Corollary1：


作者甚至证明了，


在TRPO中都没有看到类似证明，真的好严谨啊

还有一个连续性相关的推论一起放在这里：

则 Sequential Policy Update Scheme的伪代码：




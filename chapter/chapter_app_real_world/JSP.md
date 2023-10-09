论文标题：

A Multi-action Deep Reinforcement Learning Framework for Flexible Job-shop Scheduling Problem

论文链接：

https://www.sciencedirect.com/science/article/abs/pii/S0957417422010624

代码链接：

https://github.com/leikun-starting/End-to-end-DRL-for-FJSP

https://github.com/leikun-starting/Dispatching-rules-for-FJSP

柔性作业车间调度问题作为典型的 NP-hard 组合优化问题，目前其求解方法主要分为两类：精确算法和近似算法。基于数学规划的精确算法可以在整个解空间中搜索以找到最优解，但这些方法由于其 NP-hard 特性难以在合理的时间内解决大规模调度问题。因此，越来越多的近似方法（包括启发式、元启发式和机器学习技术）用于求解大规模调度问题。

通常，近似方法可以在计算时间和调度结果的质量之间实现良好的折中，特别是群体智能（swarm intelligence, SI）和进化算法（evolutionary algorithm, EA），如遗传算法、粒子群算法、蚁群算法、人工蜂群算法等。

尽管与精确算法相比，SI 和 EA 可以在合理的时间内解决 FJSP，但这些方法并不能直接应用于求解产线实时运行需求下的大规模资源调度问题。基于优先级的启发式调度规则被广泛地应用于实时调度系统，例如考虑动态事件的调度问题。调度规则通常具有较低的计算复杂度，并且比数学编程和元启发式算法更容易实现。

通常，用于解决 FJSP 的调度规则可以分为两个基本类别：工件选择规则和机器选择规则。这些规则的设计和组合旨在最小化调度目标，例如平均完工时间、平均延误、最大延误。然而，有效的调度规则通常需要大量的领域专业知识和反复试验，并且不能保证求解质量。

近年来，深度强化学习 (deep reinforcement learning, DRL) 已广泛地应用于求解组合优化问题，为解决具有共同特征的调度问题提供了一种思路。然而，目前的工作主要专注于其他类型的组合优化问题，例如旅行商问题和车辆路径问题，对于更为复杂的调度问题如 FJSP 研究较少。

通常，常规的强化学习仅适用于单个动作的决策问题。其中，强化学习智能体与环境交互的方式为：智能体首先从环境中获取状态并根据该状态选择动作，然后获得奖励并转移到下一个状态。然而，在 FJSP 中面临着工序的排序任务和机器的指派任务，即该问题是一个具有多动作空间的决策问题，这意味着常规的强化学习不能直接应用于 FJSP。

图 5 构建了 FJSP 的多动作空间的层级结构。在该层级结构中，强化学习智能体首先从工序动作空间中选择一个工序动作，然后从机器动作空间中选择一个机器动作。



▲ 图5 FJSP的层级动作空间结构示意图

本文首先将柔性作业车间调度过程描述为多动作强化学习任务，并进一步将该任务定义为一个多马可夫决策过程 (Multi-Markov Decision Process)。在此基础上，提出了一种新的基于 GNN 的多指针图网络（multi-pointer graph network, MPGN，如图 6 所示）用于编码嵌入 FJSP 的析取图 (Disjunctive Graph) 作为局部状态，注：析取图作为调度过程中的局部状态提供了调度过程中的全局信息包含数值和结构信息，如工序优先级约束、调度后的工序在每台机器的加工顺序、每个工序的兼容机器集合以及兼容机器的加工时间等。



▲ 图6 MPGN wolkflow

该网络适用于 FJSP、列车调度问题等多动作组合优化问题（结构如图 7 所示）。此外，为训练该网络结构设计基于 actor-critic 风格的多近端策略优化算法 (multi-proximal policy optimization algorithm, multi-PPO) 来训练所提出的 MPGN。

具体实现细节及实验结论请参考原文链接。

此外，我们近期还投稿了使用分层强化学习（Hierarchical Reinforcement Learning）端到端地求解动态调度问题的文章，后续也会开源代码，大家感兴趣可以持续关注下。

欢迎关注 github, 后期会上传 RL to optimize （scheduling and routing problem）及 Offline RL 的代码：

https://github.com/leikun-starting
[1]: https://www.sohu.com/a/593601615_121119001

# TD3+BC

本文是Google Brain团队和McGill大学合作，由 TD3、BCQ的作者 Fujimoto 提出并发表在NeurIPS2021顶会上的文章，本文方法最大的优点是：方法简单、无任何复杂数学公式、可实现性强(开源)、对比实验非常充分（满分推荐），正如标题一样(A minimalist approach)。

摘要： 相比于几篇博客讲过的BCQ（通过扰动网络生成动作，不断将学习策略和行为策略拉进）、BEAR(通过支撑集匹配避免分布匹配的问题)、BRAC（通过VP和PR两个方法正则化）以及REM（通过随机集成混合方法对多个值函数求取凸优化最优的鲁棒性）方法。本文作者提出的TD3+BC方法，结构简单，仅在值函数上添加一个行为克隆(BC）的正则项，并对state进行normalizing，简单的对TD3修改了几行代码就可以与前几种方法相媲美，结果表明：TD3+BC效果好，训练时间也比其他少很多。

1. Offline RL的一些挑战。
实现和Tune的复杂性(Implementation and Tuning Complexities), 在强化学习中，算法的实现、论文的复现都是一个非常难的问题，很多算法并没法去复现，即使相同的seed有时候未必也能达到效果。同样在Offline中仍然存在，此外在Offline中还要解决分布偏移、OODd等之外的一些问题。
额外算力需求(Extra Computation Requirement)，由于过于复杂的数学优化、过多的超参数等算法的执行带来了很长的训练时间，导致不得不增加计算资源来学习算法使得其收敛。
训练策略的不稳定性(Instability of Trained Policies)，强化学习领域的不稳定性众所周知，所以Offline RL如何才能与Supervised leanring一样很稳定是一个重要的研究问题。
Offline RL改进问题(algorithmic/Coding/Optimization)，包括了代码层次的优化改进和理论结构方面的改进等。

其实本文并不是去解决传统的offline RL中的一些诸如分布偏移、OOD、过估计以及等等这些问题，而是去解决如何简单、快速、高效的实现算法的实现与高效运行问题，因此作者面对这些问题，发出疑问并给出方法：

[1]: https://zhuanlan.zhihu.com/p/495616028

# Dyna-2

在Dyna算法框架的基础上后来又发展出了Dyna-2算法框架。和Dyna相比，Dyna-2将和和环境交互的经历以及 模型的预测这两部分使用进行了分离。还是以Q函数为例，Dyna-2将记忆分为永久性记忆 (permanent memory) 和瞬 时记忆 (transient memory), 其中永久性记忆利用实际的经验来更新，瞬时记忆利用模型模拟经验来更新。
永久性记忆的Q函数定义为：

$$
Q(S, A)=\phi(S, A)^T \theta
$$

瞬时记忆的Q函数定义为：

$$
Q^{\prime}(S, A)=\bar{\phi}(S, A)^T \bar{\theta}
$$

组合起来后记忆的Q函数定义为：

$$
\bar{Q}(S, A)=\phi(S, A)^T \theta+\bar{\phi}(S, A)^{T^{-}} \bar{\theta}
$$

Dyna-2的基本思想是在选择实际的执行动作前，智能体先执行一遍从当前状态开始的基于模型的模拟，该模拟 将仿真完整的轨迹，以便评估当前的动作值函数。智能体会根据模拟得到的动作值函数加上实际经验得到的值函数共同 选择实际要执行的动作。价值函数的更新方式类似于 $SARSA(\lambda)$

以下是Dyna-2的算法流程：

TODO

[1]: https://cloud.tencent.com/developer/article/1398231

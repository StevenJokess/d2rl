

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-26 18:17:48
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-02-26 18:18:42
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 无监督强化学习(Unsupervised Reinforcement Learning)：

现有的强化学习方法都是有监督的，这使得智能体过度拟合特定的外在奖励，因此限制了算法的泛化能力。无监督强化学习与监督强化学习非常相似。两者都假设底层环境由马尔可夫决策过程(MDP)或部分可观测MDP描述，目的都是使奖励最大化。主要区别在于监督强化学习假设监督是由环境通过外部奖励提供的，而无监督强化学习通过自我监督任务定义内部奖励。与自然语言处理和机器视觉中的监督一样，监督奖励要么是由人工操作员设计的，要么作为标签提供，这些操作员很难扩展并将强化学习算法的泛化限制到特定任务。迄今为止已知的大多数无监督强化学习算法可以分为三类——基于知识的、基于数据的和基于能力的。基于知识的方法是最大化预测模型的预测误差或不确定性(例如 Curiosity、Disagreement、RND)，基于数据的方法是最大化观察数据的多样性(例如 APT、ProtoRL)，基于能力的方法使状态和一些通常被称为"skill"或"task"向量的潜在向量之间的互信息最大化(例如 DIAYN、SMM、APS)。[1]

[1]: https://www.cnblogs.com/kailugaji/p/15354491.html#_label3_0_2_0




<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-26 18:16:10
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-11 19:36:57
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 元强化学习(Meta Reinforcement Learning)

尽管强化学习具有很好的研究和应用前景, 但从头开始训练算法时, 获取样本的代价过于高昂, 严重阻碍强化学习研究与应用的发展. “Learning to learn”的元学习(meta-learning)为快速、灵活的强化学习提供了可能[65]. 在元强化学习(meta RL)体系当中, 通过在大量先验任务(prior tasks)上训练出泛化能力强的智能体(agent)/元学习者(meta-learner), 在面对新任务时只需少量样本或训练步即可实现快速适应.

早期的元强化学习研究中多使用循环神经网络(Recurrent Neural Network, RNN)表示智能体[46,66]. 之后, 加州大学伯克利分校的人工智能研究组BAIR(Berkeley Artificial Intelligence Research)提出了著名的模型无关元学习方法(Model-Agnostic Meta-Learning, MAML)[53], 通过“二重梯度”算法找到泛化能力最强的参数, 只需一步或几步梯度下降实现对新任务的快速适应. MAML不限定具体的网络模型, 通过改变Loss函数去解决各类问题, 如回归、分类和强化学习. 之后众多工作以此为基础发展出性能更优的算法, 如增加结构化噪声扩大搜索范围的MAESN (Model-Agnostic Exploration with Structured Noise)算法[48], 识别模型任务分布、调整参数的多模型MMAML (Multimodel Model-Agnostic Meta-Learning)算法[67]. 同时, MAML算法因其良好的泛化性能, 已被推广到自适应控制[68]、模仿学习[69-71]、逆强化学习[72]和小样本目标推理[73]等研究领域. 然而, 以MAML为基础的一系列算法中, “二重梯度”过程极大增加了计算量, 同时外层循环采用TRPO、PPO等同步策略方法, 算法在元训练阶段的采样效率较低.

除了以上同步策略算法之外, Rakelly等提出了一种异步策略的、概率表示的强化学习算法PEARL[74](Probabilistic Embeddings for Actor-critic RL), 极大提高了样本效率, 并采用后验采样提高探索效率, 相比同步策略算法实现了20–100倍的元训练(meta-training)采样效率提升, 以及显著的渐进性能提升. 同时, 由于概率表示量的引入, PEARL算法具有更强的探索能力, 能够很好地解决稀疏回报问题. 需要指出的是, PEARL算法并不针对一个新任务去更新策略参数, 而是利用概率表示的潜在上下文信息泛化到新任务. 一旦新任务与元训练任务间存在较大差异, PEARL算法的表现将大幅下降. 此外, Mendonca等在最近的工作中提出一种新的引导式元策略学习方法GMPS (Guided Meta-Policy Search)[49], 通过多个异步策略的局部学习者(local learner)独立学习不同的任务, 再合并为一个中心学习者(centralized learner)来快速适应新的任务, 同样实现了元训练效率跨量级的提升. 此外, GMPS算法能够充分利用人类示范或视频示范, 适应稀疏回报的操纵性问题. 虽然GMPS算法在采样效率、探索效率、稀疏回报问题上均有十分优异的表现, 但其中的元(训练)策略非常复杂, 进一步增加了异步策略超参数的敏感性, 算法的复现和应用难度极大.

http://www.c-s-a.org.cn/html/2020/12/7701.html#outline_anchor_9

---

除改善一个具体任务上的学习效率外，研究人员也在寻求能够提高在不同任务上整体学习表现的方法，这与模型的通用性(Generality)和多面性(Versatility)相关。因此，我们会问，如何让智能体基于它所学习的旧任务来在新任务上更快地学习？因此有了元学习(Meta Learning)这一概念。

元学习的最初目的是让智能体解决不同问题或掌握不同技能。然而，我们无法忍受它对每个任务都从头学习，尤其是用深度学习来拟合的时候。元学习，也称学会学习(Learn to Learn)，是让智能体根据以往经验在新任务上更快学习的方法，而非将每个任务作为一个单独的任务。通常一个普通的学习者学习一个具体任务的过程被看作是元学习中的内循环(Inner-Loop)学习过程，而元学习者(Meta-Learner)可以通过一个外循环(Outer-Loop)学习过程来更新内循环学习者。这两种学习过程可以同时优化或者以一种迭代的方式进行。三个元学习的主要类别为循环模型(Recurrent Model)、度量学习(Metric Learning)和学习优化器(Optimizer)。结合元学习和强化学习，可以得到元强化学习(Meta Reinforcement Learning)方法。一种有效的元强化学习方法像与模型无关的元学习(Finn et al., 2017) 可以通过小样本学习(Few-Shot Learning)或者几步更新来解决一个简单的新任务。可参见：https://www.cnblogs.com/kailugaji/tag/Meta%20Learning/

TODO:https://www.cnblogs.com/kailugaji/p/15592726.html

[1]: https://www.cnblogs.com/kailugaji/p/15354491.html#_label3_0_2_0

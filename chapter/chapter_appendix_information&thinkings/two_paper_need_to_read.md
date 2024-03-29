# 工作必读两篇文献

当你阅读本书并使用深度强化学习进行工作或研究时，请务必阅读以下两篇文献。按照已发表的论文完全原样实现，却不能很好地再现结果。针对这一问题，“Deep Reinforcement Learning that Matters”[1]进行了验证，分析了难以进行深度强化学习和难以复现论文的因素，包括了超参数、神经网络结构、奖励的缩放/裁剪、随机数种子值和试验变化以及实现方法等，在实现时需要特别注意这些要点。在“Deep Reinforcement Learning Doesn't Work Yet—Sorta Insightful”[2]中讨论了深度强化学习的实际应用问题。完成深度强化学习任务，面临着很多困难，例如学习需要很长时间，目前对很多任务而言，其他方法在性能上有更好的表现；很难设计奖励，很容易陷入局部解决方案，学习好的网络很难迁移到别的任务上，甚至在成功学习的网络中，每次试验的成功以及可获得的奖励变化非常大等。深度强化学习的另一个问题是：在存在多个Agent的环境下或者能获得有效奖励较少的环境下，学习较困难。

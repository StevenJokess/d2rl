# DARQN



## 结合Attention机制

随着视觉注意力机制在目标跟踪和机器翻译等领域的成功[1], Sorokin等受此启发提出深度注意力递归Q网络 (deep attention recurrent Q network, DARQN). 它能够选择性地重点关注相关信息区域, 减少深度神经网络的参数数量和计算开销[2]

## 注意力机制与多智能体强化学习

注意力机制(Attention) 是一种重要的深度学习方法，它最主要的用途是自然语言处理，比如机器翻译、情感分析。本章的目的不是详细解释注意力机制的原理，而是它在多智能体强化学习(MARL) 中的应用。第17.1 简单介绍自注意力机制(Self-Attention)，它是一种特殊的注意力机制。第17.2 将自注意力机制应用在MARL，改进中心化训练或中心化决策。当智能体数量m 较大时，自注意力机制对MARL 有明显的效果提升。

总结：自注意力机制在非合作关系的MARL 中普遍适用。如果系统架构使用中心化训练，那么m 个价值网络可以用一个神经网络实现，其中使用自注意力层。如果系统架构使用中心化决策，那么m 个策略网络也可以实现成一个神经网络，其中使用自注意力层。在m 较大的情况下，使用自注意力层对效果有较大的提升。

[1]: https://datawhalechina.github.io/unusual-deep-learning/#/16.DRL?id=%e5%85%b6%e5%ae%83%e6%b7%b1%e5%ba%a6%e5%bc%ba%e5%8c%96%e5%ad%a6%e4%b9%a0%e6%96%b9%e6%b3%95
[2]: http://pg.jrj.com.cn/acc/Res/CN_RES/INDUS/2023/2/9/27c20431-8ed3-4562-83b5-5c82706f28a5.pdf



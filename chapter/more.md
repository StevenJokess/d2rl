# 更多算法

## D4PG算法

分布的分布式深度确定性策略梯度算法（distributed distributional deep deterministic policy gradient，D4PG)，相对于深度确定性策略梯度算法，其优化部分如下。[1]

1）分布式评论员(critic)：不再只估计Q值的期望值，而是估计期望Q值的分布，即将期望Q值作为一个随机变量来估计。

2）N 步累计回报：计算时序差分误差时，D4PG 计算的是N步的时序差分目标值而不仅仅只有一步，这样就可以考虑未来更多步骤的回报。

3）多个分布式并行演员(actor)：D4PG 使用K个独立的演员并行收集训练数据并存储到同一个回放缓冲区中。

4）优先经验回放（prioritized experience replay，PER）：使用一个非均匀概率 π 从回放缓冲区中进行数据采样。

[1]: https://www.cnblogs.com/kailugaji/p/16140474.html

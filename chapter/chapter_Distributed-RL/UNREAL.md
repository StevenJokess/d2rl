# UNREAL

Jaderberg 等在 A3C 的基础上做了进一步扩展，提出了非监督强化辅助学习(unsupervised reinforcement and auxiliary learning，UNREAL)算法[28]。UNREAL 算法在训练 A3C 的同时，训练多个辅助任务来改进算法，其中包含了两类辅助任务，第一种是控制任务，包括像素控制和隐层激活控制，另一种是回馈预测任务。

UNREAL 算法本质上是通过训练多个面向同一个最终目标的任务来提升动作网络的表达能力和水平，这样提升了深度强化学习的数据利用率，在 A3C 算法的基础上对性能和速度进行进一步提升。实验结果显示，UNREAL 在 Atari 游戏上取得了人类水平 8.8 倍的成绩，并且在第一视角的 3D 迷宫环境 Labyrinth 上也达到了 87%的人类水平。

[1]: https://www.eefocus.com/article/402315.html

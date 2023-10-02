

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-30 01:38:07
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-30 01:38:24
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# 现实世界中的强化学习

## 现实世界中强化学习面临的挑战

谷歌Deepmind和谷歌研究院合作发表论文，研究为什么强化学习虽然在游戏等问题获得了巨大成功，但在现实世界中仍然没有被大规模应用。他们讨论了下面九个制约因素：

1. 能够对现场系统从有限的采样中学习；
2. 处理系统执行器、传感器、或奖赏中存在的未知、可能很大的延迟；
3. 在高维状态空间和动作空间学习、行动；
4. 满足系统约束，永远或极少违反；
5. 与部分可观察的系统交互，这样的系统可以看成是不平稳的或随机的；
6. 从多目标或没有很好指明的奖赏函数学习；
7. 可以提供实时动作，尤其是为高控制频率的系统；
8. 从外部行为策略的固定的日志数据离线学习；
9. 为系统操作员提供可解释的策略。他们辨识并定义了这些挑战因素，对每个挑战设计实验并做分析，设计实现基线任务包含这些挑战因素，并开源了软件包。

# 视频压缩

[1]: https://www.bilibili.com/video/BV1Nd4y1t7qE/?spm_id_from=333.337.search-card.all.click&vd_source=bca0a3605754a98491958094024e5fe3
[2]: https://arxiv.org/abs/2202.06626
[3]: https://www.deepmind.com/blog/muzeros-first-step-from-research-into-the-real-world
[4]: https://cloud.tencent.com/developer/article/2197037

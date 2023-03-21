

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-23 20:09:19
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-01 01:33:08
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 有模型学习

不同于免模型学习，有模型学习方法不是很好分类：很多方法之间都会有交叉。我们会给出一些例子，当然肯定不够详尽，覆盖不到全部。在这些例子里面， 模型 要么已知，要么是可学习的。

**背景：纯规划 ：**这种最基础的方法，从来不显示的表示策略，而是纯使用规划技术来选择行动，例如 [模型预测控制](/MPC.md) (model-predictive control, MPC)。在模型预测控制中，智能体每次观察环境的时候，都会计算得到一个对于当前模型最优的规划，这里的规划指的是未来一个固定时间段内，智能体会采取的所有行动（通过学习值函数，规划算法可能会考虑到超出范围的未来奖励）。智能体先执行规划的第一个行动，然后立即舍弃规划的剩余部分。每次准备和环境进行互动时，它会计算出一个新的规划，从而避免执行小于规划范围的规划给出的行动。

- [MBMF](https://sites.google.com/view/mbmf) 在一些深度强化学习的标准基准任务上，基于学习到的环境模型进行模型预测控制。

**专家迭代 ：**纯规划的后来之作，使用、学习策略的显示表示形式： $\pi_{\theta}(a|s)$ 。智能体在模型中应用了一种规划算法，类似蒙特卡洛树搜索(Monte Carlo Tree Search)，通过对当前策略进行采样生成规划的候选行为。这种算法得到的行动比策略本身生成的要好，所以相对于策略来说，它是“专家”。随后更新策略，以产生更类似于规划算法输出的行动。

- [ExIt](https://arxiv.org/abs/1705.08439) 算法用这种算法训练深层神经网络来玩 Hex
- [AlphaZero](https://arxiv.org/abs/1712.01815) 这种方法的另一个例子

**免模型方法的数据增强** ：使用免模型算法来训练策略或者 Q 函数，要么 1）更新智能体的时候，用构造出的假数据来增加真实经验 2）更新的时候 仅 使用构造的假数据

- [MBVE](https://arxiv.org/abs/1803.00101) 用假数据增加真实经验
- [World Models](https://worldmodels.github.io/) 全部用假数据来训练智能体，所以被称为：“在梦里训练”

Embedding Planning Loops into Policies. ：另一种方法直接把规划程序作为策略的子程序，这样在基于任何免模型算法训练策略输出的时候，整个规划就变成了策略的附属信息。这个框架最核心的概念就是，策略可以学习到如何以及何时使用规划。这使得模型偏差不再是问题，因为如果模型在某些状态下不利于规划，那么策略可以简单地学会忽略它。

参见 [I2A](https://arxiv.org/abs/1707.06203)智能体被这种想象力赋予了这种风格。


[1]: https://spinningup.readthedocs.io/zh_CN/latest/spinningup/rl_intro2.html
[2]: https://spinningup.openai.com/en/latest/spinningup/rl_intro2.html

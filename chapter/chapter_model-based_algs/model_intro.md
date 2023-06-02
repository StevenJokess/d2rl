

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-19 23:32:35
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-18 00:04:18
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 模型（model）简介

## 规划

## 学习与规划

- 学习：环境初始时是未知的，个体不知道环境如何工作，个体通过与环境进行交互，逐渐改善其行为策略。
- 规划：环境如何工作对于个体是已知或近似已知的，个体并不与环境发生实际的交互，而是利用其构建的模型进行计算，在此基础上改善其行为策路。

### 对应方法

- 学习型方法：不需依赖于模型的强化学习方法(model-free)，直接在环境交互中学习，并不对环境本身进行学习。如蒙特卡洛和时间差分
- 规划型方法：依赖模型的强化学习方法(model-based)，学习环境如何工作，了解环境工作的方式，即学习得到一个模型，然后利用这个模型进行规划。如动态规划和启发式搜索(heuristic search)[2]

### 规划分类

- 状态空间规划（State-space planning）：搜索状态空间去得到最优策略，后续讨论的规划方法都属于这一类
- 规划空间规划（Plan-space planning）：直接对规划空间搜索，对于具有随机性的序列决策问题可能难以有效使用。

## 直接强化学习 VS 间接强化学习

- 直接强化学习：智能体直接从环境中获得奖励，并且根据奖励来学习最优策略。这种方法的优点是简单直接，可以在很多场景下使用。缺点是需要更多的交互时间来学习策略，因为智能体需要通过试错来发现最佳行动策略。
- 间接强化学习：

## 模型

模型描述了每次行动后状态转移和得到的奖励分布 $p(s’, r|s, a)$，模型分两种：

- 分布式模型（distribution model）：返回各种可能的状态 $s’$ 和奖励 $r$ 对应的概率
- 样本式模型（sample model）：只按照概率返回其中的一种可能性 $s’, r$

$M_{v,π}$ 通常用来表示一个包含状态值函数和策略的模型。

### 为什么要学习一个模型

- 优点：
  - 通过构建一个模型，个体具备了一定程度的独立思考能力，即在与环境发生实际交互之前思考各种可能的行为其对能带给环境及自身的改变。
  - 通过个体的思考以及联合其与环境的实际交互经验，个体在解决大规模MDP问题时可以取得更好的结果。
- 缺点：
  - 模型其实是一个个体对坏境运行机制的描述，不完全是真实的环境运行机劁，因出存在近似。当使用一近似的模型去进行价值函数或策略函数的学习时，又会引入一次近似。因此会带来双重的近似误差

- 本讲内容：如何从经历中直接学习模型，如何构建一个模型，如何基于模型来进行“规划”，在次基础上将学和“规划”整合起来形成Dyna算法。

### 基于模型的学习

通常情况, 根据环境四元组 $E=<S, A, P, R>$ 是否完全已知, 强化学习可以分为基于模型学习和免模型学习。

基于模型学习模型学习表示四元组 $E=<S, A, P, R>$ 已知, 即机器可以对环境进行完整建模, 能在机器内部模 拟出与环境相同或近似的状况, 可以通过模拟推算 计算出来不同策略带来的价值回报, 通过不断的模 拟计算, 总能找出一个 (可能存在多个最优策略) 最优的策略来得到最大的回报, 因此在模型已知时 强化学习任务能够归结为基于动态规划的寻优问题。

![基于模型的学习](../img/model_learning.png)

循环：

1. 与现实交互，产生经脸
2. 学习经验，构建模型
3. 用模型，规划下步动作行为
4. 通过规划，优化现实动作

### 基于模型的规划

- 规划的过程相当于解决一个MDP的过程，求解最优值函数，最优策略。
- 实际规划过程中，利用模型来产生一个时间步长的*虚拟经历*，有了这些虚拟采样，随后使用不喜于模型的强化学习方法来学习得到价值或策略函数。这种虚拟采样方法通常很有效。

问题：不能有效探索

## 学习与规划整合

整合考虑将把基于模型的学习和不基于模型的学习结合起来，形城一个整合的架构，利用两者的优点来解决复杂问题。

当构建了一个环境的模型后，个体可以有两种经历来源：

- 实际经历(Real experience)，从环境采样（Sampled from environment）（true MDP）：$\begin{aligned} s^{\prime} & \sim \mathcal{P}_{s, s^{\prime}}^a \\ r & =\mathcal{R}_s^a\end{aligned}$
- 模拟经历(Simulated experience)，从模型采样（Sampled from model）（approximate MDP） ：$\begin{array}{r}s^{\prime} \sim \mathcal{P}_\eta\left(s^{\prime} \mid s, a\right) \\ r=\mathcal{R}_\eta(r \mid s, a)\end{array}$

model-free learning:

- no model
- Learn value function (and/or policy)from real experience

model-based learning:

- Learn a model from real experience
- Plan value function (and/or policy)from simulated experience

将不基于模型的真实经历和基于模型采样得到的模拟经历结合起来：

- Learn a model from real experience
- Learn and plan value function (and/or policy)from real andsimulated experience

提出了一种新的架构 [Dyna](Dyna-Q.md)。

[1]: https://www.bilibili.com/video/BV1HT411C78A?p=42&vd_source=bca0a3605754a98491958094024e5fe3
[2]: https://zhuanlan.zhihu.com/p/37898383
[3]: https://github.com/borninfreedom/DeepLearning/blob/master/Papers/AlphaZero%E5%8E%9F%E7%90%86%E4%B8%8E%E5%90%AF%E7%A4%BA.pdf
> https://chat.openai.com; M_v,π是啥意思；"M_v,π" 通常用来表示一个包含状态值函数和策略的模型。 不应该是 M_{v,π}



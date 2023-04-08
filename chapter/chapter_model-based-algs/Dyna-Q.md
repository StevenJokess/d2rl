

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-26 17:03:52
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-09 01:06:18
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Dyna-Q 算法

## 简介

在强化学习中，“模型”通常指与智能体交互的环境模型，即对环境的状态转移概率和奖励函数进行建模。根据是否具有环境模型，强化学习算法分为两种：**基于模型的强化学习**（model-based reinforcement learning）和**无模型的强化学习**（model-free reinforcement learning）。无模型的强化学习根据智能体与环境交互采样到的数据直接进行策略提升或者价值估计，第 5 章讨论的两种时序差分算法，即 Sarsa 和 Q-learning 算法，便是两种无模型的强化学习方法，本书在后续章节中将要介绍的方法也大多是无模型的强化学习算法。在基于模型的强化学习中，模型可以是事先知道的，也可以是根据智能体与环境交互采样到的数据学习得到的，然后用这个模型帮助策略提升或者价值估计。第 4 章讨论的两种动态规划算法，即策略迭代和价值迭代，就是基于模型的强化学习方法，在这两种算法中环境模型是事先已知的。本章即将介绍的 Dyna-Q 算法也是非常基础的基于模型的强化学习算法，不过它的环境模型是通过采样数据估计得到的。

![基于模型的](../img/model-based.png)

强化学习算法有两个重要的**评价指标**：一个是算法收敛后的策略在初始状态下的期望回报，另一个是样本复杂度，即算法达到收敛结果需要在真实环境中采样的样本数量。

基于模型的强化学习算法由于具有一个环境模型，智能体可以额外和环境模型进行交互，对真实环境中*样本的需求量往往就会减少*，因此通常会比无模型的强化学习算法具有更低的样本复杂度。但是，环境模型可能并不准确，不能完全代替真实环境，因此基于模型的强化学习算法收敛后其策略的*期望回报可能不如*无模型的强化学习算法。

从上述结果中我们可以很容易地看出，随着 Q-planning 步数的增多，Dyna-Q 算法的收敛速度也随之变快。当然，并不是在所有的环境中，都是 Q-planning 步数越大则算法收敛越快，这取决于环境是否是确定性的，以及环境模型的精度。在上述悬崖漫步环境中，状态的转移是完全确定性的，构建的环境模型的精度是最高的，所以可以通过增加 Q-planning 步数来直接降低算法的样本复杂度。



## Dyna-Q

Dyna-Q 算法是一个经典的基于模型的强化学习算法。如图 6-1 所示，Dyna-Q 使用一种叫做 Q-planning 的方法来基于模型生成一些模拟数据，然后用模拟数据和真实数据一起改进策略。Q-planning 每次选取一个曾经访问过的状态，采取一个曾经在该状态下执行过的动作 $a$，通过模型得到转移后的状态 $s' $以及奖励 $r$，并根据这个模拟数据 $(s, a, r, s')$，用 Q-learning 的更新方式来更新动作价值函数。

下面我们来看一下 Dyna-Q 算法的具体流程：

- 初始化 $Q(s, a)$，初始化模型 $M(s, a)$ 【具体来说，包括奖励模型 $R(s, a)$ 和状态转移概率模型 $P(s, a)$[2]】

- **for** $t=1 \rightarrow T$ **do**：
  - 用 $\epsilon$-贪婪策略根据 $Q$ 选择当前状态 $s$ 下的动作 $a$
  - 得到环境反馈的 $s^{\prime}, r$
  - $Q(s, a) \leftarrow Q(s, a)+\alpha\left[r+\gamma \max _{a^{\prime}} Q\left(s^{\prime}, a^{\prime}\right)-Q(s, a)\right]$
  - $M(s, a) \leftarrow r, s^{\prime}$【使用 $s, a, s^{\prime}$ 更新状态模型 $P(s, a)$ ，使用 $s, a, r$ 更新状态模型 $R(s, a)$】
  - **for** 次数 $n=1 \rightarrow N$ **do**:
    - 随机选择一个曾经访问过的状态 $s_m$
    - 采取一个曾经在状态 $s_m$ 下执行过的动作 $a_m$
    - $r_m, s_m^{\prime} \leftarrow M\left(s_m, a_m\right)$【基于模型 $P(s, a)$ 得到 $s^{\prime}$, 基于模型 $R(s, a)$ 得到 $r$】
    - $Q\left(s_m, a_m\right) \leftarrow Q\left(s_m, a_m\right)+\alpha\left[r_m+\gamma \max _{a^{\prime}} Q\left(s_m^{\prime}, a^{\prime}\right)-Q\left(s_m, a_m\right)\right]$
  - **end for**
- **end for**

可以看到，在每次与环境进行交互执行一次 Q-learning 之后，Dyna-Q 会做 $N$ 次Q-planning。其中 Q-planning 的次数 $N$ 是一个事先可以选择的超参数，当其为 0 时就是普通的 Q-learning。

值得注意的是，上述 Dyna-Q 算法是执行在一个离散并且确定的环境中，所以当看到一条经验数据 $\left(s, a, r, s^{\prime}\right)$ 时，可以直接对模型做出更新，即 $M(s, a) \leftarrow r, s^{\prime}$ 。

## Dyna-Q 代码实践

我们在悬崖漫步环境中执行过 Q-learning 算法，现在也在这个环境中实现 Dyna-Q，以方便比较。首先仍需要实现悬崖漫步的环境代码，和 5.3 节一样。

然后我们在 Q-learning 的代码上进行简单修改，实现 Dyna-Q 的主要代码。最主要的修改是加入了环境模型`model`，用一个字典表示，每次在真实环境中收集到新的数据，就把它加入字典。根据字典的性质，若该数据本身存在于字典中，便不会再一次进行添加。在 Dyna-Q 的更新中，执行完 Q-learning 后，会立即执行 Q-planning。

code

下面是 Dyna-Q 算法在悬崖漫步环境中的训练函数，它的输入参数是 Q-planning 的步数。

code

接下来对结果进行可视化，通过调整参数，我们可以观察 Q-planning 步数对结果的影响（另见彩插图 3）。若 Q-planning 步数为 0，Dyna-Q 算法则退化为 Q-learning。

code

![基于模型的](../img/model-based.png)


## 小结

本章讲解了一个经典的基于模型的强化学习算法 Dyna-Q，并且通过调整在悬崖漫步环境下的 Q-planning 步数，直观地展示了 Q-planning 步数对于收敛速度的影响。我们发现基于模型的强化学习算法 Dyna-Q 在以上环境中获得了很好的效果，但这些环境比较简单，模型可以直接通过经验数据得到。如果环境比较复杂，状态是连续的，或者状态转移是随机的而不是决定性的，如何学习一个比较准确的模型就变成非常重大的挑战，这直接影响到基于模型的强化学习算法能否应用于这些环境并获得比无模型的强化学习更好的效果。

[1]: https://hrl.boyuai.com/chapter/1/dyna-q%E7%AE%97%E6%B3%95/
[2]: https://cloud.tencent.com/developer/article/1398231

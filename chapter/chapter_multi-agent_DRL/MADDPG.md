

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-23 20:18:52
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-28 22:32:25
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 简介

## 多智能体强化学习进阶

第 20 章中已经初步介绍了多智能体强化学习研究的问题和最基本的求解范式。本章来介绍一种比较经典且效果不错的进阶范式：*中心化训练时去中心化执行*（centralized training with decentralized execution，CTDE）。所谓中心化训练去中心化执行是指在训练的时候使用一些单个智能体看不到的全局信息而以达到更好的训练效果，而在执行时不使用这些信息，每个智能体完全根据自己的策略直接动作以达到去中心化执行的效果。中心化训练去中心化执行的算法能够在训练时有效地利用全局信息以达到更好且更稳定的训练效果，同时在进行策略模型推断时可以仅利用局部信息，使得算法具有一定的扩展性。CTDE 可以类比成一个足球队的训练和比赛过程：在训练时，11 个球员可以直接获得教练的指导从而完成球队的整体配合，而教练本身掌握着比赛全局信息，**教练的指导也是从整支队、整场比赛的角度进行的**；而训练好的 11 个球员在上场比赛时，则根据场上的实时情况直接做出决策，不再有教练的指导。

CTDE 算法主要分为两种：

- 基于价值函数的方法，例如 VDN，QMIX 算法等；
- 基于 Actor-Critic 的方法，例如 MADDPG 和 COMA 等。

本章将重点介绍 MADDPG 算法。

## MADDPG 算法

多智能体 DDPG（muli-agent DDPG，MADDPG）算法从字面意思上来看就是对于每个智能体实现一个 DDPG 的算法。所有智能体共享一个中心化的 Critic 网络，该 Critic 网络在训练的过程中同时对每个智能体的 Actor 网络给出指导，而执行时每个智能体的 Actor 网络则是完全独立做出行动，即去中心化地执行。

CTDE 算法的应用场景通常可以被建模为一个**部分可观测马尔可夫博弈**(partially observable Markov games) : 用 $\mathcal{S}$ 代表 $N$ 个智能体所有可能的状态空间，这是全局的信息。对于每个智能体 $i$ ，其动作空间为 $\mathcal{A}_i$ ，观测空间 为 $\mathcal{O}_i$ ，每个智能体的策略 $\pi_{\theta_i}: \mathcal{O}_i \times \mathcal{A}_i \rightarrow[0,1]$ 是一个概率分布，用来表示智能体在每个观测下采取各个动作的概率。环境的状态转移函数为

$\mathcal{T}: \mathcal{S} \times \mathcal{A}_1 \times \cdots \times \mathcal{A}_N \rightarrow \Omega(\mathcal{S})$ 。每个智能体的奖励函数为

$r_i: \mathcal{S} \times \mathcal{A} \rightarrow \mathbb{R}$ ，每个智能体从全局状态得到的部分观测信息为

$\mathbf{o}_i: \mathcal{S} \rightarrow \mathcal{O}_i$ ，初始状态分布为 $\rho: \mathcal{S} \rightarrow[0,1]$ 。每个智能体的目标是最大化 其期望累积奖励 $\mathbb{E}\left[\sum_{t=0}^T \gamma^t r_i^t\right]_{\text {。 }}$

接下来我们看一下 MADDPG 算法的主要细节吧！如图 21-1 所示，每个智能 体用 Actor-Critic 的方法训练，但不同于传统单智能体的情况，在 MADDPG 中每个智能体的 Critic 部分都能够获得其他智能体的策略信息。具体来说， 考虑一个有 $N$ 个智能体的博恋，每个智能体的策略参数为 $\theta=\left\{\theta_1, \ldots, \theta_N\right\}$ ，记 $\pi=\left\{\pi_1, \ldots, \pi_N\right\}$ 为所有智能体的策略集合，那么我们可以写出在随 机性策略情况下每个智能体的期望收益的策略梯度：

$$
\nabla_{\theta_i} J\left(\theta_i\right)=\mathbb{E}_{s \sim p^\mu, a \sim \pi_i}\left[\nabla_{\theta_i} \log \pi_i\left(a_i \mid o_i\right) Q_i^\pi\left(\mathbf{x}, a_1, \ldots, a_N\right)\right]
$$

图21-1 MADDPG 算法总览图

其中， $Q_i^\pi\left(\mathbf{x}, a_1, \ldots, a_N\right)$ 就是一个中心化的动作价值函数。为什么说 $Q_i$ 是 一个中心化的动作价值函数呢? 一般来说 $\mathbf{x}=\left(o_1, \ldots, o_N\right)$ 包含了所有智能 体的观测，另外 $Q_i$ 也需要输入所有智能体在此刻的动作，因此 $Q_i$ 工作的前提就是所有智能体要同时给出自己的观测和相应的动作。

对于确定性策略来说，考虑现在有 $N$ 个连续的策略 $\mu_{\theta_i}$ ，可以得到 DDPG 的梯度公式：

$$
\nabla_{\theta_i} J\left(\mu_i\right)=\mathbb{E}_{\mathbf{x} \sim \mathcal{D}}\left[\left.\nabla_{\theta_i} \mu_i\left(o_i\right) \nabla_{a_i} Q_i^\mu\left(\mathbf{x}, a_1, \ldots, a_N\right)\right|_{a_i=\mu_i\left(o_i\right)}\right]
$$

其中， $\mathcal{D}$ 是我们用来存储数据的经验回放池，它存储的每一个数据为 $\left(\mathbf{x}, \mathbf{x}^{\prime}, a_1, \ldots, a_N, r_1, \ldots, r_N\right)$ 。而 在 MADDPG 中，中心化动作价值函数可以按照下面的损失函数来更新：

$$
\mathcal{L}\left(\omega_i\right)=\mathbb{E}_{\mathbf{x}, a, r, \mathbf{x}^{\prime}}\left[\left(Q_i^\mu\left(\mathbf{x}, a_1, \ldots, a_N\right)-y\right)^2\right], \quad y=r_i+\left.\gamma Q_i^{\mu^{\prime}}\left(\mathbf{x}^{\prime}, a_1^{\prime}, \ldots, a_N^{\prime}\right)\right|_{a_j^{\prime}=\mu_j^{\prime}\left(o_j\right)}
$$

其中， $\mu^{\prime}=\left(\mu_{\theta_1}^{\prime}, \ldots, \mu_{\theta_N}^{\prime}\right)$ 是更新价值函数中使用的目标策略的集合，它们有着延迟更新的参数。


MADDPG 的具体算法流程如下：

- 随机初始化每个智能体的 Actor 网络和 Critic 网络
  - for 序列  $e = 1 \rightarrow E$ do
    - 初始化一个随机过程 $N$ ，用于动作探索；
    - 获取所有智能体的初始观测 $\mathbf{x}$；
  - for  $t = 1 \rightarrow E$ do：
    - 对于每个智能体 ，用当前的策略选择一个动作；
    - 执行动作 并且获得奖励和新的观测；
    - 把存储到经验回放池中；
    - 从中随机采样一些数据；
    - 对于每个智能体，中心化训练 Critic 网络
    - 对于每个智能体，训练自身的 Actor 网络
    - 对每个智能体，更新目标 Actor 网络和目标 Critic 网络
  - end for
- end for

## MADDPG 代码实践

下面我们来看看如何实现 MADDPG 算法，首先是导入一些需要用到的包。

我们要使用的环境为多智能体粒子环境（multiagent particles environment，MPE），它是一些面向多智能体交互的环境的集合，在这个环境中，粒子智能体可以移动、通信、“看”到其他智能体，也可以和固定位置的地标交互。

接下来安装环境，由于 MPE 的官方仓库的代码已经不再维护了，而其依赖于 gym 的旧版本，因此我们需要重新安装 gym 库。

本章选择 MPE 中的`simple_adversary` 环境作为代码实践的示例，如图 21-2 所示。该环境中有 1 个红色的对抗智能体（adversary）、个蓝色的正常智能体，以及个地点（一般），这个地点中有一个是目标地点（绿色）。这个正常智能体知道哪一个是目标地点，但对抗智能体不知道。正常智能体是合作关系：它们其中任意一个距离目标地点足够近，则每个正常智能体都能获得相同的奖励。对抗智能体如果距离目标地点足够近，也能获得奖励，但它需要猜哪一个才是目标地点。因此，正常智能体需要进行合作，分散到不同的坐标点，以此欺骗对抗智能体。

需要说明的是，MPE 环境中的每个智能体的动作空间是离散的。第 13 章介绍过 DDPG 算法本身需要使智能体的动作对于其策略参数可导，这对连续的动作空间来说是成立的，但是对于离散的动作空间并不成立。但这并不意味着当前的任务不能使用 MADDPG 算法求解，因为我们可以使用一个叫作 Gumbel-Softmax 的方法来得到离散分布的近似采样。下面我们对其原理进行简要的介绍并给出实现代码。

假设有一个随机变量 $Z$ 服从某个离散分布 $\mathcal{K}=\left(a_1, \ldots, a_k\right)$ 。 其中， $a_i \in[0,1]$ 表示 $P[Z=i]$ 并且满足 $\sum_{i=1}^k a_i=1$ 。当我们希望按照这个分布即 $z \sim \mathcal{K}$ 进行采样时，可以发现这种离散分布的采样是不可导的。

那有没有什么办法可以让离散分布的采样可导呢? 答案是肯定 的! 那就是重参数化方法，这一方法在第 14 章的 SAC 算法中 已经介绍过，而这里要用的是 Gumbel-Softmax 技巧。具体来 说，我们引入一个重参数因子 $g_i$ ，它是一个采样自 $\operatorname{Gumbel}(0,1)$ 的噪声:

$$
g_i=-\log (-\log u), u \sim \operatorname{Uniform}(0,1)
$$

Gumbel-Softmax 采样可以写成

$$
y_i=\frac{\exp \left(\left(\log a_i+g_i\right) / \tau\right)}{\sum_{j=1}^k \exp \left(\left(\log a_j+g_i\right) / \tau\right)}, i=1, \ldots, k
$$

此时，如果通过 $z=\arg \max _i y_i$ 计算离散值，该离散值就近似等价于离散采样 $z \sim \mathcal{K}$ 的值。更进一步，采样的结果 $\mathbf{y}$ 中自然地引入了对于 $a$ 的梯度。

$\tau>0$ 被称作分布的温度参数，通过调整它可以控制生成的 Gumbel-Softmax 分布与离散分布的近似程度： $\tau$ 越小，生成的分布越趋向于 onehot $\left(\arg \max _i\left(\log a_i+g_i\right)\right)$ 的结果； $\tau$ 越大，生成的分布越趋向于均匀分布。

接着再定义一些需要用到的工具函数，其中包括让 DDPG 可以适用于离散动作空间的 Gumbel Softmax 采样的相关函数。

接着实现我们的单智能体 DDPG。其中包含 Actor 网络与 Critic 网络，以及计算动作的函数，这在第 13 章中的已经介绍过，此处不再赘述。但这里没有更新网络参数的函数，其将会在 MADDPG 类中被实现。

接下来正式实现一个 MADDPG 类，该类对于每个智能体都会维护一个 DDPG 算法。它们的策略更新和价值函数更新使用的是 21.2 节中关于接下来正式实现一个 MADDPG 类，该类对于每个智能体都会维护一个 DDPG 算法。它们的策略更新和价值函数更新使用的是 21.2 节中关于 $J\left(\mu_i\right)$ 和 $\mathcal{L}\left(\omega_i\right)$ 的公式给出的形式。


现在我们来定义一些超参数，创建环境、智能体以及经验回放池并准备训练。



接下来实现以下评估策略的方法，之后就可以开始训练了！



训练结束，我们来看看训练效果如何。



可以看到，正常智能体`agent_0`和`agent_1`的回报结果完全一致，这是因为它们的奖励函数完全一样。正常智能体最终保持了正向的回报，说明它们通过合作成功地占领了两个不同的地点，进而让对抗智能体无法知道哪个地点是目标地点。另外，我们也可以发现 MADDPG 的收敛速度和稳定性都比较不错。

## 小结

本章讲解了多智能体强化学习**CTDE 范式**下的经典算法 MADDPG，MADDPG 后续也衍生了不少多智能体强化学习算法。因此，理解 MADDPG 对深入探究多智能体算法非常关键，有兴趣的读者可阅读 MADDPG 原论文加深理解。

[1]: https://hrl.boyuai.com/chapter/3/%E5%A4%9A%E6%99%BA%E8%83%BD%E4%BD%93%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E8%BF%9B%E9%98%B6/

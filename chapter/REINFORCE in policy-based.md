

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-24 00:06:24
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-18 01:38:02
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:https://hrl.boyuai.com/chapter/2/%E7%AD%96%E7%95%A5%E6%A2%AF%E5%BA%A6%E7%AE%97%E6%B3%95
-->
# 策略梯度算法

## 简介

本书之前介绍的 Q-learning、DQN 及 DQN 改进算法都是**基于价值**（value-based）的方法，其中 Q-learning 是处理有限状态的算法，而 DQN 可以用来解决连续状态的问题。在强化学习中，除了基于值函数的方法，还有一支非常经典的方法，那就是**基于策略**（policy-based）的方法。对比两者，基于值函数的方法主要是学习值函数，然后根据值函数导出一个策略，学习过程中并不存在一个显式的策略；而基于策略的方法则是**直接显式地学习一个目标策略**。策略梯度是基于策略的方法的基础，本章从策略梯度算法说起。

## 策略梯度（Policy gradient）

基于策略的方法首先需要将策略参数化。假设目标策略是一个随机性策略，并且处处可微，其中是对应的参数。我们可以用一个线性模型或者神经网络模型来为这样一个策略函数建模，输入某个状态，然后输出一个动作的概率分布。我们的目标是要寻找一个最优策略并最大化这个策略在环境中的期望回报。我们将策略学习的目标函数定义为

$$
J(\theta)=\mathbb{E}_{s_0}\left[V^{\pi_\theta}\left(s_0\right)\right]
$$

其中， $s_0$ 表示初始状态。现在有了目标函数，我们将目标函数对策略 $\theta$ 求导，得 到导数后，就可以用梯度上升方法来最大化这个目标函数，从而得到最优策略。
我第 3 章讲解过策略 $\pi$ 下的状态访问分布，在此用 $\nu{ }^\pi$ 表示。然后我们对目标函数 求梯度，可以得到如下式子，更详细的推导过程将在 $9.6$ 节给出。

$$
\begin{aligned}
\nabla_\theta J(\theta) & \propto \sum_{s \in S} \nu^{\pi_\theta}(s) \sum_{a \in A} Q^{\pi_\theta}(s, a) \nabla_\theta \pi_\theta(a \mid s) \\
& =\sum_{s \in S} \nu^{\pi_\theta}(s) \sum_{a \in A} \pi_\theta(a \mid s) Q^{\pi_\theta}(s, a) \frac{\nabla_\theta \pi_\theta(a \mid s)}{\pi_\theta(a \mid s)} \\
& =\mathbb{E}_{\pi_\theta}\left[Q^{\pi_\theta}(s, a) \nabla_\theta \log \pi_\theta(a \mid s)\right]
\end{aligned}
$$

这个梯度可以用来更新策略，且按回合（episode）更新。[2]需要注意的是，因为上式中期望的下标是  $\pi_\theta$ ，所以策略梯度算法为**在线策略**（on-policy）算法，即必须使用当前策略 $\pi_\theta$ 采样得到的数据来计算梯度。直观理解一下策略梯度这个公式，可以发现在每一个状态下，梯度的修改是*让策略更多地去采样到带来较高 $Q$ 值的动作更少地去采样到带来较低 $Q$ 值的动作*，可以说是不断试错的公式化，如图 9-1 所示。

在计算策略梯度的公式中，我们需要用到 $Q^{\pi_\theta}(s, a)$ ，可以用多种方式对它进行估计。接下来要介绍的 REINFORCE 算法便是采用了蒙特卡洛方法来估计 $Q^{\pi_\theta}(s, a)$ ，对于一个有限步数的环境来说， REINFORCE 算法中的策略梯度为:

$\nabla_\theta J(\theta)=\mathbb{E}_{\pi_\theta}\left[\sum_{t=0}^T\left(\sum_{t^{\prime}=t}^T \gamma^{t^{\prime}-t} r_{t^{\prime}}\right) \nabla_\theta \log \pi_\theta\left(a_t \mid s_t\right)\right]$

其中，$T$是和环境交互的最大步数。例如，在车杆环境下，$T = 200$。

## REINFORCE

REINFORCE 算法的具体算法流程如下：

- 初始化策略参数 $\theta$
  - for 序列 $e=1 \rightarrow E$ do :
    - 用当前策略 $\pi_\theta$ 采样轨迹 $\left\{s_1, a_1, r_1, s_2, a_2, r_2, \ldots s_T, a_T, r_T\right\}$
    - 计算当前轨迹每个时刻 $t$ 往后的回报 $\sum_{t^{\prime}=t}^T \gamma^{t^{\prime}-t} r_{t^{\prime}}$ ，记为 $\psi_t$
    - 对 $\theta$ 进行更新， $\theta=\theta+\alpha \sum_t^T \psi_t \nabla_\theta \log \pi_\theta\left(a_t \mid s_t\right)$
- end for

这便是 REINFORCE 算法的全部流程了。接下来让我们来用代码来实现它，看看效果如何吧！

## REINFORCE 代码实践

我们在车杆环境中进行 REINFORCE 算法的实验。

首先定义策略网络PolicyNet，其输入是某个状态，输出则是该状态下的动作概率分布，这里采用在离散动作空间上的softmax()函数来实现一个可学习的多项分布（multinomial distribution）。

再定义我们的 REINFORCE 算法。在函数`take_action()`函数中，我们通过动作概率分布对离散的动作进行采样。在更新过程中，我们按照算法将损失函数写为策略回报的负数，即，对求导后就可以通过梯度下降来更新策略。

定义好策略，我们就可以开始实验了，看看 REINFORCE 算法在车杆环境上表现如何吧！

在 CartPole-v0 环境中，满分就是 200 分，我们发现 REINFORCE 算法效果很好，可以达到 200 分。接下来我们绘制训练过程中每一条轨迹的回报变化图。由于回报抖动比较大，往往会进行平滑处理。

可以看到，随着收集到的轨迹越来越多，REINFORCE 算法有效地学习到了最优策略。不过，相比于前面的 DQN 算法，REINFORCE 算法使用了更多的序列，这是因为 REINFORCE 算法是一个在线策略算法，之前收集到的轨迹数据*不会被再次利用。*此外，REINFORCE 算法的性能也有一定程度的波动，这主要是因为每条采样轨迹的回报值波动比较大，这也是 REINFORCE 算法主要的不足。

可以用DP部分观测的MDP[3]

缺点：在REINFORCE算法中，每次需要根据一个策略采集一条完整的轨迹，并计算这条轨迹上的回报。这种采样方式估计梯度时噪声很大，即方差比较大，学习效率也比较低，即收敛缓慢，或学习不稳定。

## 小结

REINFORCE 算法是策略梯度乃至强化学习的典型代表，智能体根据当前策略直接和环境交互，通过采样得到的轨迹数据直接计算出策略参数的梯度，进而更新当前策略，使其向最大化策略期望回报的目标靠近。这种学习方式是典型的从交互中学习，并且其优化的目标（即策略期望回报）正是最终所使用策略的性能，这比基于价值的强化学习算法的优化目标（一般是时序差分误差的最小化）要更加直接。 REINFORCE 算法理论上是能保证局部最优的，它实际上是借助蒙特卡洛方法采样轨迹来估计动作价值，这种做法的一大优点是可以得到无偏的梯度。但是，正是因为使用了蒙特卡洛方法，REINFORCE 算法的梯度估计的方差很大，可能会造成一定程度上的不稳定，这也是第 10 章将介绍的 Actor-Critic 算法要解决的问题。

## 扩展阅读：策略梯度证明

策略梯度定理是强化学习中的重要理论。本节我们来证明

$$
\nabla_\theta J(\theta) \propto \sum_{s \in S} \nu^{\pi_\theta}(s) \sum_{a \in A} Q^{\pi_\theta}(s, a) \nabla_\theta \pi_\theta(a \mid s) 。
$$

先从状态价值函数的推导开始：

$$
\begin{aligned}
\nabla_\theta V^{\pi_\theta}(s) & =\nabla_\theta\left(\sum_{a \in A} \pi_\theta(a \mid s) Q^{\pi_\theta}(s, a)\right) \\
& =\sum_{a \in A}\left(\nabla_\theta \pi_\theta(a \mid s) Q^{\pi_\theta}(s, a)+\pi_\theta(a \mid s) \nabla_\theta Q^{\pi_\theta}(s, a)\right) \\
& =\sum_{a \in A}\left(\nabla_\theta \pi_\theta(a \mid s) Q^{\pi_\theta}(s, a)+\pi_\theta(a \mid s) \nabla_\theta \sum_{s^{\prime}, r} p\left(s^{\prime}, r \mid s, a\right)\left(r+\gamma V^{\pi_\theta}\left(s^{\prime}\right)\right)\right. \\
& =\sum_{a \in A}\left(\nabla_\theta \pi_\theta(a \mid s) Q^{\pi_\theta}(s, a)+\gamma \pi_\theta(a \mid s) \sum_{s^{\prime}, r} p\left(s^{\prime}, r \mid s, a\right) \nabla_\theta V^{\pi_\theta}\left(s^{\prime}\right)\right) \\
& =\sum_{a \in A}\left(\nabla_\theta \pi_\theta(a \mid s) Q^{\pi_\theta}(s, a)+\gamma \pi_\theta(a \mid s) \sum_{s^{\prime}} p\left(s^{\prime} \mid s, a\right) \nabla_\theta V^{\pi_\theta}\left(s^{\prime}\right)\right)
\end{aligned}
$$

为了简化表示，我们让 $\phi(s)=\sum_{a \in A} \nabla_\theta \pi_\theta(a \mid s) Q^{\pi_\theta}(s, a)$ ，定义 $d^{\pi_\theta}(s \rightarrow x, k)$

为策略 $\pi$ 从状态 $s$ 出发 $k$ 步后到达状态 $x$ 的概率。我们继续推导:

$$
\begin{aligned}
\nabla_\theta V^{\pi_\theta}(s) & =\phi(s)+\gamma \sum_a \pi_\theta(a \mid s) \sum_{s^{\prime}} P\left(s^{\prime} \mid s, a\right) \nabla_\theta V^{\pi_\theta}\left(s^{\prime}\right) \\
& =\phi(s)+\gamma \sum_a \sum_{s^{\prime}} \pi_\theta(a \mid s) P\left(s^{\prime} \mid s, a\right) \nabla_\theta V^{\pi_\theta}\left(s^{\prime}\right) \\
& =\phi(s)+\gamma \sum_{s^{\prime}} d^{\pi_\theta}\left(s \rightarrow s^{\prime}, 1\right) \nabla_\theta V^{\pi_\theta}\left(s^{\prime}\right) \\
& =\phi(s)+\gamma \sum_{s^{\prime}} d^{\pi_\theta}\left(s \rightarrow s^{\prime}, 1\right)\left[\phi\left(s^{\prime}\right)+\gamma \sum_{s^{\prime \prime}} d^{\pi_\theta}\left(s^{\prime} \rightarrow s^{\prime \prime}, 1\right) \nabla_\theta V^{\pi_\theta}\left(s^{\prime \prime}\right)\right] \\
& =\phi(s)+\gamma \sum_{s^{\prime}} d^{\pi_\theta}\left(s \rightarrow s^{\prime}, 1\right) \phi\left(s^{\prime}\right)+\gamma^2 \sum_{s^{\prime \prime}} d^{\pi_\theta}\left(s \rightarrow s^{\prime \prime}, 2\right) \nabla_\theta V^{\pi_\theta}\left(s^{\prime \prime}\right) \\
& =\phi(s)+\gamma \sum_{s^{\prime}} d^{\pi_\theta}\left(s \rightarrow s^{\prime}, 1\right) \phi\left(s^{\prime}\right)+\gamma^2 \sum_{s^{\prime \prime}} d^{\pi_\theta}\left(s^{\prime} \rightarrow s^{\prime \prime}, 2\right) \phi\left(s^{\prime \prime}\right)+\gamma^3 \sum_{s^{\prime \prime \prime}} d^{\pi_\theta}\left(s \rightarrow s^{\prime \prime \prime}, 3\right) \nabla_\theta V^{\pi_\theta}\left(s^{\prime \prime \prime}\right) \\
& =\cdots \\
& =\sum_{x \in S} \sum_{k=0}^{\infty} \gamma^k d^{\pi_\theta}(s \rightarrow x, k) \phi(x)
\end{aligned}
$$

$$
\begin{aligned}
\nabla_\theta J(\theta) & =\nabla_\theta \mathbb{E}_{s_0}\left[V^{\pi_\theta}\left(s_0\right)\right] \\
& =\sum_s \mathbb{E}_{s_0}\left[\sum_{k=0}^{\infty} \gamma^k d^{\pi_\theta}\left(s_0 \rightarrow s, k\right)\right] \phi(s) \\
& =\sum_s \eta(s) \phi(s) \\
& =\left(\sum_s \eta(s)\right) \sum_s \frac{\eta(s)}{\sum_s \eta(s)} \phi(s) \\
& \propto \sum_s \frac{\eta(s)}{\sum_s \eta(s)} \phi(s) \\
& =\sum_s \nu^{\pi_\theta}(s) \sum_a Q^{\pi_\theta}(s, a) \nabla_\theta \pi_\theta(a \mid s)
\end{aligned}
$$

证明完毕!

？？？

![数学推导[5]](../img/PG_math.png)

https://hrl.boyuai.com/chapter/2/%E7%AD%96%E7%95%A5%E6%A2%AF%E5%BA%A6%E7%AE%97%E6%B3%95#96-%E6%89%A9%E5%B1%95%E9%98%85%E8%AF%BB%EF%BC%9A%E7%AD%96%E7%95%A5%E6%A2%AF%E5%BA%A6%E8%AF%81%E6%98%8E

[1]: https://hrl.boyuai.com/chapter/2/%E7%AD%96%E7%95%A5%E6%A2%AF%E5%BA%A6%E7%AE%97%E6%B3%95
[2]: https://www.cnblogs.com/kailugaji/p/16140474.html
[3]: http://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec-5.pdf

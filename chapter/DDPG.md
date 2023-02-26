

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-23 21:12:17
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-02-26 16:48:00
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# DDPG算法

# 简介

之前的章节介绍了基于策略梯度的算法 REINFORCE、Actor-Critic 以及两个改进算法——TRPO 和 PPO。这类算法有一个共同的特点：它们都是在线策略算法，这意味着它们的**样本效率**（sample efficiency）比较低。我们回忆一下 DQN 算法，DQN 算法直接估计最优函数 Q，可以做到离线策略学习，但是它只能处理动作空间有限的环境，这是因为它需要从所有动作中挑选一个值最大的动作。如果动作个数是无限的，虽然我们可以像 8.3 节一样，将动作空间离散化，但这比较粗糙，无法精细控制。那有没有办法可以用类似的思想来处理动作空间无限的环境并且使用的是离线策略算法呢？本章要讲解的**深度确定性策略梯度**（deep deterministic policy gradient，DDPG）算法就是如此，它构造一个**确定性策略**，用梯度上升的方法来最大化 $Q$ 值。DDPG 也属于一种 Actor-Critic 算法。我们之前学习的 REINFORCE、TRPO 和 PPO 学习随机性策略，而本章的 DDPG 则学习一个**确定性策略**。

## DDPG算法

之前我们学习的策略是随机性的，可以表示为 $a \sim \pi_\theta(\cdot \mid s)$ ；而如果策略是确 定性的，则可以记为 $a=\mu_\theta(s)$ 。与策略梯度定理类似，我们可以推导出确定性策略梯度定理 (deterministic policy gradient theorem)：

$$
\nabla_\theta J\left(\pi_\theta\right)=\mathbb{E}_{s \sim \nu^{\pi_\beta}}\left[\left.\nabla_\theta \mu_\theta(s) \nabla_a Q_\omega^\mu(s, a)\right|_{a=\mu_\theta(s)}\right]
$$

其中， $\pi_\beta$ 是用来收集数据的行为策略。我们可以这样理解这个定理：假设现在已经有函数 $Q$ ，给定一个状态 $s$ ，但由于现在动作空间是无限的，无法通过遍历所有动作来得到 $Q$ 值最大的动作，因此我们想用策略 $\mu$ 找到使 $Q(s, a)$ 值最大的动作 $a$ ，即 $\mu(s)=\arg \max_a Q(s, a)$ 。此时， $Q$ 就是 Critic， $\mu$ 就是 Actor，这是一个 Actor-Critic 的框架，如图 13-1 所示。

那如何得到这个 $\mu$ 呢? 首先用 $Q$ 对 $\mu_\theta$ 求导 $\nabla_\theta Q\left(s, \mu_\theta(s)\right)$ ，其中会用到梯度 的链式法则，先对 $a$ 求导，再对 $\theta$ 求导。然后通过梯度上升的方法来最大化函 数 $Q$ ，得到 $Q$ 值最大的动作。具体的推导过程可参见 $13.5$ 节。

下面我们来看一下 DDPG 算法的细节。DDPG 要用到 4 个神经网络，其中 Actor 和 Critic 各用一个网络，此外它们都各自有一个目标网络。至于为什么需要目标网络，读者可以回到第 7 章去看 DQN 中的介绍。DDPG 中 Actor 也需要目标网络因为目标网络也会被用来计算目标 $Q$ 值。DDPG 中目标网络的更新与 DQN 中略有不同：在 DQN 中，每隔一段时间将网络直接复制给目标 $Q$ 网络；而在 DDPG 中，目标 $Q$ 网络的更新采取的是一种软更新的方式，即让目标网络缓慢更新，逐渐接近 $Q$ 网络，其公式为：

$$
\omega^{-} \leftarrow \tau \omega+(1-\tau) \omega^{-}
$$

通常是一个比较小的数，当 $tau = 1$ 时，就和 DQN 的更新方式一致了。而目标网络也使用这种软更新的方式。

另外，由于函数存在值过高估计的问题，DDPG 采用了 Double DQN 中的技术来更新网络。但是，由于 DDPG 采用的是确定性策略，它本身的探索仍然十分有限。回忆一下 DQN 算法，它的探索主要由-贪婪策略的行为策略产生。同样作为一种离线策略的算法，DDPG 在行为策略上引入一个随机噪声来进行探索。我们来看一下 DDPG 的具体算法流程吧！




## DDPG 代码实践

下面我们以倒立摆环境为例，结合代码详细讲解 DDPG 的具体实现。

对于策略网络和价值网络，我们都采用只有一层隐藏层的神经网络。策略网络的输出层用正切函数 $(y=\tanh x)$ 作为激活函数，这是因为正切函数的值域是 $[-1,1]$ ，方便按比例调整成环境可以接受的动作范围。在 DDPG 中处 理的是与连续动作交互的环境， $Q$ 网络的输入是状态和动作拼接后的向量, $Q$ 网络的输出是一个值，表示该状态动作对的价值。


接下来我们在倒立摆环境中训练 DDPG，并绘制其性能曲线。


可以发现 DDPG 在倒立摆环境中表现出很不错的效果，其学习速度非常快，并且不需要太多样本。有兴趣的读者可以尝试自行调节超参数（例如用于探索的高斯噪声参数），观察训练结果的变化。



## 小结

本章讲解了深度确定性策略梯度算法（DDPG），它是面向连续动作空间的深度确定性策略训练的典型算法。相比于它的先期工作，即确定性梯度算法（DPG），DDPG 加入了**目标网络和软更新**的方法，这对深度模型构建的价值网络和策略网络的稳定学习起到了关键的作用。DDPG 算法也被引入了多智能体强化学习领域，催生了 MADDPG 算法，我们会在后续的章节中对此展开讨论。

更多：# TD3：Double DQN -> DDPG(TD3)

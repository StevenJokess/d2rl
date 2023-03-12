

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-23 21:12:17
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-12 23:29:51
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# DDPG算法

# 简介

之前的章节介绍了基于策略梯度的算法 REINFORCE、Actor-Critic 以及两个改进算法——TRPO 和 PPO。这类算法有一个共同的特点：它们都是在线策略算法，这意味着它们的**样本效率**（sample efficiency）比较低。我们回忆一下 DQN 算法，DQN 算法直接估计最优函数 Q，可以做到离线策略学习，但是它只能处理动作空间有限的环境，这是因为它需要从所有动作中挑选一个值最大的动作。如果动作个数是无限的，是连续的，虽然我们可以像 8.3 节一样，将动作空间离散化，但这比较粗糙，无法精细控制。那有没有办法可以用类似的思想来处理动作空间无限的环境并且使用的是离线策略算法呢？本章要讲解的**深度确定性策略梯度**（deep deterministic policy gradient，DDPG）算法就是如此，它构造一个**确定性策略**，用梯度上升的方法来最大化 $Q$ 值。DDPG 也属于一种 Actor-Critic 算法。我们之前学习的 REINFORCE、TRPO 和 PPO 学习随机性策略，而本章的 DDPG 则学习一个**确定性策略**。总结，DDPG是同时学习确定性策略和 Q 函数的算法，即DDPG = DQN + DPG（deterministic policy gradient）。[3]

## 离散动作与连续动作的区别

- 离散动作：是可数的，例如：在 CartPole 环境中，可以有向左推小车、向右推小车两个动作。在 Frozen Lake 环境中，小乌龟可以有上、下、左、右4个动作。在雅达利的 Pong 游戏中，游戏有 6 个按键的动作可以输出。
- 连续动作：是不可数的。 在实际情况中，我们经常会遇到连续动作空间的情况，也就是输出的动作是不可数的。比如：推小车推力的大小、选择下一时刻方向盘转动的具体角度、给四轴飞行器的4个螺旋桨给的电压的大小。

[使用神经网络输出离散动作与连续动作](../img/disconcrete&continuous_action_output_by_NN.png)

如图 12.3 所示，要输出离散动作，我们就加一个 softmax 层来确保所有的输出是动作概率，并且所有的动作概率和为 1。要输出连续动作，我们一般可以在输出层加一层 tanh 函数。tanh 函数的作用就是把输出限制到 [−1,1] 。我们得到输出后，就可以根据实际动作的范围将其缩放，再输出给环境。比如神经网络输出一个浮点数 2.8，经过 tanh 函数之后，它就可以被限制在 [−1,1] 之间，输出 0.99。假设小车速度的范围是 [−2,2] ，我们就按比例从 [−1,1] 扩大到 [−2,2]，0.99 乘 2，最终输出的就是 1.98，将其作为小车的速度或者推小车的推力输出给环境。

## DPG算法(Deterministic Policy Gradient)

### Deterministic

顾名思义，DPG是一种deterministic的policy gradient算法。可以说，差不多在2014年以前，学者们都在发展随机**策略搜索**的方法。因为，大家都认为确定性策略梯度是不存在的。直到2014年，强化学习算法大神Silver在论文《Deterministic Policy Gradient Algorithms》中提出了确定性策略理论，策略搜索方法中才出现确定性策略这个方法。[7]



policy gradient算法的基本思想 是，用一个参数化的概率分布 $\pi_\theta(a \mid s)=P[a \mid s ; \theta]$ 来表示policy，并且由于policy是一个概率 分布，那么action $a$ 就是随机选取的，也就是所谓的Stochastic Policy Gradient。

DPG做的事情，就是摒弃了用概率分布表示policy的方法，转而用一个确定性的函数 $a=\mu_\theta(s)$ 表示policy。也就是说，给定当前的state $s$ ，选取的action $a$ 就是确定的。
相对于stochastic function，用deterministic function表示policy有其优点和缺点。优点就是， 从理论上可以证明， deterministic policy的梯度就是Q函数梯度的期望，这使得deterministic方 法在计算上比stochastic方法更高效；
但缺点也很明显，对于每个state，下一步的action是确定的。这就导致只能做exploitation而不能 做exploration。这可能也是为什么policy gradient一开始就采用stochastic算法的原因。
为了解决不能做exploration的问题，DPG采用了 off-policy的方法。也就是说，采样的policy和待 优化的policy是不同的: 其中采样的policy是stochastic的，而待优化的policy是deterministic 的。采样policy的随机性保证了充分的exploration。
值得一提的是，由于这篇文章是比较偏理论的，作者（至少在method部分) 并没有提到怎么去构 造这个采样的policy。DDPG将给出一个可以实用的构造方式。

### 重要结论——确定性策略梯度定理 (deterministic policy gradient theorem)：：

$$
\begin{aligned}
\nabla_{\theta^\mu} J & \approx \mathbb{E}_{s_t \sim \rho^\beta}\left[\left.\nabla_{\theta^\mu} Q\left(s, a \mid \theta^Q\right)\right|_{s=s_t, a=\mu\left(s_t \mid \theta^\mu\right)}\right] \\
& =\mathbb{E}_{s_t \sim \rho^\beta}\left[\left.\left.\nabla_a Q\left(s, a \mid \theta^Q\right)\right|_{s=s_t, a=\mu\left(s_t\right)} \nabla_{\theta^\mu} \mu\left(s \mid \theta^\mu\right)\right|_{s=s_t}\right]
\end{aligned}
$$

解释一下各个符号的含义:[5]

- $s$ 和 $a$ 分别是state和action，
- $Q$ 为 $\mathrm{Q}$ 函数，即action value函数， $\theta^\mu$ 为其参数；
- $J$ 表示的是初始状态分布下的期望回报，也就是给定初始状态的概率分布，期望能够获得的总 reward (可能要考虑折扣因子 $\gamma$ )，我们的目标就是使得 $J$ 越大越好；
- $s_t$ 表示在 $t$ 时刻的状态，而 $\rho^\beta$ 则表示在随机采样policy $\beta$ 之下，每个state被访问的概率分 布；
- $\mu$ 表示待优化的deterministic policy, $\theta^\mu$ 是它的参数。

理解公式：

- 第一行的通俗理解：期望回报对待优化的policy $\mu$ 的梯度 ( $\nabla_{\theta^\mu} J$ )，可以近似为在随机采样policy $\beta$ 的状态访问的 概率分布下 $\left(s_t \sim \rho^\beta\right)$ ， Q函数对 $\mu$ 的梯度 $\left(\left.\nabla_{\theta^\mu} Q\left(s, a \mid \theta^Q\right)\right|_{s=s_t, a=\mu\left(s_t \mid \theta^\mu\right)}\right)$ 的期望 ( $\mathbb{E}$ )
- 第二行的由来：对 $\left.\nabla_{\theta^\mu} Q\left(s, a \mid \theta^Q\right)\right|_{s=s_t, a=\mu\left(s_t \mid \theta^\mu\right)}$ 使用链式法则，即Q函数对 $\mu$ 的梯度，等于Q函数对 action $a$ 的梯度乘以action $a$ 对 $\mu$ 的梯度，就得到了上述公式的第二行。
- 左侧：$\nabla_{\theta^\mu} J$ 就是DPG的policy gradient，用这个policy gradient做梯度上升算法，即可优化policy $\mu$ ，使得期望回报最大化。

异策略随机策略梯度: $\nabla_\theta J_\beta\left(\pi_\theta\right)=E_{s \sim \rho^\beta, a \sim \beta}\left[\frac{\pi_\theta(a \mid s)}{\beta_\theta(a \mid s)} \nabla_\theta \log \pi_\theta(a \mid s) Q^\pi(s, a)\right]$
采样策略为 $\beta$.

为了给出确定性策略AC的方法，我们首先给出确定性策略梯度：

$$
\nabla_\theta J\left(\mu_\theta\right)=E_{s \sim \rho^\mu}\left[\left.\nabla_\theta \mu_\theta(s) \nabla_a Q^\mu(s, a)\right|_{a=\mu_\theta(s)}\right](8.5)
$$

(8.5)即为确定性策略梯度。跟随机策略梯度 (8.3) 相比，少了对动作的积分，多了回报函数 对动作的导数。

异策略确定性策略梯度为:

$$
\nabla_\theta J_\beta\left(\mu_\theta\right)=E_{s \sim \rho^\beta}\left[\left.\nabla_\theta \mu_\theta(s) \nabla_a Q^\mu(s, a)\right|_{a=\mu_\theta(s)}\right](8.6)
$$

比较 (8.6) 和（8.4) 我们发现，确定性策略梯度求解时少了重要性权重，这是因为重要性采 样是用简单的概率分布区估计复杂的概率分布，而确定性策略的动作是确定值不是概率分 布，另外确定性策略的值函数评估用的是Qlearning的方法，即用TD(0)来估计动作值函数并 忽略重要性权重。
有了 (8.6)，我们便可以得到确定性策略异策略AC算法的更新过程了:

$$
\begin{aligned}
& \delta_t=r_t+\gamma Q^w\left(s_{t+1}, \mu_\theta\left(s_{t+1}\right)\right)-Q^w\left(s_t, a_t\right) \\
& w_{t+1}=w_t+\alpha_w \delta_t \nabla_w Q^w\left(s_t, a_t\right) \\
& \theta_{t+1}=\theta_t+\left.\alpha_\theta \nabla_\theta \mu_\theta\left(s_t\right) \nabla_a Q^w\left(s_t, a_t\right)\right|_{a=\mu_\theta(s)}
\end{aligned}
$$

（8.7) 的第一行和第二行是利用值函数逼近的方法更新值函数参数，第三行是利用确定性策 略梯度的方法更新策略参数。

以上介绍的是确定性策略梯度方法，可以称为 DPG的方法。有了DPG，我们再讲DDPG。[7]


## DDPG算法

为了打破数据之间的相关性，DQN用了两个技巧：经验回放和独立的目标网络。DDPG的算法便是将这两条技巧用到了DPG算法中。[7]

之前我们学习的策略是随机性的，可以表示为 $a \sim \pi_\theta(\cdot \mid s)$ ；而如果策略是确定性的，则可以记为 $a=\mu_\theta(s)$ 。与策略梯度定理类似，我们可以推导出确定性策略梯度定理 (deterministic policy gradient theorem)：

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

- 随机噪声可以用 $\mathcal{N}$ 来表示，用随机的网络参数 $\omega$ 和 $\theta$ 分别初始化 Critic 网络 $Q_\omega(s, a)$ 和 Actor 网络 $\mu_\theta(s)$
- 复制相同的参数 $\omega^{-} \leftarrow \omega$ 和 $\theta^{-} \leftarrow \theta$ ，分别初始化目标网络 $Q_{\omega^{-}}$和 $\mu_{\theta^{-}}$ 初始化经验回放池 $R$
  - **for** 序列 $e=1 \rightarrow E$ **do** :
  - 初始化随机过程 $\mathcal{N}$ 用于动作探索
  - 获取环境初始状态 $s_1$
  - **for** 时间步 $t=1 \rightarrow T$ **do**:
    - 根据当前策略和噪声选择动作 $a_t=\mu_\theta\left(s_t\right)+\mathcal{N}$
    - 执行动作 $a_t$ ，获得奖励 $r_t$ ，环境状态变为 $s_{t+1}$
    - 将 $\left(s_t, a_t, r_t, s_{t+1}\right)$ 存储进回放池 $R$
    - 从 $R$ 中采样 $N$ 个元组 $\left\{\left(s_i, a_i, r_i, s_{i+1}\right)\right\}_{i=1, \ldots, N}$
    - 对每个元组，用目标网络计算 $y_i=r_i+\gamma Q_{\omega^{-}}\left(s_{i+1}, \mu_{\theta^{-}}\left(s_{i+1}\right)\right)$
    - 最小化目标损失 $L=\frac{1}{N} \sum_{i=1}^N\left(y_i-Q_\omega\left(s_i, a_i\right)\right)^2$ ，以此更新当前 Critic 网络
    - 计算采样的策略梯度，以此更新当前 Actor 网络：
    $$
    \left.\nabla_\theta J \approx \frac{1}{N} \sum_{i=1}^N \nabla_\theta \mu_\theta\left(s_i\right) \nabla_a Q_\omega\left(s_i, a\right)\right|_{a=\mu_\theta\left(s_i\right)}
    $$
    - 更新目标网络:
  - **end for**
- **end for**

## DDPG 代码实践

下面我们以倒立摆环境为例，结合代码详细讲解 DDPG 的具体实现。

code

对于策略网络和价值网络，我们都采用只有一层隐藏层的神经网络。策略网络的输出层用正切函数 $(y=\tanh x)$ 作为激活函数，这是因为正切函数的值域是 $[-1,1]$ ，方便按比例调整成环境可以接受的动作范围。在 DDPG 中处 理的是与连续动作交互的环境， $Q$ 网络的输入是状态和动作拼接后的向量, $Q$ 网络的输出是一个值，表示该状态动作对的价值。

接下来是 DDPG 算法的主体部分。在用策略网络采取动作的时候，为了更好地探索，我们向动作中加入高斯噪声。在 DDPG 的原始论文中，添加的噪声符合奥恩斯坦-乌伦贝克（Ornstein-Uhlenbeck，OU）随机过程：

$$
\Delta x_t=\theta\left(\mu-x_{t-1}\right)+\sigma W
$$

其中， $\mu$ 是均值， $W$ 是符合布朗运动的随机噪声， $\theta$ 和 $\sigma$ 是比例参数。可以看出， 当 $x(t-1)$ 偏离均值时， $x_t$ 的值会向均值靠拢。OU 随机过程的特点是在均值附 近做出线性负反馈，并有额外的干扰项。OU 随机过程是与时间相关的，适用于 有惯性的系统。在 DDPG 的实践中，不少地方仅使用正态分布的噪声。这里为 了简单起见，同样使用正态分布的噪声，感兴趣的读者可以自行改为 OU 随机过 程并观察效果。

code

接下来我们在倒立摆环境中训练 DDPG，并绘制其性能曲线。

code

可以发现 DDPG 在倒立摆环境中表现出很不错的效果，其学习速度非常快，并且不需要太多样本。有兴趣的读者可以尝试自行调节超参数（例如用于探索的高斯噪声参数），观察训练结果的变化。

## 小结

本章讲解了深度确定性策略梯度算法（DDPG），它是面向连续动作空间的深度确定性策略训练的典型算法。相比于它的先期工作，即确定性梯度算法（DPG），DDPG 加入了**目标网络和软更新**的方法，这对深度模型构建的价值网络和策略网络的稳定学习起到了关键的作用。DDPG 算法也被引入了多智能体强化学习领域，催生了 MADDPG 算法，我们会在后续的章节中对此展开讨论。

更多：双延迟确定性策略梯度算法TD3 (Twin Delayed Deep Deterministic policy gradient)：在DDPG的基础上, 引入性能更好的Double DQN, 取两个Critic之间的最小值来限制过拟合。

## 双延迟深度确定性策略梯度 TD3

虽然 DDPG 有时表现很好，但它对于超参数和其他类型的调整方面经常很敏感。如图 12.9 所示，DDPG常见的问题是已经学习好的 Q 函数开始显著地高估 Q 值，然后导致策略被破坏，因为它利用了 Q 函数中的误差。

图 12.9 DDPG的问题

我们可以使用实际的 Q 值与Q网络输出的 Q 值进行对比。实际的 Q 值可以用蒙特卡洛来算。根据当前的策略采样 1000 条轨迹，得到 G
G 后取平均值，进而得到实际的 Q 值。

双延迟深度确定性策略梯度（twin delayed DDPG，TD3）通过引入3个关键技巧来解决这个问题。

1. **截断的双 Q 学习 (clipped dobule Q-learning) **。TD3学习两个Q函数（因此名字中有 “twin")。TD3通过最小化均方 差来同时学习两个Q函数: $Q_{\phi_1}$ 和 $Q_{\phi_2}$ 。两个Q函数都使用 一个目标，两个Q函数中给出的较小的值会被作为如下的 Qtarget:
$$
y\left(r, s^{\prime}, d\right)=r+\gamma(1-d) \min _{i=1,2} Q_{\phi i, \operatorname{targ}}\left(s^{\prime}, a_{\mathrm{TD} 3}\left(s^{\prime}\right)\right)
$$
1. **延迟的策略更新 (delayed policy updates)** 。相关实验结果表明，同步训练动作网络和评价网络，却不使用目标网 络，会导致训练过程不稳定；但是仅固定动作网络时，评 价网络往往能够收敛到正确的结果。因此 TD3算法以较低的 频率更新动作网络，以较高的频率更新评价网络，通常每 更新两次评价网络就更新一次策略。
1. **目标策略平滑 (target policy smoothing)**。TD3引入了平滑化 (smoothing) 思想。TD3在目标动作中加入噪声，通 过平滑 $\mathrm{Q}$ 沿动作的变化，使策略更难利用 $\mathrm{Q}$ 函数的误差。

这 3 个技巧加在一起，使得性能相比基线 DDPG 有了大幅的提升。

目标策略平滑化的工作原理如下:

$$
a_{\text {TD3 }}\left(s^{\prime}\right)=\operatorname{clip}\left(\mu_{\theta, \operatorname{targ}}\left(s^{\prime}\right)+\operatorname{clip}(\epsilon,-c, c), a_{\text {low }}, a_{\text {high }}\right)
$$

其中 $\epsilon$ 本质上是一个噪声，是从正态分布中取样得到的，即 $\epsilon \sim$ $N(0, \sigma)$ 。目标策略平滑化是一种正则化方法。


如图 12.10 所示，我们可以将 TD3 算法与其他算法进行对比。TD3算法的作者自己实现的 深度确定性策略梯度（图中为our DDPG）和官方实现的 DDPG 的表现不一样，这说明 DDPG 对初始化和调参非常敏感。TD3对参数不是这么敏感。在TD3的论文中，TD3的性能比**软演员-评论员（soft actor-critic，SAC）**高。软演员-评论员又被译作软动作评价。但在SAC的论文中， SAC 的性能比TD3 高，这是因为强化学习的很多算法估计对参数和初始条件敏感。

TD3的作者给出了其对应[PyTorch的实现](https://github.com/sfujim/TD3/)，代码写得很棒，我们可以将其作为一个强化学习的标准库来学习。TD3以异策略的方式训练确定性策略。由于该策略是确定性的，因此如果智能体要探索策略，则一开始它可能不会尝试采取足够广泛的动作来找到有用的学习信号。为了使TD3策略更好地探索，我们在训练时在它们的动作中添加了噪声，通常是不相关的均值为0的高斯噪声。为了便于获取高质量的训练数据，我们可以在训练过程中减小噪声的大小。 在测试时，为了查看策略对所学知识的利用程度，我们不会在动作中增加噪声。[6]

[1]:
[2]: http://www.c-s-a.org.cn/html/2020/12/7701.html#outline_anchor_19
[3]: https://spinningup.readthedocs.io/zh_CN/latest/spinningup/rl_intro2.html
[4]: https://github.com/sfujim/TD3/
[5]: https://zhuanlan.zhihu.com/p/337976595#2.1%20Story
[6]: https://datawhalechina.github.io/easy-rl/#/chapter12/chapter12
[7]: https://zhuanlan.zhihu.com/p/26441204


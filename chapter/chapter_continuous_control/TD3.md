

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-04 00:45:09
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-04 00:45:16
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# 双延迟深度确定性策略梯度 TD3

双延迟确定性策略梯度算法TD3 (Twin Delayed Deep Deterministic policy gradient)：在DDPG的基础上, 引入性能更好的Double DQN, 取两个Critic之间的最小值来限制过拟合。

## 背景

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

TODO：https://spinningup.openai.com/en/latest/algorithms/td3.html

[6]: https://datawhalechina.github.io/easy-rl/#/chapter12/chapter12

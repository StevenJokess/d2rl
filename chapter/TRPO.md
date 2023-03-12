

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-24 01:59:33
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-13 01:25:22
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# TRPO 算法

## 简介

本书之前介绍的基于策略的方法包括策略梯度算法和 Actor-Critic 算法。这些方法虽然简单、直观，但在实际应用过程中会遇到训练不稳定的情况。回顾一下基于策略的方法：参数化智能体的策略，并设计衡量策略好坏的目标函数，通过*梯度上升的方法来最大化这个目标函数*，使得策略最优。具体来说，假设  表示策略  的参数，定义，基于策略的方法的目标是找到 ，策略梯度算法主要沿着  方向迭代更新策略参数 。但是这种算法有一个明显的缺点：当策略网络是深度模型时，*沿着策略梯度更新参数，很有可能由于步长太长，策略突然显著变差*，进而影响训练效果。

针对以上问题，我们考虑在更新时找到一块**信任区域**（trust region），在这个区域上更新策略时能够得到某种策略性能的安全性保证，这就是**信任区域策略优化**（trust region policy optimization，TRPO）算法的主要思想。TRPO 算法在 2015 年被提出，它在理论上能够保证策略学习的性能单调性，并在实际应用中取得了比策略梯度算法更好的效果。

图7.4 代价函数推导

注意，这时状态s的分布由新的策略产生，对新的策略严重依赖。

TRPO= NPG + Linesearch + monotonic improvement theorem!

## 技巧

### 第一个技巧

这时，我们引入TRPO的第一个技巧对状态分布进行处理。我们忽略状态分布的变化，依然采 用旧的策略所对应的状态分布。这个技巧是对原代价函数的第一次近似。其实，当新旧参数 很接近时，我们将用旧的状态分布代替新的状态分布也是合理的。这时，原来的代价函数变成了:

$$
L_\pi(\tilde{\pi})=\eta(\pi)+\sum_s \rho_\pi(s) \sum_a \tilde{\pi}(a \mid s) A^\pi(s, a)(7.5)
$$
我们再看(7.5)式的第二项策略部分，这时的动作a是由新的策略 $\tilde{\pi}$ 产生。可是新的策略 $\tilde{\pi}$ 是 带参数 $\theta$ 的，这个参数是末知的，因此无法用来产生动作。这时，我们引入TRPO的第二个技巧。

##

### 第三个技巧：

在约束条件中，利用平均 $K L$ 散度代替最大KL散度，即：

$subject to \bar{D}_{K L}^{\rho_{\theta_{o l d}}}\left(\theta_{o l d}, \theta\right) \leq \delta$

### 第四个技巧：

$s \sim \rho_{\theta_{\text {old }}} \rightarrow s \sim \pi_{\theta_{\text {old }}}$
最终TRPO问题化简为:
$$
\begin{aligned}
& \operatorname{maximize}_\theta E_s \pi_{\theta_{\text {old }}, a \pi_{\theta_{\text {old }}}}\left[\frac{\pi_\theta(a \mid s)}{\pi_{\theta_{\theta l d}}(a \mid s)} A_{\theta_{\text {old }}}(s, a)\right] \\
& \text { subject to } E_{s \pi_{\theta_{\text {old }}}}\left[D_{K L}\left(\pi_{\theta_{\text {old }}}(\cdot \mid s)|| \pi_\theta(\cdot \mid s)\right)\right] \leq \delta \\
&
\end{aligned}
$$


TRPO在车杆环境中很快收敛，展现了十分优秀的性能效果。

接下来我们尝试倒立摆环境，由于它是与连续动作交互的环境，我们需要对上面的代码做一定的修改。对于策略网络，因为环境是连续动作的，所以策略网络分别输出表示动作分布的高斯分布的均值和标准差。




###

## TRPO 代码实践

本节将使用支持与离散和连续两种动作交互的环境来进行 TRPO 的实验。我们使用的第一个环境是车杆（CartPole），第二个环境是倒立摆（Inverted Pendulum）。

首先导入一些必要的库。

## 总结

本章讲解了 TRPO 算法，并分别在离散动作和连续动作交互的环境中进行了实验。TRPO 算法属于在线策略学习方法，每次策略训练仅使用上一轮策略采样的数据，是基于策略的深度强化学习算法中十分有代表性的工作之一。直觉性地理解，TRPO 给出的观点是：由于策略的改变导致数据分布的改变，这大大影响深度模型实现的策略网络的学习效果，所以通过划定一个**可信任的策略学习区域**，保证策略学习的稳定性和有效性。

TRPO 算法是比较难掌握的一种强化学习算法，需要较好的数学基础。读者若在学习过程中遇到困难，可自行查阅相关资料。TRPO 有一些后续工作，其中最著名的当属 PPO，我们将在第 12 章进行介绍。



[1]:
[2]: https://www.cnblogs.com/kailugaji/p/15388913.html
[3]: https://zhuanlan.zhihu.com/p/26308073

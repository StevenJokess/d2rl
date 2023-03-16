

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-24 01:59:33
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-16 21:52:54
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# TRPO 算法

## 简介

本书之前介绍的基于策略的方法包括策略梯度算法和 Actor-Critic 算法。这些方法虽然简单、直观，但在实际应用过程中会遇到训练不稳定的情况。回顾一下基于策略的方法：参数化智能体的策略，并设计衡量策略好坏的目标函数，通过*梯度上升的方法来最大化这个目标函数*，使得策略最优。具体来说，假设  表示策略  的参数，定义，基于策略的方法的目标是找到 ，策略梯度算法主要沿着  方向迭代更新策略参数 。但是这种算法有一个明显的缺点：当策略网络是深度模型时，*沿着策略梯度更新参数，很有可能由于步长太长，策略突然显著变差*，进而影响训练效果。

针对以上问题，我们考虑在更新时找到一块**信任区域**（trust region），在这个区域上更新策略时能够得到某种策略性能的安全性保证，这就是**信任区域策略优化**（trust region policy optimization，TRPO）算法的主要思想。TRPO 算法在 2015 年被提出，它在理论上能够保证策略学习的性能单调性，并在实际应用中取得了比策略梯度算法更好的效果。

## 策略目标



图7.4 代价函数推导

注意，这时状态s的分布由新的策略产生，对新的策略严重依赖。

TRPO= NPG + Linesearch + monotonic improvement theorem!



这里的不等式约束定义了策略空间中的一个 KL 球，被称为信任区域。在这个区域中，可以认为当前学习策略和环境交互的状态分布与上一轮策略最后采样的状态分布一致，进而可以基于一步行动的重要性采样方法使当前学习策略稳定提升。TRPO 背后的原理如图 11-1 所示。


## 技巧

### 第一个技巧

这时，我们引入TRPO的第一个技巧对状态分布进行处理。我们忽略状态分布的变化，依然采 用旧的策略所对应的状态分布。这个技巧是对原代价函数的第一次近似。其实，当新旧参数很接近时，我们将用旧的状态分布代替新的状态分布也是合理的。这时，原来的代价函数变成了:

$$
L_\pi(\tilde{\pi})=\eta(\pi)+\sum_s \rho_\pi(s) \sum_a \tilde{\pi}(a \mid s) A^\pi(s, a)(7.5)
$$
我们再看(7.5)式的第二项策略部分，这时的动作a是由新的策略 $\tilde{\pi}$ 产生。可是新的策略 $\tilde{\pi}$ 是 带参数 $\theta$ 的，这个参数是末知的，因此无法用来产生动作。这时，我们引入TRPO的第二个技巧。

### 第二个技巧：

TRPO的第二个技巧是利用重要性采样对动作分布进行的处理。

#### 重要性采样的概念

$$
\begin{aligned}
E_{x \sim p i x)}[f(x)] & =\int p(x) f(x) d x \\
& =\int \frac{q(x)}{q(x)} p(x) f(x) d x \\
& =\int q(x) \frac{p(x)}{q(x)} f(x) d x \\
& =E_{x \sim q(x)}\left[\frac{p(x)}{q(x)} f(x)\right]
\end{aligned}
$$

我们在已知 $q$ 的分布后，可以使用上述公式计算出从 $p$ 分布的期望值。也就可以使用 $q$ 来对于 $p$ 进行采样了，即为重要性采样。[4]

> *使用重要性采样时需要注意的问题有哪些？*
>
> 我们可以在重要性采样中将 $p$ 替换为任意的 $q$ ，但是本质上要 求两者的分布不能差太多，即使我们补偿了不同数据分布的权 重 $\frac{p(x)}{q(x)}$ 。 $E_{x \sim p}[f(x)]=E_{x \sim q}\left[f(x) \frac{p(x)}{q(x)}\right]$ ，当我们对于两 者的采样次数都比较多时，最终的结果会是较为接近的。但是 通常我们不会取理想数量的采样数据，所以如果两者的分布相 差较大，最后结果的方差将会很大。

$$
\sum_a{\tilde{\pi}_{\theta}\left(a|s_n\right)A_{\theta_{old}}\left(s_n,a\right)=E_{a~q}\left[\frac{\tilde{\pi}_{\theta}\left(a|s_n\right)}{q\left(a|s_n\right)}A_{\theta_{old}}\left(s_n,a\right)\right]}
$$

通过利用两个技巧，我们再利用 $\frac{1}{1-\gamma}E_{s~\rho_{\theta_{old}}}\left[\cdots\right] 代替 \sum_s{\rho_{\theta_{old}}\left(s\right)}\left[\cdots\right] ；取 q\left(a|s_n\right)=\pi_{\theta_{old}}\left(a|s_n\right) $；

替代回报函数变为：

$ L_{\pi}\left(\tilde{\pi}\right)=\eta\left(\pi\right)+E_{s~\rho_{\theta_{old}},a~\pi_{\theta_{old}}}\left[\frac{\tilde{\pi}_{\theta}\left(a|s\right)}{\pi_{\theta_{old}}\left(a|s\right)}A_{\theta_{old}}\left(s,a\right)\right] $ (7.6)

接下来，我们看一下替代回报函数（7.6）和原回报函数（7.4）有什么关系

通过比较我们发现，（7.4）和（7.6）唯一的区别是状态分布的不同。将 $ L_{\pi}\left(\tilde{\pi}\right)\textrm{，}\eta\left(\tilde{\pi}\right) $ 都看成是策略 $\tilde{\pi}$ 的函数，则 $ L_{\pi}\left(\tilde{\pi}\right)\textrm{，}\eta\left(\tilde{\pi}\right) $ 在策略 $\pi_{\theta_{old}}$ 处一阶近似，即：

$ L_{\pi_{\theta_{old}}}\left(\pi_{\theta_{old}}\right)=\eta\left(\pi_{\theta_{old}}\right) \\ \nabla_{\theta}L_{\pi_{\theta_{old}}}\left(\pi_{\theta}\right)|_{\theta =\theta_{old}}=\nabla_{\theta}\eta\left(\pi_{\theta}\right)|_{\theta =\theta_{old}} $ (7.7)

用图来表示为：


图7.5 回报函数与替代回报函数示意图

在 $\theta_{old}$ 附近，能改善L的策略也能改善原回报函数。问题是步长多大呢？

再次引入第二个重量级的不等式

\[ \eta\left(\tilde{\pi}\right)\geqslant L_{\pi}\left(\tilde{\pi}\right)-CD_{KL}^{\max}\left(\pi ,\tilde{\pi}\right) \\ where\ C=\frac{2\varepsilon\gamma}{\left(1-\gamma\right)^2} \](7.8)

其中 $D_{KL}\left(\pi ,\tilde{\pi}\right)$ 是两个分布的KL散度。我们在这里看一看，该不等式给了我们什么启示。

首先，该不等式给了 $\eta\left(\tilde{\pi}\right)$ 的下界，我们定义这个下界为 $M_i\left(\pi\right)=L_{\pi_i}\left(\pi\right)-CD_{KL}^{\max}\left(\pi_i,\pi\right)$ ，

下面利用这个下界，我们证明策略的单调性：

\[ \eta\left(\pi_{i+1}\right)\geqslant M_i\left(\pi_{i+1}\right) \]

且 \[ \eta\left(\pi_i\right)=M_i\left(\pi_i\right) \]

则： \[ \eta\left(\pi_{i+1}\right)-\eta\left(\pi_i\right)\geqslant M_i\left(\pi_{i+1}\right)-M\left(\pi_i\right) \]

如果新的策略 \pi_{i+1} 能使得 M_i 最大，那么有不等式 \[ M_i\left(\pi_{i+1}\right)-M\left(\pi_i\right)\geqslant 0 \] ，则 \[ \eta\left(\pi_{i+1}\right)-\eta\left(\pi_i\right)\geqslant 0 \] ，这个使得 M_i 最大的新的策略就是我们一直在苦苦找的要更新的策略。那么这个策略如何得到呢？

该问题可形式化为：

$maximize_{\theta}\left[L_{\theta_{old}}\left(\theta\right)-CD_{KL}^{\max}\left(\theta_{old},\theta\right)\right]$
如果利用惩罚因子C则每次迭代步长很小，因此问题可转化为：

$maximize_{\theta}E_{s~\rho_{\theta_{old}},a~\pi_{\theta_{old}}}\left[\frac{\pi_{\theta}\left(a|s\right)}{\pi_{\theta_{old}}\left(a|s\right)}A_{\theta_{old}}\left(s,a\right)\right] \\ subject\ to\\ D_{KL}^{\max}\left(\theta_{old},\theta\right)\le\delta$ (7.9)

需要注意的是，因为有无穷多的状态，因此约束条件 D_{KL}^{\max}\left(\theta_{old},\theta\right) 有无穷多个。问题不可解。

### 第三个技巧：

在约束条件中，利用平均 $KL$ 散度代替最大KL散度，即：

subject to $\bar{D}_{K L}^{\rho_{\theta_{o l d}}}\left(\theta_{o l d}, \theta\right) \leq \delta$

### 第四个技巧：

$s \sim \rho_{\theta_{\text {old }}} \rightarrow s \sim \pi_{\theta_{\text {old }}}$

最终TRPO问题化简为:

$$
\begin{aligned}
& \operatorname{maximize}_\theta E_{s \sim \pi_{\theta_{\text {old}}}, a \sim \pi_{\theta_{\text {old}}}}\left[\frac{\pi_\theta(a \mid s)}{\pi_{\theta_{\theta l d}}(a \mid s)} A_{\theta_{\text {old }}}(s, a)\right] \\
& \text { subject to } E_{s \sim\pi_{\theta_{\text {old }}}}\left[D_{K L}\left(\pi_{\theta_{\text {old }}}(\cdot \mid s)|| \pi_\theta(\cdot \mid s)\right)\right] \leq \delta \\
&
\end{aligned}
$$

## 伪代码（Pseudocode）

![TRPO的伪代码[4]](../img/trpo_Pseudocode.svg)



## TRPO 代码实践

本节将使用支持与离散和连续两种动作交互的环境来进行 TRPO 的实验。我们使用的第一个环境是车杆（CartPole），第二个环境是倒立摆（Inverted Pendulum）。

首先导入一些必要的库。

code

接下来在车杆环境中训练 TRPO，并将结果可视化。

code

TRPO在车杆环境中很快收敛，展现了十分优秀的性能效果。

接下来我们尝试倒立摆环境，由于它是与连续动作交互的环境，我们需要对上面的代码做一定的修改。对于策略网络，因为环境是连续动作的，所以策略网络分别输出表示动作分布的高斯分布的均值和标准差。

code

## 总结

本章讲解了 TRPO 算法，并分别在离散动作和连续动作交互的环境中进行了实验。TRPO 算法属于在线策略学习方法，每次策略训练仅使用上一轮策略采样的数据，是基于策略的深度强化学习算法中十分有代表性的工作之一。直觉性地理解，TRPO 给出的观点是：由于策略的改变导致数据分布的改变，这大大影响深度模型实现的策略网络的学习效果，所以通过划定一个**可信任的策略学习区域**，保证策略学习的稳定性和有效性。

TRPO 算法是比较难掌握的一种强化学习算法，需要较好的数学基础。读者若在学习过程中遇到困难，可自行查阅相关资料。TRPO 有一些后续工作，其中最著名的当属 PPO，我们将在第 12 章进行介绍。



[1]: https://hrl.boyuai.com/chapter/2/trpo%E7%AE%97%E6%B3%95/#115-%E7%BA%BF%E6%80%A7%E6%90%9C%E7%B4%A2
[2]: https://www.cnblogs.com/kailugaji/p/15388913.html
[3]: https://zhuanlan.zhihu.com/p/26308073
[4]: https://spinningup.openai.com/en/latest/algorithms/trpo.html

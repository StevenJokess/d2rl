

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-02-25 23:21:39
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-02-28 21:53:19
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# PPO 算法

第11章介绍的 TRPO 算法在很多场景上的应用都很成功，但是我们也发现它的计算过程非常复杂，每一步更新的运算量非常大。于是，TRPO 算法的改进版—— 近端策略优化算法(Proximal Policy Optimization Algorithms,PPO) [3]算法在 2017 年被提出，PPO 基于 TRPO 的思想，但是其算法实现更加简单。并且大量的实验结果表明，与 TRPO 相比，PPO 能学习得一样好（甚至更快），这使得 PPO 成为非常流行的强化学习算法。如果我们想要尝试在一个新的环境中使用强化学习算法，那么 PPO 就属于可以首先尝试的算法。

回忆一下 TRPO 的优化目标：

$$
\begin{aligned}
\max _\theta & \mathbb{E}_{s \sim \nu_{\theta_k}} \mathbb{E}_{a \sim \pi_{\theta_k}(\cdot \mid s)}\left[\frac{\pi_\theta(a \mid s)}{\pi_{\theta_k}(a \mid s)} A^{\pi_{\theta_k}}(s, a)\right] \\
\text { s.t. } & \mathbb{E}_{s \sim \nu^{\pi_k}}\left[D_{K L}\left(\pi_{\theta_k}(\cdot \mid s), \pi_\theta(\cdot \mid s)\right)\right] \leq \delta
\end{aligned}
$$[3]

需要限制新旧策略使两者差异不能太大，TRPO通过添加新旧策略的KL约束项，而PPO是限制两者比率的变化范围

TRPO 使用泰勒展开近似、共轭梯度、线性搜索等方法直接求解。PPO 的优化目标与 TRPO 相同，但 PPO 用了一些相对简单的方法来求解。具体来说，PPO 有两种形式，一是 PPO-惩罚，二是 PPO-截断，我们接下来对这两种形式进行介绍。[2]



## PPO-惩罚

PPO-惩罚（PPO-Penalty）用拉格朗日乘数法直接将 KL 散度的限制放进了目标函数中，这就变成了一个无约束的优化问题，在迭代的过程中不断更新 KL 散度前的系数。即：

$$
\underset{\theta}{\arg \max } \mathbb{E}_{s \sim \nu}{ }^{\pi_{\theta_k}} \mathbb{E}_{a \sim \pi_{\theta_k}(\cdot \mid s)}\left[\frac{\pi_\theta(a \mid s)}{\pi_{\theta_k}(a \mid s)} A^{\pi_{\theta_k}}(s, a)-\beta D_{K L}\left[\pi_{\theta_k}(\cdot \mid s), \pi_\theta(\cdot \mid s)\right]\right]
$$

令 $d_k=D_{K L}^{\nu^{\pi_k}}\left(\pi_{\theta_k}, \pi_\theta\right) ， \beta$ 的更新规则如下:

1. 如果 $d_k<\delta / 1.5$ ，那么 $\beta_{k+1} = \beta_k / 2$
2. 如果 $d_k>\delta \times 1.5$ ，那么 $\beta_{k+1} = \beta_k \times 2$
3. 否则 $\beta_{k+1} = \beta_k$

其中，$\delta$ 是事先设定的一个超参数，用于限制学习策略和之前一轮策略的差距。

## PPO-截断

PPO 的另一种形式 PPO-截断（PPO-Clip）更加直接，它在目标函数中进行限制，以保证新的参数和旧的参数的差距不会太大，即：

$$
\underset{\theta}{\arg \max } \mathbb{E}_{s \sim \nu^{\pi_{\theta_k}}} \mathbb{E}_{a \sim \pi_{\theta_k}(\cdot \mid s)}\left[\min \left(\frac{\pi_\theta(a \mid s)}{\pi_{\theta_k}(a \mid s)} A^{\pi_{\theta_k}}(s, a), \operatorname{clip}\left(\frac{\pi_\theta(a \mid s)}{\pi_{\theta_k}(a \mid s)}, 1-\epsilon, 1+\epsilon\right) A^{\pi_{\theta_k}}(s, a)\right)\right]
$$

其中 $\operatorname{clip}(x, l, r):=\max (\min (x, r), l)$ ，即

TODO:？把 $x$ 限制在 $[l, r]$ 内。

上式中 $\epsilon$ 是一 个超参数，表示进行截断 (clip) 的范围。

如果 $A^{\pi_{\theta_k}}(s, a)>0$ ，说明这个动作的价值高于平均，最大化这个式子会增大 $\frac{\pi_\theta(a \mid s)}{\pi_{\theta_k}(a \mid s)}$ ，但不会让其超过 $1+\epsilon_{\circ}$ 反之，如果 $A^{\pi_{\theta_k}}(s, a)<0$ ，最大化这个式子会减小 $\frac{\pi_\theta(a \mid s)}{\pi_{\theta_k}(a \mid s)}$ ，但不会让其超过 $1-\epsilon$ 。如图 12-1 所示。





## PPO 代码实践


与 TRPO 相同，我们仍然在车杆和倒立摆两个环境中测试 PPO 算法。大量实验表明，PPO-截断总是比 PPO-惩罚表现得更好。因此下面我们专注于 PPO-截断的代码实现。

首先导入一些必要的库，并定义策略网络和价值网络。


接下来在车杆环境中训练 PPO 算法。

倒立摆是与连续动作交互的环境，同 TRPO 算法一样，我们做一些修改，让策略网络输出连续动作高斯分布（Gaussian distribution）的均值和标准差。后续的连续动作则在该高斯分布中采样得到


创建环境`Pendulum-v0`，并设定随机数种子以便重复实现。接下来我们在倒立摆环境中训练 PPO 算法。

## 总结

PPO 是 TRPO 的一种改进算法，它在实现上简化了 TRPO 中的**复杂计算**，并且它在实验中的性能大多数情况下会比 TRPO 更好，因此目前常被用作一种常用的基准算法。需要注意的是，TRPO 和 PPO 都属于在线策略学习算法，即使优化目标中包含重要性采样的过程，但其只是用到了上一轮策略的数据，而不是过去所有策略的数据。

PPO 是 TRPO 的第一作者 John Schulman 从加州大学伯克利分校博士毕业后在 OpenAI 公司研究出来的。通过对 TRPO 计算方式的改进，PPO 成为了最受关注的深度强化学习算法之一，并且其论文的引用量也超越了 TRPO。

### 重要性采样的概念

$$
\begin{aligned}
E_{x \sim p i x)}[f(x)] & =\int p(x) f(x) d x \\
& =\int \frac{q(x)}{q(x)} p(x) f(x) d x \\
& =\int q(x) \frac{p(x)}{q(x)} f(x) d x \\
& =E_{x \sim q(x)}\left[\frac{p(x)}{q(x)} f(x)\right]
\end{aligned}
$$

我们在已知 $q$ 的分布后，可以使用上述公式计算出从 $p$ 分布的期望值。也就可以使用 $q$ 来对于 $p$ 进行采样了，即为重要性采样。[4]




[1]: https://hrl.boyuai.com/chapter/2/ppo%E7%AE%97%E6%B3%95
[2]: https://www.cnblogs.com/kailugaji/p/15401383.html#_lab2_0_1
[3]: https://www.cnblogs.com/kailugaji/p/15396437.html
[4]: http://rail.eecs.berkeley.edu/deeprlcourse/static/slides/lec-5.pdf

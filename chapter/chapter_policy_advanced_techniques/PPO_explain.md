

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-05-24 01:47:58
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-24 01:53:58
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
算法描述

根据文1，最大期望回报表达如下：

$$
\bar{R}_{\theta} = E_{\tau\sim p_{\theta}(\tau)}[R(\tau)] = \sum_{\tau}p_{\theta}(\tau)R(\tau)
$$

现在我们希望有一个新的策略 q，用来产生 \tau ，原有策略 p 用来学习，那么我们可以对 \bar{R}_{\theta} 做如下修改：

$$
\bar{R}_{\theta} = \sum_{\tau}q_{\theta'}(\tau)\frac{p_{\theta}(\tau)}{q_{\theta'}(\tau)}R(\tau) = E_{\tau\sim q_{\theta'}(\tau)}[\frac{p_{\theta}(\tau)}{q_{\theta'}(\tau)}R(\tau)]
$$

（Importance Sampling）


为了写起来方便，我们用 g 来表示文1里的 $\nabla \bar{R}(\theta) $ ，那么新的 g 如下：

$$
g = \nabla_{\theta} \sum_{\tau}q_{\theta'}(\tau)\frac{p_{\theta}(\tau)}{q_{\theta'}(\tau)}R(\tau) \\ \ \ = \frac{1}{m}\sum_{i=1}^{m}R(\tau^{(i)})\sum_{t=1}^{T}\frac{p_{\theta} (\tau^{(i)})}{q_{\theta'}(\tau^{(i)})}\nabla_{\theta}log\ p_\theta(a_{t}^{(i)}|s_{t}^{(i)}) \\ \ \ = \frac{1}{m}\sum_{i=1}^{m}R(\tau^{(i)})\sum_{t=1}^{T}\frac{p_{\theta}(\tau^{(i)})}{q_{\theta'}(\tau^{(i)})}\frac{\nabla_{\theta}p_\theta(a_{t}^{(i)}|s_{t}^{(i)})}{p_\theta(a_{t}^{(i)}|s_{t}^{(i)})} \\ \ \ = \frac{1}{m}\sum_{i=1}^{m}R(\tau^{(i)})\sum_{t=1}^{T} \frac{\prod_{t=0}^{T}p_{\theta}(a_{t}^{(i)}|s_{t}^{(i)})p(s_{t+1}^{(i)}|s_{t}^{(i)}, a_{t}^{(i)})} {\prod_{t=0}^{T}q_{\theta'}(a_{t}^{(i)}|s_{t}^{(i)})q(s_{t+1}^{(i)}|s_{t}^{(i)}, a_{t}^{(i)})} \frac{\nabla_{\theta}p_\theta(a_{t}^{(i)}|s_{t}^{(i)})}{p_\theta(a_{t}^{(i)}|s_{t}^{(i)})} \\ \ \ \approx \frac{1}{m}\sum_{i=1}^{m}R(\tau^{(i)})\sum_{t=1}^{T} \frac{\nabla_{\theta}p_\theta(a_{t}^{(i)}|s_{t}^{(i)})}{q_\theta'(a_{t}^{(i)}|s_{t}^{(i)})}
$$

这里对 g 取了近似，那么上面的 $\bar{R}_{\theta} $ 也可以近似为：

$$
\bar{R}_{\theta} \approx E_{\tau\sim q_{\theta'}(\tau)}[\frac{p_\theta(a_{t}|s_{t})}{q_\theta'(a_{t}|s_{t})}R(\tau)]
$$

这就是论文里说的 Surrogate Objective[1][2]，那这里是不是直接求导，使得新的期望回报值最大就行了吗？

当然不是！注意上面 g 近似成立的前提是 p 和 q 的值差距不大，如果差距太大的话，等式就不成立了，求导也没有意义，所以我们需要对 Surrogate Objective 进行截断，让 p 和 q 的值不要差距太大。

所以有了 Clipped Surrogate Objective[1][3]：

$$
E_{\tau\sim q_{\theta'}(\tau)}[min(\frac{p_\theta(a_{t}|s_{t})}{q_\theta'(a_{t}|s_{t})}R(\tau), clip(\frac{p_\theta(a_{t}|s_{t})}{q_\theta'(a_{t}|s_{t})}, 1-\epsilon, 1 + \epsilon)R(\tau))]
$$

clip 代表把 $\frac{p_\theta(a_{t}|s_{t})}{q_\theta'(a_{t}|s_{t})}$ 限制在区间 $(1- \epsilon, 1 + \epsilon)$。

那么当 R > 0 时，Surrogate Objective 等同于：

$$
E_{\tau\sim q_{\theta'}(\tau)}[min(\frac{p_\theta(a_{t}|s_{t})}{q_\theta'(a_{t}|s_{t})}R(\tau), (1 + \epsilon)R(\tau))]
$$

这就确保会增大 $p_\theta(a_{t}|s_{t})$ 但最大不会让 $\frac{p_\theta(a_{t}|s_{t})}{q_\theta'(a_{t}|s_{t})}$ 大于 $1 + \epsilon$

当 R < 0，Surrogate Objective 等同于：

$$
E_{\tau\sim q_{\theta'}(\tau)}[min(\frac{p_\theta(a_{t}|s_{t})}{q_\theta'(a_{t}|s_{t})}R(\tau), (1-\epsilon)R(\tau))]
$$

这就确保会减小 $p_\theta(a_{t}|s_{t}) $ 但最小不会让 $\frac{p_\theta(a_{t}|s_{t})}{q_\theta'(a_{t}|s_{t})}$ 小于 $1 - \epsilon$

从而满足了让 p 和 q 的值不要差距太大的约束条件。

再把 R 替换成文一里提到的 Credit Assignment 的形式，我们对 REINFORCE 的算法的优化暂时到这里。

## 缺点

总结一下，我们用 Clipped Surrogate Objective 和 Credit Assignment 让 REINFORCE 可以做 off-policy，并且让 R 的表示更合理，但是我们再次看一下 R 现在的表示方法，虽然更合理了，但是还是有可以优化的地方。

$$
R_{t}(\tau) = \sum_{t=t(a)}^{T}\gamma^{t-t(a)}r_{t},\gamma \in [0,1]
$$

虽然对每个 a 都有相应的 R，但是回到最开始 a 是怎么来的，每一步的 a 是采样来的，所以这种 Monte-Carlo 式的评估 R 的方式会引入大的方差。

[1]: https://zhuanlan.zhihu.com/p/574810519

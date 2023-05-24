

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-23 21:55:25
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-25 03:03:55
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 策略搜索

## 确定性策略

要想完全理解本章的内容需要熟练掌握前8章的要点，并且假设读者对DQN网络很熟悉。

我们先从图9.1策略搜索方法的分类开始。从图中我们可以看到，无模型的策略搜索方法可以分为随机策略搜索方法和确定性策略搜索方法。其中随机策略搜索方法又发展出了很多算法。可以说，2014年以前，学者们都在发展随机策略搜索的方法，因为大家都认为确定性策略梯度是不存在的。直到2014年，强化学习算法大神Silver在论文Deterministic Policy Gradient  Algorithms中提出了确定性策略理论，策略搜索方法中提出确定性策略的方法。2015年，DeepMind的大神们又将该理论与DQN的成功经验结合，在论文Continuous Control with Deep Reinforcement Learning中提出了DDPG算法。本章以这两篇论文为素材，向大家介绍确定性策略。[2]

## 直接策略搜索方法

直接策略搜索方法实际上是对策略 $\pi$ 进行参数化表示，参数为 $\theta$ 。用 $\tau$ 表示一组状态-行为序列 $s_0,u_0,...,s_H,u_H$ ， $R(\tau)=\sum_{t=0}^H R(s_t,u_t)$ 表示轨迹 $\tau$ 的回报， $P(\tau,\theta)$ 表示轨迹 $\tau$ 出现的概率，则强化学习的目标函数表示为:

$$
U(\theta)=E(\sum_{t=0}^H R(s_t,u_t);\pi_{\theta})=\sum_{\tau} P(\tau;\theta)R(\tau)
$$

那么策略搜索方法则变成了一个优化问题，目的就是找到最优参数 $\theta$ ，使得最大化目标函数，即 $\max \limits_{\theta} U(\theta) $。

既然是最优化问题，关键点就是求取策略梯度 $\nabla_{\theta}U(\theta)$ ,这里用到一个技巧 $\nabla_{\theta}P(\tau;\theta)=\frac{\nabla_{\theta}P(\tau;\theta)}{P(\tau;\theta)}$ ，则有:

$$
\begin{aligned}
\nabla_{\theta}U(\theta)=\nabla_{\theta}\sum_r P(\tau;\theta)R(\tau) & =\sum_r P(\tau;\theta)\frac{\nabla_{\theta}P(\tau;\theta)}{P(\tau;\theta)}R(\tau) \\
& =\sum_r P(\tau;\theta)\nabla_{\theta}\log P(\tau;\theta)R(\tau) \\
& =E[\log P(\tau;\theta)R(\tau)] \\
\end{aligned}
$$


也就是说求取梯度变成了求 \log P(\tau;\theta)R(\tau) 的期望。这个时候可以利用经验平均估计，即采样 m 条轨迹后，策略梯度为:

$\nabla_{\theta}U(\theta)=\frac{1}{m}\sum_{i=1}^m \nabla_{\theta}\log P(\tau;\theta)R(\tau)$

轨迹的似然率可表示为:

$P(\tau;\theta)=\sum_{t=0}^H P(s_{t+1}|s_t,u_t)\pi_{\theta}(u_t|s_t)$

其中 P(s_{t+1}|s_t,u_t) 在状态 s_t 选择动作 u_t 后跳转的下一个状态 s_{t+1} 的概率，为转移概率，无关参数 \theta 。故有：

$\nabla_{\theta} \log P(\tau;\theta)=\nabla_{\theta}[\sum_{t=0}^H \log \pi_{\theta}(u_t|s_t)]=\sum_{t=0}^H \nabla_{\theta} \log \pi_{\theta}(u_t|s_t)$

即有：

$$
\nabla_{\theta} U(\theta)=\frac{1}{m}\sum_{i=0}^m(\sum_{t=0}^H \nabla_{\theta} \log \pi_{\theta}(u_t^i|s_t^i)R(\tau^i))
$$

为了降低方差，引入一个常数 b 降低方差，有：

$$
\nabla_{\theta} U(\theta)=\frac{1}{m}\sum_{i=0}^m(\sum_{t=0}^H \nabla_{\theta} \log \pi_{\theta}(u_t^i|s_t^i)(R(\tau^i)-b))
$$

为了使方差最小，令 $X=\nabla_{\theta}\log P(\tau;\theta)(R(\tau)-b)$ ,有：


$Var(X)=E(X-\overline X)^2=EX^2-E{\overline X}^2$


$\frac{\partial Var(X)}{\partial b}=E(X\frac{\partial X}{\partial b})=0$

$$
b=\frac{\sum_{i=1}^m[(\sum_{t=0}^H \nabla_{\theta}\log \pi_{\theta}(u_t^i|s_t^i))^2R(\tau)]}{\sum_{i=1}^m[(\sum_{t=0}^H \nabla_{\theta}\log \pi_{\theta}(u_t^i|s_t^i))^2]}
$$

存在问题：确定性策略无法探索环境，那么如何学习呢？

答案就是利用异策略学习方法，即off-policy.
异策略是指行动策略和评估策略不是一个策略。这里我们的行动策略是随机策略，以保证充足的探索。评估策略是确定性策略，即公式（8.2）。整个确定性策略的学习框架采用AC的方法。

[1]: https://zhuanlan.zhihu.com/p/62363784#%E7%9B%B4%E6%8E%A5%E7%AD%96%E7%95%A5%E6%90%9C%E7%B4%A2%E6%96%B9%E6%B3%95
[2]: https://www.zhihu.com/column/p/26441204

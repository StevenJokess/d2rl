

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-09 11:50:02
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-09 14:20:21
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# off-policy 梯度下降

我们使用重要性采样将 on-policy 调整为 off-policy

$$
\nabla \bar{R}_\theta=E_{\tau \sim p_{\theta^{\prime}}(\tau)}\left[\frac{p_\theta(\tau)}{p_{\theta^{\prime}}(\tau)} R(\tau) \nabla \log p_\theta(\tau)\right]
$$

- 从 $\theta^{\prime}$ 采样得到数据集
- 使用该 数据集多次训练 $\theta$

梯度更新过程：

$$
\begin{aligned}
& =E_{\left(s_t, a_t\right) \sim \pi_\theta}\left[A^\theta\left(s_t, a_t\right) \nabla \log p_\theta\left(a_t^n \mid s_t^n\right)\right] \\
& =E_{\left(s_t, a_t\right) \sim \pi_{\theta^{\prime}}}\left[\frac{p_\theta\left(s_t, a_t\right)}{p_{\theta^{\prime}}\left(s_t, a_t\right)} A^{\theta^{\prime}}\left(s_t, a_t\right) \nabla \log p_\theta\left(a_t^n \mid s_t^n\right)\right] \\
& =E_{\left(s_t, a_t\right) \sim \pi_{\theta^{\prime}}}\left[\frac{p_\theta\left(a_t \mid s_t\right)}{p_{\theta^{\prime}}\left(a_t \mid s_t\right)} \frac{p_\theta\left(s_t\right)}{p_{\theta^{\prime}}\left(s_t\right)} A^{\theta^{\prime}}\left(s_t, a_t\right) \nabla \log p_\theta\left(a_t^n \mid s_t^n\right)\right] \\
& =E_{\left(s_t, a_t\right) \sim \pi_{\theta^{\prime}}}\left[\frac{p_\theta\left(a_t \mid s_t\right)}{p_{\theta^{\prime}}\left(a_t \mid s_t\right)} A^{\theta^{\prime}}\left(s_t, a_t\right) \nabla \log p_\theta\left(a_t^n \mid s_t^n\right)\right]
\end{aligned}
$$

- 其中 $A^\theta\left(s_t, a_t\right)$ 指的是 advantage 函数，其计算方式为加上衰减机制后的奖励值并减去基线。
- 由于 $\frac{p_\theta\left(s_t\right)}{p_{\theta^{\prime}}\left(s_t\right)}$ 的值难以计算，将其设置为 1 ，简化计算

## 目标函数

由于 $\nabla f(x)=f(x) \nabla \log f(x)$ 再结合不定积分，目标函数可以表示为：

$$
J^{\theta^{\prime}}(\theta)=E_{\left(s_t, a_t\right) \sim \pi_{\theta^{\prime}}}\left[\frac{p_\theta\left(a_t \mid s_t\right)}{p_{\theta^{\prime}}\left(a_t \mid s_t\right)} A^{\theta^{\prime}}\left(s_t, a_t\right)\right]
$$

[1]: https://zhuanlan.zhihu.com/p/614049308

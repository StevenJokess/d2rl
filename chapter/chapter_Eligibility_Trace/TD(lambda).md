

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-19 00:44:35
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-11-10 09:55:52
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# TD(lambda)

我们在计算TD目标时只沿着动作链进一步追踪一步。人们可以很容易地扩展它，采取多个步骤来估计回报。


我们可以随意选择TD学习中的任何 $n$ 。现在的问题是什么才是最好的\#1? 哪个 $G_t^{(n)}$ 给出了最好的返 回近似值? 一个常见但聪明的解决方案是应用所有可能的n步TD目标的加权和，而不是选择单个最佳 $\mathrm{n}$ 。权重衰减因子 $\lambda$ ，其中 $n ， \lambda^{n-1}$; 直觉类似于我们为什么要在计算回报时对未来奖励进行折扣：我们 越是憧憬未来，我们就越没有信心。为了使所有权重 $(n \rightarrow \infty)$ 总和为 1 ，我们将每个权重乘以 (1$\lambda)$ ，因为:
$$
\text { let } \begin{aligned}
S & =1+\lambda+\lambda^2+\ldots \\
S & =1+\lambda\left(1+\lambda+\lambda^2+\ldots\right) \\
S & =1+\lambda S \\
S & =1 /(1-\lambda)
\end{aligned}
$$

多个n步返回的加权和称为 $\lambda$-返回 $G_t^\lambda=(1-\lambda) \sum_{n=1}^{\infty} \lambda^{n-1} G_t^{(n)}$ 。采用入-返回值进行值更新的TD学 习被标记为TD $(\lambda)$ 。

我们之前介绍的原始版本以及SARSA、Q-Learning，都相当于TD(lambda) lambda=0 。[1]

而MC时往后看inf步，故与TD（lambda）lambda=1 时相同。[2]


[1]: https://lilianweng.github.io/posts/2018-02-19-rl-overview/
[2]: https://www.nowcoder.com/questionTerminal/6556b0f010a040e0ae0ddc566ff1e84d

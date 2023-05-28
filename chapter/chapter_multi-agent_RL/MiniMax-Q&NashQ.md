

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-05-28 02:24:40
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-28 21:19:25
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# MiniMax-Q&NashQ

## MiniMax-Q 算法

在完全竞争的随机博弈中(对于两个智能体，即 $R_1＝R_2$，可以应用极小极大化原则)：在最坏情况下假设最大化一个智能体的回报，这个假设是对手将始终努力使其回报最小化。

MiniMax-Q 算法采用极小极大原理来计算阶段游戏的策略和 Q 值, 以及类似于 Q 学习的时序差分规则。下面给出智能体 1 的算法:

$$
\begin{aligned}
& h_{1, t}\left(s_t, \cdot\right)=\arg m_1\left(Q_t, x_t\right) \\
& Q_{t+1}\left(s_t, a_{1, t}, a_{2, t}\right)= Q_t\left(s_t, a_{1, t}, a_{2, t}\right)+\alpha\left[r_{k+1}+\gamma m_1\left(Q_t,\right.\right. \\
&\left.\left.a_{t+1}\right)-Q_t\left(s_t, a_{1, t}, a_{2, t}\right)\right]
\end{aligned}
$$

其中, $m_1$ 是智能体 1 的极小极大化回报。

$$
m_1(Q, s)=\max _{\left.h_{1(s,},\right)} \min _{a_2} \sum_{a_1} h_1\left(x, a_1\right) Q\left(s, a_1, a_2\right)
$$

其中, 在状态 $s$ 时的智能体 1 的随机策略由 $h_{1(s,·}$, 表示, 点代表动作参数。最优化问题可以由线性规划解决。


[1]: https://www.jsjkx.com/CN/article/openArticlePDF.jsp?id=18437


https://www.cs.sjtu.edu.cn/~linghe.kong/%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E8%AE%B2%E4%B9%89%E5%86%AF%E7%BF%94.pdf

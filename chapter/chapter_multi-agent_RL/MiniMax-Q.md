

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-05-28 02:24:40
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-07 14:17:58
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# MiniMax-Q 算法

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

---

根据二人零和 Markov 博弈的定义, 可以得到对于任意策略组合 $\sigma=\left(\sigma_1, \sigma_2\right)$, 按式(9)定义的值函数对于任意的博弈状态 $s \in S$ 都满足 $\mathcal{V}_1\left(s, \sigma_1, \sigma_2\right)=$ $-\mathcal{V}_2\left(s, \sigma_1, \sigma_2\right)^{[27]}$. 奻此, 可以定义二人零和 Markov 博弈中的最优值函数为:

$$
\mathcal{V}^*(s)=\max _{\sigma_1 \in \Sigma_1 \sigma_2 \in \Sigma_2} \min _1 \mathcal{V}_1\left(s, \sigma_1, \sigma_2\right) \#(19)
$$

与经典的 $\mathrm{Q}$ 学习类似, 此时可以得到最优的行动值函数

$$
\begin{aligned}
& Q_1^*\left(s, a_1, a_2\right)= \\
& \mathcal{R}_1\left(s, a_1, a_2\right)+\gamma \sum_{s^{\prime} \in S} \mathcal{Q}\left(s, a_1, a_2, s^{\prime}\right) \mathcal{V}^*\left(s^{\prime}\right) . \text { 然后对 }
\end{aligned}
$$

于任意的状态 $s \in S$, 就可以利用最优行动值函数就能得到个体 1 的最优策略

$$
\sigma_1^*(s)=\underset{\sigma_1\left(s, j \in \Delta\left(A_1\right)\right.}{\operatorname{argmax}} \min _{a_2 \in A_2} \sum_{a_1 \in A_1} \sigma_1\left(s, a_1\right) Q_1^*\left(s, a_1, a_2\right) \#(20)
$$

同理可得个体 2 的最优策略.

基于此, Littman 在 1994 年提出了 minimax-Q 学习算法。与 $\mathrm{Q}$ 学习类似, $\operatorname{minimax}-\mathrm{Q}$ 在计算时 不需要知道环境动力学模型, 是一种无模型方法。当博弈双方在状态 $s$ 执行行动 $\left(a_i, a_{-i}\right)$ 且状态转移 到 $s^{\prime}$ 时, minimax-Q 按照如下方式进行 $\mathrm{Q}$ 值的更新

$$
\begin{gathered}
Q_{i, t+1}\left(s, a_i, a_{-i}\right)=Q_{i, t}\left(s, a_i, a_{-i}\right)+ \\
\alpha_t\left(\mathcal{R}_i\left(s, a_i, a_{-i}\right)+\gamma \mathcal{V}^{\prime}{ }_t\left(s^{\prime}\right)-Q_{i, t}\left(s, a_i, a_{-i}\right)\right) \#(21)
\end{gathered}
$$

其中值函数

$$
\mathcal{V}^{\prime}{ }_t(s)=\max _{\sigma_i(s,) \in \Delta\left(A_i\right) a_{-i} \in A_{-i}} \min _{a_i \in A_i} \sum_i\left(s, a_i\right) Q_{i, t}\left(s, a_i, a_{-i}\right) \#(22)
$$

当 $t \rightarrow \infty$ 且每个状态行动组合对 $\left(s, a_i, a_{-i}\right)$ 被访问 足够多 (趋于无穷) 时, 式(21)定义的 minimax-Q 学习算法会使每个个体 $i$ 的 $Q$ 函数收敛到㖩优行动 价值函数 $Q_i^*$, 此时就可以根据式(20)得到个体的最优策略。

[1]: https://www.jsjkx.com/CN/article/openArticlePDF.jsp?id=18437


https://www.cs.sjtu.edu.cn/~linghe.kong/%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E8%AE%B2%E4%B9%89%E5%86%AF%E7%BF%94.pdf

http://cjc.ict.ac.cn/online/onlinepaper/zl-202297212302.pdf

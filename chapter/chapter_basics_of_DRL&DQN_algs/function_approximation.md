

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-20 15:24:36
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-06 21:25:57
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# 函数近似原理

利用深度网络作为强化学习的Q 函数、策略函数的函数逼近器，使得强化学习算法能够推广到高维状态和动作空间[2]

本节介绍用函数近似 ( function approximation) 方法来估计给定策略 $\pi$ 的状态价值函数 $v_\pi$ 或动作价值函数 $q_\pi$ 。要评估状态价值, 我们可以用一个参数为 $\mathbf{w}$ 的函数 $v(s ; \mathbf{w})$ $(s \in \mathcal{S})$ 来近似状态价值; 要评估动作价值, 我们可以用一个参数为 $\mathbf{w}$ 的函数 $q(s, a ; \mathbf{w})$ $(s \in \mathcal{S}, a \in \mathcal{A}(s))$ 来近似动作价值。在动作集 $\mathcal{A}$ 有限的情况下, 还可以用一个矢量函数 $\mathbf{q}(s ; \mathbf{w})=(q(s, a ; \mathbf{w}): a \in \mathcal{A}) \quad(s \in \mathcal{S})$ 来近似动作价值。矢量函数 $\mathbf{q}(s ; \mathbf{w})$ 的每一个元素对应 着一个动作, 而整个矢量函数除参数外只用状态作为输人。这里的函数 $v(s ; \mathbf{w})(s \in \mathcal{S})$ 、 $q(s, a ; \mathbf{w})(s \in \mathcal{S}, a \in \mathcal{A}(s)) 、 \mathbf{q}(s ; \mathbf{w})(s \in \mathcal{S})$ 形式不限, 可以是线性函数, 也可以是神经 网络。但是, 它们的形式需要事先给定, 在学习过程中只更新参数 $\mathbf{w}$ 。一旦参数 $\mathbf{w}$ 完全确 定, 价值估计就完全给定。所以, 本节将介绍如何更新参数 $\mathbf{w}$ 。更新参数的方法既可以用于策略价值评估, 也可以用于最优策略求解。



## 随机梯度下降

本节来看同策回合更新价值估计。将同策回合更新价值估计与函数近似方法相结合,[1]

得一提的是，对于异策的 学习，即使采用了线性近似，仍然不能保证收敛研究人员发现，只要异策自益、函数近似这三者同时出现，就不能保证收敛 一个著名的例子是 Baird 反例 (Baird' s counterexample) ，有兴趣的读者可以自行查阅


## 收敛的条件

表6-1和表6-2分别总结了策略评估算法和最优策略求解算法的收敛性。在这两个表 格中，表格法指的是第4 5章介绍的不采用函数近似的方法。一般情况下，它们都 能收敛到真实的价值或最优价值。但是，对于函数近似算法，收敛性往往只在采用 梯度下降的回合更新时有保证，而在采用半梯度下降的时序差分方法是没有保证的。

表6-1 策略评估算法的收敛性

| 算法 | 表格法 | 线性近似 | 非线性近似 |
| :---: | :---: | :---: | :---: |
| 回合更新 | 收敛 | 收敛或在最优解附近摆动 | 不一定收敛 |
| SARSA | 收敛 | 收敛或在最优解附近摆动 | 不一定收敛 |
| Q 学习 | 收敛 | 不一定收敛 | 不一定收敛 |

线性近似具有简单的线性叠加结构，使得线性近似可以获得额外的收敛性。当然，所有收玫性都是在学习率满足Robbins-Monro序列的情况下才具有的。对 于能保证收敛的情况，收敛性一般都可以通过验证随机近似Robbins-Monro算法 的条件证明。


## Baird反例

值得一提的是，对于异策的Q学习，即使采用了线性近似，仍然不能保证收敛。研 究人员发现，只要异策、自益、函数近似这三者同时出现，就不能保证收敛。一个 著名的例子是Baird反例(Baird's counterexample)。

Baird反例——考虑如下Markov决策过程: 如图 6-1所示，状态空间为 $S=$ $\left\{s^{(0)}, s^{(1)}, \ldots, s^{(5)}\right\}$ ，动作空间为 $A=\left\{a^{(0)}, a^{(1)}\right\}$ 。一开始等概率处于状态空间中的任 意一个状态。对于时刻 $t(t=0 ， 1 ， \ldots)$ ，无论它处于哪个状态，如果采用动作 $a(0)$ ， 则下一状态为 $s(0)$ ，获得奖励 0 ；如果采用动作 $a^{(1)}$ ，则下一状态等概率从 $S \backslash\{(0)\}$ 中选择，获得奖励为 0 。折扣因子 $\gamma$ 是一个非常接近 1 的数（如0.99）。显然在这个 Markov决策过程中，所有策略的状态价值和动作价值都是 0 ，最优状态价值和最优动作价值也都是 0 ，所有策略都是最优策略。


![](../../img/线性近似具有简单的线性叠加结构，使得线性近似可以获得额外的收敛性。
当然，所有收玫性都是在学习率满足Robbins-Monro序列的情况下才具有的。对 于能保证收敛的情况，收敛性一般都可以通过验证随机近似Robbins-Monro算法 的条件证明。
6.3.2 Baird反例
值得一提的是，对于异策的Q学习，即使采用了线性近似，仍然不能保证收敛。研 究人员发现，只要异策、自益、函数近似这三者同时出现，就不能保证收敛。一个 著名的例子是Baird反例(Baird's counterexample)。

Baird反 例考虑如下 Markov决 策过程: 如图 6-1所示，状态空间为 $S=$ $\left\{s^{(0)}, s^{(1)}, \ldots, s^{(5)}\right\}$ ，动作空间为 $A=\left\{a^{(0)}, a^{(1)}\right\}$ 。一开始等概率处于状态空间中的任 意一个状态。对于时刻 $t(t=0 ， 1 ， \ldots)$ ，无论它处于哪个状态，如果采用动作 $a(0)$ ， 则下一状态为 $s(0)$ ，获得奖励 0 ；如果采用动作 $a^{(1)}$ ，则下一状态等概率从 $S \backslash\{(0)\}$ 中选择，获得奖励为 0 。折扣因子 $\gamma$ 是一个非常接近 1 的数（如0.99）。显然在这个 Markov决策过程中，所有策略的状态价值和动作价值都是 0 ，最优状态价值和最优 动作价值也都是 0 ，所有策略都是最优策略。)



为了证明同时满足异策、自益、函数近似的算法可能不收敛，我们来设计一个策略 并试图评估这个策略。我们将证明这个策略评估算法是发散的。 态总是 $s(0)$ ，其状态价值为 $v_\pi(s)=0(s \in S)$ 。
口函数近似：设计状态价值估计为线性形式 $v(s(l) ; w)=\left(g^{(\lambda)}\right)^{\top} w(i=0$ ， $1 ， \ldots,|S|-1)$ ，其中 $\boldsymbol{w}=(w(0) ， \ldots, w(|S|))^{\top}$ 是待学习的参数， $\boldsymbol{g}^{(1)} $为

$$
\left(\begin{array}{c}
\left(\boldsymbol{g}^{(0)}\right)^{\mathrm{T}} \\
\left(\boldsymbol{g}^{(1)}\right)^{\mathrm{T}} \\
\left(\boldsymbol{g}^{(2)}\right)^{\mathrm{T}} \\
\vdots \\
\left(\boldsymbol{g}^{(|\mathcal{S}|-1)}\right)^{\mathrm{T}}
\end{array}\right)=\left(\begin{array}{ccccc}
2 & 0 & 0 & \cdots & 1 \\
1 & 2 & 0 & \cdots & 0 \\
1 & 0 & 2 & \cdots & 0 \\
\vdots & \vdots & \vdots & & \vdots \\
1 & 0 & 0 & \cdots & 0
\end{array}\right) 。
$$
显然近似函数对参数 $\boldsymbol{w}$ 的梯度为 $\left.\nabla v\left(s^{(}\right) ； \boldsymbol{w}\right)=\boldsymbol{g}^{(I)}(i=0,1, \ldots,|S|-1)$ 。


如果 $S_t=s^{(I)}(i>0) ， A_t=a^{(0)}$ ，这时有

$$
\begin{aligned}
\delta_t & =\gamma\left(\boldsymbol{g}^{(0)}\right)^{\mathrm{T}} \boldsymbol{w}_t-\left(\boldsymbol{g}^{(i)}\right)^{\mathrm{T}} \boldsymbol{w}_t \\
& =\gamma\left(2 w_t^{(0)}+w_t^{(6)}\right)-\left(w_t^{(0)}+2 w_t^{(i)}\right) \\
& =\gamma(2 \cdot 5 \chi+10(\gamma-1) \chi)-(5 \chi+2 \cdot 2 \chi) \\
& =\left(10 \gamma^2-9\right) \chi \\
& \approx \chi_0
\end{aligned}
$$

考虑到 $\boldsymbol{g}^{(1)}$ 的表达式，参数 $w^{(0)}$ 和 $w^{(1)}$ 分别增加 $\alpha \rho t$ 倍的 $\chi$ 和 $2 \chi$ 。

考虑到对于状态集上的状态是等概率分布的，所以各参数分量增加的幅度大致为

$$
\begin{array}{ll}
w^{(0)}: & 20(\gamma-1) \chi+5 \cdot \chi \approx 5 \chi \\
w^{(i)}(i=1, \cdots, 5): & 2 \chi \\
w^{(6)}: & 10(\gamma-1) \chi 。
\end{array}
$$

这些参数分量增加的比例正是 $5: 2: . . .: 2: 10(\gamma-1)$ 。这样就完成了粗略验证。




[1]: E:/BaiduNetdiskDownload/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8B.pdf
[2]: https://cardwing.github.io/files/131270027-%E4%BE%AF%E8%B7%83%E5%8D%97-%E9%99%88%E6%98%A5%E6%9E%97.pdf
[3]: https://weread.qq.com/web/reader/85532b40813ab82d4g017246k64232b60230642e92efb54c

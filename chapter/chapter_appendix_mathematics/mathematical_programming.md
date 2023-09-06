# 数学规划

数学规划(mathematical programming)，也称数学优化(mathematical optimization)，是数学中的一个分支，它主要研究的目标在给定的区域中寻找可以最小化或最大化某一函数的最优解。数学规划在几乎所有的科学领域都有着不容忽视的应用，所以一直都是一门受到着重关注和研究的学科。[1]

## Minimax

极小化极大 (Minimax)，是一类重要的数学规划问题，指在找出失败的最大可能性中的最小值。

**定义**
$$
\max _{Y \in G} F\left(X^*, Y\right)=\min _{X \in G} \max _{Y \in G} F(X, Y)
$$

其中 $R_n$ 是 $\mathrm{n}$ 维实数空间， $\mathrm{O}$ 是 $R_n$ 的一个闭凸子集， $\mathrm{G}$ 是 $R_m$ 的一个有解闭子集，求解 $X^*$ ，如果 $F(X, Y)$ 是线性的，称上式为极小化极大问题。如果 $O \neq R_m$ ，则上式表示的 是有约束的极小化极大问题。

极小化极大问题，虽然目标函数有时可微，但其极大值函数通常不可微，因而极小化极大问题是不可微优化问题。从模型角度，极小化极大问题可以分为离散极小化极大问题和连续的极小化极大问题。[2]


### 解法

#### 思路1

找出极值集合 $G_r$ ，使得

$$
\min _{Y \in G_r} F\left(X^*, Y\right)=\min _{X \in G} \max _{Y \in G} F(X, Y)
$$

要解出 $X^*$ 使得极大值函数最小，必须先找出使得目标函数极大的极值集合 $G_r$ ，然后在 $G_r$ 中求极大值函数的极小值。

#### 思路2

求极大值函数 $\Phi(X)$ 的最小值

$$
\Phi(X)=\max _{Y \in G} F(X, Y)
$$

要求解极小化极大问题，可以通过求出极大值函数，研究极大值函数的性质求得其最小值点。

#### 思路3

求一个鞍点: $X^*, Y^*$ 使得
$$
F\left(X^*, Y\right) \leq F\left(X^*, Y^*\right) \leq F\left(X, Y^*\right)
$$

[1]: https://zhuanlan.zhihu.com/p/25064890
[2]: https://chengzhaoxi.xyz/5aed26f7.html

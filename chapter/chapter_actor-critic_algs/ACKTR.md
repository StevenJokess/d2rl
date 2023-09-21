

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-17 01:46:48
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-20 16:50:05
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# ACKTR

actor-critic using Kronecker-factored trust region

ACKTR 是以 actor-critic 框架为基础，引入 TRPO 使算法稳定性得到保证，然后加上 Kronecker 因子分解以提升样本的利用效率并使模型的可扩展性得到加强。ACKTR 相比于 TRPO 在数据利用率和训练鲁棒性上都有所提升，因而训练效率更高。PPO 和 TRPO 一样以可信域算法为基础，以策略梯度算法作为目标更新算法，但 PPO 相比于 TRPO，只使用一阶优化算法，并对代理目标函数简单限定约束，实现过程更为简便但表现的性能更优。基于策略的深度强化学习发展历程如表 4 所示。

Y. Wu 在文章《Scalable trust-region method for deep reinforcement learning using Kronecker-factored approximation》中提出了 Kronecker 因子信任域执行者/评论者算法 (Actor Critic using Kronecker-factored Trust Region, ACKTR)。这个算法将 Kronecker 因子近似曲率算法用到了信任域策略优化算法中, 减少了计算量。

Kronecker 因子近似曲率算法 (Kronecker-Factor Approximate Curvature, K-FAC) 是这 样的: 考虑一个 $m$ 层的全连接神经网络 $f: \mathbf{x} \mapsto y$, 设其第 $i$ 层 $(0 \leqslant i<m)$ 的输人为 $\mathbf{x}_i$, 权 重为 $\mathbf{W}_i$, 激活前的输出为 $\mathbf{z}_i$, 则有

$$
\mathbf{z}_i=\mathbf{W}_i \mathbf{x}_i, \quad 0 \leqslant i<m
$$

最终的输出 $y$ 对网络中所有权重

$$
\mathbf{w}=\left(\begin{array}{c}
\operatorname{vec}\left(\mathbf{W}_0\right) \\
\operatorname{vec}\left(\mathbf{W}_1\right) \\
\vdots \\
\operatorname{vec}\left(\mathbf{W}_{m-1}\right)
\end{array}\right)
$$

的梯度可以表示为

$$
\nabla_{\mathbf{w}} y=\left(\begin{array}{c}
\operatorname{vec}\left(\nabla_{\mathbf{w}_0} y\right) \\
\operatorname{vec}\left(\nabla_{\mathbf{w}_1} y\right) \\
\vdots \\
\operatorname{vec}\left(\nabla_{\mathbf{w}_{m-1}} y\right)
\end{array}\right)
$$

考虑任意的 $0 \leqslant i<m$, 由于 $\mathbf{z}_i=\mathbf{W}_i \mathbf{x}_i$, 用链式法则可得 $\nabla_{\mathbf{w}_i} y=\mathbf{g}_i \mathbf{x}_i^{\mathrm{T}}$, 其中 $\mathbf{g}_i=\nabla_{\mathbf{z}_i} y_{\circ}$ 。进而得出

$$
\operatorname{vec}\left(\nabla_{\mathbf{w}_i} y\right)=\operatorname{vec}\left(\mathbf{g}_i \mathbf{x}_i^{\mathrm{T}}\right)=\mathbf{x}_i \otimes \mathbf{g}_i
$$

其中 $\otimes$ 是 Kronecker 积, 这里用了恒等式 $\operatorname{vec}\left(\mathbf{u v}^{\mathrm{T}}\right)=\mathbf{v} \otimes \mathbf{u}$ 。再进一步, 对于任意的 $0 \leqslant i, j<m$ 有

$$
\operatorname{vec}\left(\nabla_{\mathbf{w}_i} y\right)\left[\operatorname{vec}\left(\nabla_{\mathbf{w}_j} y\right)\right]=\left[\mathbf{x}_i \otimes \mathbf{g}_i\right]\left[\mathbf{x}_j \otimes \mathbf{g}_j\right]^{\mathrm{T}}=\left(\mathbf{x}_i \mathbf{x}_j^{\mathrm{T}}\right) \otimes\left(\mathbf{g}_i \mathbf{g}_j^{\mathrm{T}}\right)
$$

这里用了恒等式 $(\mathbf{A} \otimes \mathbf{B})(\mathbf{C} \otimes \mathbf{D})=(\mathbf{A C}) \otimes(\mathbf{B D})$ 。这样, 我们就得到了 $\left(\nabla_{\mathbf{w}} y\right)\left(\nabla_{\mathbf{w}} y\right)^{\mathrm{T}}$ 的表达式, 其第 $(i, j)$ 元素为

$$
\left[\left(\nabla_{\mathbf{w}} y\right)\left(\nabla_{\mathbf{w}} y\right)^{\mathrm{T}}\right]_{i, j}=\left(\mathbf{x}_i \mathbf{x}_j^{\mathrm{T}}\right) \otimes\left(\mathbf{g}_i \mathbf{g}_j^{\mathrm{T}}\right), \quad 0 \leqslant i, j<m
$$

当神经网络的输人是随机变量 $\mathbf{X}$ 时, 有

$$
\mathrm{E}\left[\left(\nabla_{\mathbf{w}} Y\right)\left(\nabla_{\mathrm{w}} Y\right)^{\mathrm{T}}\right]_{i, j}=\mathrm{E}\left[\left(\mathbf{X}_i \mathbf{X}_j^{\mathrm{T}}\right) \otimes\left(\mathbf{G}_i \mathbf{G}_j^{\mathrm{T}}\right)\right] \approx \mathrm{E}\left[\mathbf{X}_i \mathbf{X}_j^{\mathrm{T}}\right] \otimes \mathrm{E}\left[\mathbf{G}_i \mathbf{G}_j^{\mathrm{T}}\right], \quad 0 \leqslant i, j<m
$$

这就是 Kronecker 因子近似曲率算法的表达式。除了用这样的近似以外, 还可以进一步将某 些 $\mathrm{E}\left[\left(\nabla_{\mathrm{w}} Y\right)\left(\nabla_{\mathrm{w}} Y\right)^{\mathrm{T}}\right]_{i, j}$ 近似为全零矩阵, 得到分块对角矩阵或分块三角矩阵。

Kronecker 因子近似曲率算法可以直接用在 Fisher 信息矩阵的计算上。回顾前文, Fisher 信息矩阵具有以下形式:
$$
\mathbf{F}=\mathrm{E}\left[\left[\nabla \ln \pi\left(A_t \mid S_t ; \boldsymbol{\theta}\right)\right]\left[\nabla \ln \pi\left(A_t \mid S_t ; \boldsymbol{\theta}\right)\right]^{\mathrm{T}}\right]
$$
这正是 Kronecker 因子算法可以处理的形式。这个算法将 Kronecker 因子近似曲率算法用到了信任域策略优化算法中, 减少了计算量。[2]

[1]: https://www.eefocus.com/article/402315.html
[2]: E:/BaiduNetdiskDownload/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8B.pdf

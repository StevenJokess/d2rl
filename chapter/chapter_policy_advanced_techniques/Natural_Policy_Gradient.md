# 自然策略梯度算法

自然策略梯度算法（Natural Policy Gradient, NPG）是一个基于代理优势的迭代算法, 它由 J. Schulman 等在文章《Trust region policy optimization》中提出。它的原理是通过 最大化代理优势并限定新策略处于信任域内来更新策略参数。这事实上就在考虑以下优化问题：

$$
\begin{array}{ll}
\operatorname{minimize} & \mathrm{E}_{\pi\left(\boldsymbol{\theta}_k\right)}\left[\frac{\pi(A \mid S ; \boldsymbol{\theta})}{\pi\left(A \mid S ; \boldsymbol{\theta}_k\right)} a_{\pi\left(\boldsymbol{\theta}_k\right)}(S, A)\right] \\
\text { over } & \boldsymbol{\theta} \\
\text { s.t. } & \mathrm{E}_{S \sim \pi\left(\boldsymbol{\theta}_k\right)}\left[d_{\mathrm{KL}}\left(\pi(\cdot \mid S ; \boldsymbol{\theta}) \| \pi\left(\cdot \mid S ; \boldsymbol{\theta}_k\right)\right)\right] \leqslant \delta
\end{array}
$$

其中 $\delta$ 是一个可以设置的参数。这个优化问题的目标函数和约束都很复杂, 需要进一步进行简化。
如果把上述优化问题中的目标取 $\boldsymbol{\theta}=\boldsymbol{\theta}_k$ 处的 Tayler 展开取前两项近似（注意由于代理优 势的梯度和优化的最终目标 $\mathrm{E}_{\pi(\boldsymbol{\theta})}\left[G_0\right]$ 的梯度在 $\boldsymbol{\theta}=\boldsymbol{\theta}_k$ 处是相同的, 所以这个近似可以认为是 直接对最终目标展开):

$$
\mathrm{E}_{\pi\left(\boldsymbol{\theta}_k\right)}\left[\frac{\pi(A \mid S ; \boldsymbol{\theta})}{\pi\left(A \mid S ; \boldsymbol{\theta}_k\right)} a_{\pi\left(\boldsymbol{\theta}_k\right)}(S, A)\right] \approx \mathbf{0}+\mathbf{g}\left(\boldsymbol{\theta}_k\right) \cdot\left(\boldsymbol{\theta}-\boldsymbol{\theta}_k\right)
$$

约束取二次型近似：

$$
\bar{d}_{\mathrm{KL}}\left(\boldsymbol{\theta} \| \boldsymbol{\theta}_k\right) \approx 0+\boldsymbol{0} \cdot\left(\boldsymbol{\theta}-\boldsymbol{\theta}_k\right)+\frac{1}{2}\left(\boldsymbol{\theta}-\boldsymbol{\theta}_k\right)^{\mathrm{T}} \mathbf{F}\left(\boldsymbol{\theta}_k\right)\left(\boldsymbol{\theta}-\boldsymbol{\theta}_k\right)
$$

可以得到一个简化的优化问题：

$$
\begin{array}{ll}
\operatorname{maximize} & \mathbf{g}\left(\boldsymbol{\theta}_k\right) \cdot\left(\boldsymbol{\theta}-\boldsymbol{\theta}_k\right) \\
\text { over } & \boldsymbol{\theta} \\
\text { s.t. } & \frac{1}{2}\left(\boldsymbol{\theta}-\boldsymbol{\theta}_k\right)^{\mathrm{T}} \mathbf{F}\left(\boldsymbol{\theta}_k\right)\left(\boldsymbol{\theta}-\boldsymbol{\theta}_k\right) \leqslant \delta
\end{array}
$$

而这个简化的优化问题具有闭式解：

$$
\boldsymbol{\theta}_{k+1}=\boldsymbol{\theta}_k+\sqrt{\frac{2 \delta}{\left(\mathbf{g}\left(\boldsymbol{\theta}_k\right)\right)^{\mathrm{T}} \mathbf{F}^{-1}\left(\boldsymbol{\theta}_k\right) \mathbf{g}\left(\boldsymbol{\theta}_k\right)}} \mathbf{F}^{-1}\left(\boldsymbol{\theta}_k\right) \mathbf{g}\left(\boldsymbol{\theta}_k\right)
$$




这里的 $\sqrt{\frac{2 \delta}{\left(\mathbf{g}\left(\boldsymbol{\theta}_k\right)\right)^{\mathrm{T}} \mathbf{F}^{-1}\left(\boldsymbol{\theta}_k\right) \mathbf{g}\left(\boldsymbol{\theta}_k\right)}} \mathbf{F}^{-1}\left(\boldsymbol{\theta}_k\right) \mathbf{g}\left(\boldsymbol{\theta}_k\right)$ 就称为自然梯度, 而上式就是自然策略梯度算法的迭代式。

基于上述迭代式，算法 8-6 给出了自然策略梯度算法。

将简单的自然梯度算法和共轭梯度算法结合，可以得到带共轭梯度的自然梯度算法


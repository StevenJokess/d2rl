

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-20 15:04:52
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-20 15:29:40
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 总结

本章介绍了执行者 / 评论者算法在连续动作空间中的确定性版本。连续动作空间中的确 定性版本与普通情况下的执行者 / 评论者算法在处理上略有区别, 特别对于异策的算法更是如此。使用时要注意选用合适的版本。
至此, 已经学完了本书的理论部分。后续章节将展示一些综合实例。

## 本章要点

$>$ 连续动作空间的最优确定性策略可用 $\pi(s ; \boldsymbol{\theta})$ 近似。
$>$ 连续动作空间中确定性策略梯度定理为:
$$
\nabla \mathrm{E}_{\pi(\boldsymbol{\theta})}\left[G_0\right]=\mathrm{E}_{S \sim \rho_{\pi(\theta)}}\left[\nabla \pi(S ; \boldsymbol{\theta})\left[\nabla_a q_{\pi(\theta)}(S, a)\right]_{a=\pi(S ; \theta)}\right],
$$
其中 $\rho_\pi$ 是策略 $\pi$ 的带折扣状态分布。
$>$ 基本的同策和异策确定性执行者/评论者算法在更新策略参数 $\boldsymbol{\theta}$ 时试图增大 $\gamma^t q\left(S_t, \pi\left(S_t ; \boldsymbol{\theta}\right) ; \mathbf{w}\right)$ 。
连续动作空间上的确定性算法可以通过增加扰动来实现探索。扰动可以是独立同分 布的 Gaussian 噪声、Ornstein Uhlenbeck 过程等。
$>$ 引入行为策略和重要性采样, 可以实现确定性异策回合更新梯度算法。深度确定性 策略梯度算法常结合经验回放、双重网络等技术。[1]

[1]: E:/BaiduNetdiskDownload/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8B.pdf


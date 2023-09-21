

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-20 15:24:36
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-20 17:23:34
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# 函数近似原理

本节介绍用函数近似 ( function approximation) 方法来估计给定策略 $\pi$ 的状态价值 函数 $v_\pi$ 或动作价值函数 $q_\pi$ 。要评估状态价值, 我们可以用一个参数为 $\mathbf{w}$ 的函数 $v(s ; \mathbf{w})$ $(s \in \mathcal{S})$ 来近似状态价值; 要评估动作价值, 我们可以用一个参数为 $\mathbf{w}$ 的函数 $q(s, a ; \mathbf{w})$ $(s \in \mathcal{S}, a \in \mathcal{A}(s))$ 来近似动作价值。在动作集 $\mathcal{A}$ 有限的情况下, 还可以用一个矢量函数 $\mathbf{q}(s ; \mathbf{w})=(q(s, a ; \mathbf{w}): a \in \mathcal{A}) \quad(s \in \mathcal{S})$ 来近似动作价值。矢量函数 $\mathbf{q}(s ; \mathbf{w})$ 的每一个元素对应 着一个动作, 而整个矢量函数除参数外只用状态作为输人。这里的函数 $v(s ; \mathbf{w})(s \in \mathcal{S})$ 、 $q(s, a ; \mathbf{w})(s \in \mathcal{S}, a \in \mathcal{A}(s)) 、 \mathbf{q}(s ; \mathbf{w})(s \in \mathcal{S})$ 形式不限, 可以是线性函数, 也可以是神经 网络。但是, 它们的形式需要事先给定, 在学习过程中只更新参数 $\mathbf{w}$ 。一旦参数 $\mathbf{w}$ 完全确 定, 价值估计就完全给定。所以, 本节将介绍如何更新参数 $\mathbf{w}$ 。更新参数的方法既可以用 于策略价值评估, 也可以用于最优策略求解。

## 随机梯度下降

本节来看同策回合更新价值估计。将同策回合更新价值估计与函数近似方法相结合,[1]

得一提的是，对于异策的 学习，即使采用了线性近似，仍然不能保证收敛研究人员发现，只要异策自益、函数近似这三者同时出现，就不能保证收敛 一个著名的例子是 Baird 反例 (Baird' s counterexample) ，有兴趣的读者可以自行查阅



[1]: E:/BaiduNetdiskDownload/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8B.pdf

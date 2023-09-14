

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-11 21:25:49
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-11 21:27:12
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 高斯过程

高斯过程 (Gaussian Process) 也是一种应用广泛的随机过程模型. 假设有 一组连续随机变量 $X_0, X_1, \cdots, X_T$, 如果由这组随机变量构成的任一有限集合

$$
X_{t_1, \cdots, t_N}=\left[X_{t_1}, \cdots, X_{t_N}\right]^{\top}, \quad 1 \leq N \leq T
$$

都服从一个多元正态分布, 那么这组随机变量为一个随机过程. 高斯过程也可以 定义为: 如果 $X_{t_1, \cdots, t_N}$ 的任一线性组合都服从一元正态分布, 那么这组随机变量 为一个随机过程.

## 高斯过程回归

高斯过程回归 (Gaussian Process Regression) 是利用高斯过程 来对一个函数分布进行建模. 和机器学习中参数化建模 (比如贝叶斯线性回归) 相比, 高斯过程是一种非参数模型, 可以拟合一个黑盒函数, 并给出拟合结果的 置信度 [Rasmussen, 2003].

假设一个未知函数 $f(\boldsymbol{x})$ 服从高斯过程, 且为平滑函数. 如果两个样本 $\boldsymbol{x}_1, \boldsymbol{x}_2$ 比较接近, 那么对应的 $f\left(\boldsymbol{x}_1\right), f\left(\boldsymbol{x}_2\right)$ 也比较接近. 假设从函数 $f(\boldsymbol{x})$ 中采样有限个样本 $\boldsymbol{X}=\left[\boldsymbol{x}_1, \boldsymbol{x}_2, \cdots, \boldsymbol{x}_N\right]$, 这 $N$ 个点服从一个多元正态分布,

$$
\left[f\left(\boldsymbol{x}_1\right), f\left(\boldsymbol{x}_2\right), \cdots, f\left(\boldsymbol{x}_N\right)\right]^{\top} \sim \mathcal{N}(\mu(X), \boldsymbol{K}(X, X)),
$$

其中 $\boldsymbol{\mu}(\boldsymbol{X})=\left[\boldsymbol{\mu}\left(\boldsymbol{x}_1\right), \boldsymbol{\mu}\left(\boldsymbol{x}_2\right), \cdots, \boldsymbol{\mu}\left(\boldsymbol{x}_N\right)\right]^{\top}$ 是均值向量, $\boldsymbol{K}(\boldsymbol{X}, \boldsymbol{X})=\left[k\left(\boldsymbol{x}_i, \boldsymbol{x}_j\right)\right]_{N \times N}$ 是协方差矩阵, $k\left(\boldsymbol{x}_i, \boldsymbol{x}_j\right)$ 为核函数, 可以衡量两个样本的相似度.

在高斯过程回归中,一个常用的核函数是**平方指数** ( Squared Exponential) 核函数:

$$
k\left(\boldsymbol{x}_i, \boldsymbol{x}_{\boldsymbol{j}}\right)=\exp \left(\frac{-\left\|\boldsymbol{x}_i-\boldsymbol{x}_{\boldsymbol{j}}\right\|^2}{2 l^2}\right),
$$

其中 $l$ 为超参数. 当 $\boldsymbol{x}_i$ 和 $\boldsymbol{x}_{\boldsymbol{j}}$ 越接近, 其函数值越大, 表明 $f\left(\boldsymbol{x}_i\right)$ 和 $f\left(\boldsymbol{x}_{\boldsymbol{j}}\right)$ 越相关.

假设 $f(\boldsymbol{x})$ 的一组带噪声的观测值为 $\left\{\left(\boldsymbol{x}_n, y_n\right)\right\}_{n=1}^N$, 其中 $y_n \sim \mathcal{N}\left(f\left(\boldsymbol{x}_n\right), \sigma^2\right)$ 为 $f\left(x_n\right)$ 的观测值, 服从正态分布, $\sigma$ 为噪声方差.

对于一个新的样本点 $\boldsymbol{x}^*$, 我们希望预测 $f\left(\boldsymbol{x}^*\right)$ 的观测值 $y^*$. 令向量 $\boldsymbol{y}=$ $\left[y_1, y_2, \cdots, y_N\right]^{\top}$ 为已有的观测值, 根据高斯过程的假设, $\left[\boldsymbol{y} ; y^*\right]$ 满足

$$
\left[\begin{array}{c}
\boldsymbol{y} \\
y^*
\end{array}\right] \sim \mathcal{N}\left(\left[\begin{array}{l}
\mu(\boldsymbol{X}) \\
\mu\left(\boldsymbol{x}^*\right)
\end{array}\right],\left[\begin{array}{cc}
\boldsymbol{K}(\boldsymbol{X}, \boldsymbol{X})+\sigma^2 \boldsymbol{I} & \boldsymbol{K}\left(\boldsymbol{x}^*, \boldsymbol{X}\right)^{\top} \\
\boldsymbol{K}\left(\boldsymbol{x}^*, \boldsymbol{X}\right) & k\left(\boldsymbol{x}^*, \boldsymbol{x}^*\right)
\end{array}\right]\right),
$$

其中 $\boldsymbol{K}\left(\boldsymbol{x}^*, \boldsymbol{X}\right)=\left[k\left(\boldsymbol{x}^*, \boldsymbol{x}_1\right), \cdots, k\left(\boldsymbol{x}^*, \boldsymbol{x}_n\right)\right]$.

根据上面的联合分布, $y^*$ 的后验分布为

$$
p\left(y^* \mid \boldsymbol{X}, \boldsymbol{y}\right)=\mathcal{N}\left(\hat{\mu}, \hat{\sigma}^2\right),
$$

其中均值 $\hat{\mu}$ 和方差 $\hat{\sigma}$ 为

$$
\begin{gathered}
\hat{\mu}=\boldsymbol{K}\left(\boldsymbol{x}^*, \boldsymbol{X}\right)\left(\boldsymbol{K}(\boldsymbol{X}, \boldsymbol{X})+\sigma^2 \boldsymbol{I}\right)^{-1}(\boldsymbol{y}-\boldsymbol{\mu}(\boldsymbol{X}))+\mu\left(\boldsymbol{x}^*\right), \\
\hat{\sigma}^2=k\left(\boldsymbol{x}^*, \boldsymbol{x}^*\right)-\boldsymbol{K}\left(\boldsymbol{x}^*, \boldsymbol{X}\right)\left(\boldsymbol{K}(\boldsymbol{X}, \boldsymbol{X})+\sigma^2 \boldsymbol{I}\right)^{-1} \boldsymbol{K}\left(\boldsymbol{x}^*, \boldsymbol{X}\right)^{\top} .
\end{gathered}
$$

从公式 (D.54) 可以看出, 均值函数 $\boldsymbol{\mu}(\boldsymbol{x})$ 可以近似地互相抵消. 在实际应用 中,一般假设 $\mu(\boldsymbol{x})=0$,均值 $\hat{\mu}$ 可以简化为

$$
\hat{\mu}=K\left(\boldsymbol{x}^*, \boldsymbol{X}\right)\left(K(\boldsymbol{X}, \boldsymbol{X})+\sigma^2 \boldsymbol{I}\right)^{-1} \boldsymbol{y} .
$$

高斯过程回归可以认为是一种有效的贝叶斯优化方法, 广泛地应用于机器学习中.[1]

[1]: https://nndl.github.io/



<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-22 02:09:11
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-02 16:59:53
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 矩阵论

**矩阵**(Matrix)：是一个二维数组，其中的每一个元素一般由两个索引来确定一般用大写变量表示，m行n列的实数矩阵，记做$A \in R_{m \times n}$.

**张量**(Tensor)：是矢量概念的推广，可用来表示在一些矢量、标量和其他张量之间的线性关系的多线性函数。标量是0阶张量，矢量是一阶张量，矩阵是二阶张量，三维及以上数组一般称为张量。

**矩阵的秩**(Rank)：矩阵列向量中的极大线性无关组的数目，记作矩阵的列秩，同样可以定义行秩。行秩=列秩=矩阵的秩，通常记作rank(A)。

## 逆

**矩阵的逆**

- 若矩阵A为方阵，当 $rank(A_{n×n})<n$时，称A为奇异矩阵或不可逆矩阵；
- 若矩阵A为方阵，当 $rank(A_{n×n})=n$时，称A为非奇异矩阵或可逆矩阵

其逆矩阵 $A^{-1}$ 满足以下条件，则称 $A^{-1}$ 为矩阵A的逆矩阵：
$$
AA^{-1} = A^{-1}A = I_n
$$
其中 $I_n$ 是 $n×n$ 的单位阵。

**矩阵的广义逆矩阵**

- 如果矩阵不为方阵或者是奇异矩阵，不存在逆矩阵，但是可以计算其广义逆矩阵或者伪逆矩阵；
- 对于矩阵A，如果存在矩阵 $B$ 使得 $ABA=A$，则称 $B$ 为 $A$ 的广义逆矩阵。


### 矩阵分解

机器学习中常见的矩阵分解有特征分解和奇异值分解。

先提一下矩阵的特征值和特征向量的定义

- 若矩阵 $A$ 为方阵，则存在非零向量 $x$ 和常数 $\lambda$ 满足 $Ax=\lambda x$，则称 $ \lambda$ 为矩阵 $ A$ 的一个特征值，$x$ 为矩阵 $A$ 关于 $\lambda$ 的特征向量。
- $A_{n \times n}$ 的矩阵具有 $n$ 个特征值，$λ_1 ≤ λ_2 ≤ ⋯ ≤ λ_n$ 其对应的*n*个特征向量为 $𝒖_1，𝒖_2， ⋯ ，𝒖_𝑛$
- 矩阵的迹(trace)和行列式(determinant)的值分别为

$$
\operatorname{tr}(\mathrm{A})=\sum_{i=1}^{n} \lambda_{i} \quad|\mathrm{~A}|=\prod_{i=1}^{n} \lambda_{i}
$$

**矩阵特征值分解**：$A_{n \times n}$ 的矩阵具有 $n$ 个不同的特征值，那么矩阵A可以分解为 $A = U\Sigma U^{T}$.

其中 $\Sigma=\left[\begin{array}{cccc}\lambda_{1} & 0 & \cdots & 0 \\ 0 & \lambda_{2} & \cdots & 0 \\ 0 & 0 & \ddots & \vdots \\ 0 & 0 & \cdots & \lambda_{n}\end{array}\right] \quad \mathrm{U}=\left[\boldsymbol{u}_{1}, \boldsymbol{u}_{2}, \cdots, \boldsymbol{u}_{n}\right] \quad \left\|\boldsymbol{u}_{i}\right\|_{2}=1$ .

**奇异值分解**：对于任意矩阵 $A_{m \times n}$，存在正交矩阵 $U_{m \times m}$ 和 $V_{n \times n}$，使其满足 $A = U \Sigma V^{T} \quad U^T U = V^T V = I$，则称上式为矩阵 $A$ 的特征分解。

## 矩阵求导

- 矩阵对标量求导:矩阵的每个元素对标量求导
- 标量对矩阵求导:依次对矩阵的每个元素求偏导数，注意结果矩阵的形状与求导矩阵的形状互为转置(分子布局)
- 矩阵对向量求导:将矩阵将矩阵依次对每个分量求导即可; 若矩阵是 $n * m$ 对L维列向量求导，则得到一个 $n * m * L$ 的张量
- 向量对矩阵求导:每个分量对矩阵进行求导，就转换为标量对矩阵进行求导，一个 L维列向量对一个 $n * m$ 的矩阵 求导得到的是一个 $L * m * n$ 的张量
- 矩阵对矩阵求导:第一个矩阵的每个元素对第二个矩阵进行求导，就转换为标量对矩阵进行求导，一个L维列向量对 一个 $n * m$ 的矩阵求导得到的是一个 $L * m * n$ 的张量
- 矩阵对矩阵求导:第一个矩阵的每个元素对第二个矩阵进行求导


[1]: https://github.com/datawhalechina/unusual-deep-learning/edit/main/docs/02.%E6%95%B0%E5%AD%A6%E5%9F%BA%E7%A1%80.md
[2]: https://zhuanlan.zhihu.com/p/654302325



<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-09 20:32:52
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-11 22:25:03
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# 符号表和中英术语对照
:label:`notation`

## 符号表（Notations）

这里只列出常用字母。部分小节会局部定义的字母，以该局部定义为准。

在本书中，我们遵循以下的符号约定：

请注意，其中一些符号是占位符，而其他符号指代特定对象。作为一般规则，不定冠词 一个（"a"） 常常表示该符号是一个占位符（placeholder），类似格式的符号通常用来表示相同类型的其他对象。例如，"$x$: 一个标量" 意味着小写字母通常表示标量值，但 "$\mathbb{Z}$: 整数集" 具体指的是符号 $\mathbb{Z}$。

### 数值性的对象

* $x$: 一个标量
* $\mathbf{x}$: 一个向量
* $\mathbf{X}$: 一个矩阵
* $\mathsf{X}$: 一个一般的张量
* $\mathbf{I}$: 单位矩阵（某一给定维度），即对角线上的元素为 $1$，非对角线上的元素为 $0$
* $x_i$, $[\mathbf{x}]_i$: 向量 $\mathbf{x}$ 的第 $i^\textrm{th}$ 个元素
* $x_{ij}$, $x_{i,j}$,$[\mathbf{X}]_{ij}$, $[\mathbf{X}]_{i,j}$: 矩阵 $\mathbf{X}$ 中位于第 $i$ 行第 $j$ 列的元素。

### 集合论

* $\mathcal{X}$: 一个集合
* $\mathbb{Z}$: 整数集
* $\mathbb{Z}^+$: 正整数集
* $\mathbb{R}$: 实数集
* $\mathbb{R}^n$: 实数向量的 $n$ 维集合
* $\mathbb{R}^{a\times b}$: 实数矩阵的集合，具有 $a$ 行 $b$ 列
* $|\mathcal{X}|$: 集合 $\mathcal{X}$ 的基数（元素数量）
* $\mathcal{A}\cup\mathcal{B}$: 集合 $\mathcal{A}$ 和 $\mathcal{B}$ 的并集
* $\mathcal{A}\cap\mathcal{B}$: 集合 $\mathcal{A}$ 和 $\mathcal{B}$ 的交集
* $\mathcal{A}\setminus\mathcal{B}$: 集合 $\mathcal{A}$ 减去集合 $\mathcal{B}$（包含仅属于 $\mathcal{A}$ 而不属于 $\mathcal{B}$ 的元素）

## 函数和运算符

* $f(\cdot)$: 一个函数
* $\log(\cdot)$: 自然对数（底数为 $e$）
* $\log_2(\cdot)$: 以 $2$ 为底的对数
* $\exp(\cdot)$: 指数函数
* $\mathbf{1}(\cdot)$: 指示函数；如果布尔参数为真，则结果为 $1$，否则为 $0$
* $\mathbf{1}_{\mathcal{X}}(z)$: 集合成员指示函数；如果元素 $z$ 属于集合 $\mathcal{X}$，则结果为 $1$，否则为 $0$
* $\mathbf{(\cdot)}^\top$: 向量或矩阵的转置
* $\mathbf{X}^{-1}$: 矩阵 $\mathbf{X}$ 的逆
* $\odot$: 哈达玛积（逐元素）乘积
* $[\cdot, \cdot]$: 连接
* $\|\cdot\|_p$: $\ell_p$ 范数
* $\|\cdot\|$: $\ell_2$ 范数
* $\langle \mathbf{x}, \mathbf{y} \rangle$: 向量 $\mathbf{x}$ 和 $\mathbf{y}$ 的内积（点积）
* $\sum$: 对一组元素求和
* $\prod$: 对一组元素求积
* $\stackrel{\textrm{def}}{=}$: 将等式作为左侧符号的定义断言

### 微积分

* $\frac{dy}{dx}$: 关于 $x$ 对 $y$ 的导数
* $\frac{\partial y}{\partial x}$: 关于 $x$ 的 $y$ 的偏导数
* $\nabla_{\mathbf{x}} y$: 关于 $\mathbf{x}$ 对 $y$ 的梯度
* $\int_a^b f(x) \;dx$: 关于 $x$ 从 $a$ 到 $b$ 的定积分
* $\int f(x) \;dx$: 关于 $x$ 的不定积分

### 概率与信息论

* $X$: 一个随机变量
* $P$: 一个概率分布
* $X \sim P$: 随机变量 $X$ 服从分布 $P$
* $P(X=x)$: 随机变量 $X$ 取值为 $x$ 的概率
* $P(X \mid Y)$: 给定 $Y$ 的条件概率分布 $X$
* $p(\cdot)$: 与分布 $P$ 关联的概率密度函数（PDF）
* ${E}[X]$: 随机变量 $X$ 的期望值
* $X \perp Y$: 随机变量 $X$ 和 $Y$ 独立
* $X \perp Y \mid Z$: 随机变量 $X$ 和 $Y$ 在给定 $Z$ 的条件下独立
* $\sigma_X$: 随机变量 $X$ 的标准差
* $\textrm{Var}(X)$: 随机变量 $X$ 的方差，等于 $\sigma^2_X$
* $\textrm{Cov}(X, Y)$: 随机变量 $X$ 和 $Y$ 的协方差
* $\rho(X, Y)$: 随机变量 $X$ 和 $Y$ 的皮尔逊相关系数

### 强化学习

一般规律： 大写是随机事件或随机变量，小写是确定性事件或确定性数值。衬线体（如Times New Roman字体）是数值，非衬线体（如Open Sans字体）则不一定是数值。粗体是向量或矩阵。花体是集合。[2]

| 符号（Notations）                 | 意义表示（Meaning） |
| :---                     | :--- |
| $\mathcal {S}$ 或 $\Omega$           | 状态空间（State space）：所有合法状态的集合 |
| $\mathcal {A}$           | 动作空间（Action space）：所有合法动作的集合 |
| $\mathcal {P}$           | 转移函数（Transition function） |
| $\mathcal {R}$           | 回报函数（Reward function） |
| $\mathcal {O}$           | 观测空间（Observation space）：所有合法观测的集合 |
| $\mathcal {\rho_(o)}$    | 初始状态分布（Initial state distribution） |
| $\mathcal {\gamma}$      | 折扣率（Discount factor） |
| $\mathcal {T}$           | 终止状态的集合（Set of terminal states） |
| $\mathcal {V}$           | 状态价值函数（State-value function） |
| $\mathcal {Q}$           | 动作价值函数（Action-value function） |
| $\mathcal {J}$           | 期望总回报函数（Expected total reward function） |
| $\mathcal {L}$           | 损失函数（Loss function） |
| $\mathcal {f}$           | 目标函数（Objective function） |
| $\mathcal{\theta}$       | 神经网络参数（Neural network parameters） |
| $\mathcal{\phi}$         | 另一个神经网络参数Secondary neural network parameters） |
| $\mathcal {B}$           | 批大小（Batch size） |
| $\lambda$                | 自举超参数（Bootstrapping hyperparameter） |
| $\zeta$                  | 总共的超参数（All hyperparameters） |
| $\eta$                   | 一个可微分超参数的子集（Subset of hyperparameters that are differentiable） |

## 术语定义（Glossary）和中英术语对照（Terminology）

TODO:

自举（Bootstrapping）：“拔自己的鞋带，把自己举起来”（To lift oneself up by his bootstraps.）在强化学习里，用一个估算去更新同类的估算（In RL,bootstrapping means "using an estimated value in theupdate step for the same kind of estimated value".）[3]




[1]: https://zhiqingxiao.github.io/rl-book/zh2019/notation/zh2019notation.html
[2]: https://zhuanlan.zhihu.com/p/510965690
[3]: https://www.youtube.com/watch?v=X2-56QN79zc
[4]: https://raw.githubusercontent.com/d2l-ai/d2l-en/master/chapter_notation/index.md

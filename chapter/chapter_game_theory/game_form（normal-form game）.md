

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-05-28 23:01:36
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-07 14:12:36
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# 博弈形式

在博弈论中，经典的博弈模型有标准形式博弈（normal-form game）、扩展形式博弈（extensive-form  game）、Markov 博弈（Markov game）等。

之后会对它们的定义、解概念进行一一介绍。本节主要专注在标准形式博弈（normal-form game）。

## 标准形式博弈

标准形式博弈，又称为策略形式博弈（strategic-form game），其定义如下：

### 定义 1. 标准形式博弈

标准形式博弈 $G$ 由 $\left\langle N,\left(A_i, \mathcal{R}_i\right)_{i \in N}\right\rangle$ 表示,

其中：

- $N=\{1,2, \ldots, n\}$ 表示参与博弈所有个体的集合;
- 对于任意个体 $i \in N, A_i$ 是其动作集合; $A=\times_{i=1}^n A_i$ 表示所有个体的联合动作空间;
- $\mathcal{R}_i: A \rightarrow \mathbb{R}$ 表示个体 $i$ 的收益函数,
- $\mathcal{R}_i(\mathbf{a})$ 表示在联合动作 $\mathbf{a}=\left(a_1, a_2, \ldots, a_n\right)$ 下, 个体 $i$ 的收益。

标准形式博弈可以看作一个 $n$ 维矩阵, 矩阵中的元素是个体在不同的联合动作下收益的向量, 也就是 $\left(\mathcal{R}_1, \ldots, \mathcal{R}_n\right)$, 因此标准形式博弈也称为矩阵博弈（matrix game）。

标准形式博弈是博弈论中最基本的博弈模型, 它最直接地描述了个体的行动与收益之间的关系，并且几乎所有其他的博弈模型都可以转化为标准形式。

- 此外, 在标准形式博弈中, 所有个体同时决策（或者某一个个体在做决策时不知道其他个体当前的决策结果）且只决策一次。
- 特别地, 当 $n=2$ 且对于任意的 $\mathbf{a} \in A$ 都有 $\mathcal{R}_1(\mathbf{a})+$ $\mathcal{R}_2(\mathbf{a})=0$ 时, 称为二人零和标准形式博弈。

### 定义 2.标准形式博弈策略

给定标准形式博弈 $G=\left\langle N,\left(A_i, \mathcal{R}_i\right)_{i \in N}\right\rangle$, 对于任意玩家 $i$, 其策略 $\sigma_i$ 为集合 $A_i$ 上的概率分布 $\Delta\left(A_i\right) ; \sigma_i\left(a_i\right)$ 表示个体 $i$ 使用 行动 $a_i \in A_i$ 的概率.



http://www.aas.net.cn/cn/article/doi/10.16383/j.aas.c220680?viewType=HTML

http://cjc.ict.ac.cn/online/onlinepaper/zl-202297212302.pdf

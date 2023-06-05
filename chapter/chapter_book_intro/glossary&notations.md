

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-09 20:32:52
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-04 20:32:53
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 字母表和中英术语对照

## 字母表

这里只列出常用字母。部分小节会局部定义的字母，以该局部定义为准。

一般规律： 大写是随机事件或随机变量，小写是确定性事件或确定性数值。衬线体（如Times New Roman字体）是数值，非衬线体（如Open Sans字体）则不一定是数值。粗体是向量或矩阵。花体是集合。


| 符号（Notations）                 | 意义表示（Meaning） |[2]
| :---                     | :--- |
| $\mathcal {S}$           | 状态空间（State space）：所有合法状态的集合 |
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

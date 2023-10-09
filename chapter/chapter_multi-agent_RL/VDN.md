

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-11 20:04:31
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-10 03:17:09
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# VDN (Value Decomposition Networks)

## 适用于合作型MARL问题

合作型多智能体强化学习问题，即Dec-POMDP（所有智能体共享同一个Reward信号，但每个智能体有各自的观测集、动作集，所有智能体的联合观测可以只包含部分和环境状态S有关的信息）。

虽然VDN本身对每个智能体是否相同（Invariance）没有要求，但如果每个智能体彼此相同，则可以采用**Weight sharing技巧**来提高样本利用率（Sample Efficiency），从而加速训练。

### 原传统方法介绍

解决合作型多智能体强化学习问题（具体指Dec-POMDP）的传统方法有两类：

- 第一类是把所有智能体看做一个联合智能体，它的观测、动作分别为所有智能体的联合观测、联合动作，然后再套用单智能体强化学习算法来训练；
- 第二类是把每个智能体看做独立的智能体来通用单智能体强化学习算法分别训练。这两类分别有各自的问题。

### 各自的问题

第一类方法的问题有两点：

1. 联合智能体的联合状态集、联合动作集的基数是随着智能体的个数的增长呈指数增长的（本算法所面向的问题的状态集和动作集均为有限集，且为了便于分析，我们假设每个智能体都是相同的）。因此当智能体很多的时候模型规模会很大，训练所需要的样本数量也很多。
2. 联合智能体训练完成后实际部署时需要实现一个用于共享每个智能体观测信息的通信信道。当智能体很多时，需要通信的观测信息的信息量是巨大的，因此实现这个信道是不现实的。


第二类方法的问题也有两点：

1. 由于对每个智能体来说，其他智能体是环境的一部分，但其他智能体是不断学习的，因此环境是不断变化的。单智能体强化学习已有的理论保证主要都是在环境平稳的假设下成立的，环境的变化会使算法失去理论保证。
2. 智能体在训练时无法区分队友的探索行为和环境本身的随机性，也无法区某个奖励究竟是自己的行为导致的，还是队友的行为导致的。这些问题最终会使训练变得不稳定。

## VDN的集中训练-去中心化部署的策略

由于这两类方法均有根深蒂固的缺陷，且目前难以解决，所以VDN采用了集中训练-去中心化部署的策略来应对前两类的解决方案的种种缺点。但是基于价值的集中训练-去中心化部署的方法需要满足IGM（个体-全局最大化，individual-global maximization）条件。

为了满足IGM条件，VDN采取了求和的方式来整合每个Q函数，并通过反向传播来实现隐性信用分配。当存在多个相同的智能体时，VDN还可以采取共享参数的策略来提高样本利用率。[2]




VDN (value decomposition networks) 也是采用对每个智能体的值函数进行整合，得到一个联合动作值函数。

VDN假设中心式的 $Q_tot$ 可以分解为各个 $Q_a$ 的线性相加。

令 $\tau=\left(\tau_1, \ldots, \tau_{\mathrm{n}}\right)$ 表示联合动作-观测历史，其中，
- $\tau_{\mathrm{i}}=\left(\mathrm{a}_{\mathrm{i}, 0}, \mathrm{o}_{\mathrm{i}, 0}, \ldots, \mathrm{a}_{\mathrm{i}, \mathrm{t}-1}, \mathrm{o}_{\mathrm{i}, \mathrm{t}-1}\right)$ 为动作-观测历史，
- $\mathrm{a}=\left(\mathrm{a}_1, \ldots, \mathrm{a}_{\mathrm{n}}\right)$ 表示联合动作。

$\mathrm{Q}_{\mathrm{tot}}$ 为联合动作值函数， $\mathrm{Q}\left(\tau_{\mathrm{i}}, \mathrm{a}_{\mathrm{i}} ; \theta_{\mathrm{i}}\right)$ 为智能体的局部动作值函数，局部值函数只依赖于每个智能体的局部观测。

VDN采用的方法就是直接相加求和的方式
$\mathrm{Q}_{\mathrm{tot}}=\sum_{\mathrm{i}=1}^{\mathrm{n}} \mathrm{Q}\left(\tau_{\mathrm{i}}, \mathrm{a}_{\mathrm{i}} ; \theta_{\mathrm{i}}\right)$

虽然 $\mathrm{Q}\left(\tau_{\mathrm{i}}, \mathrm{a}_{\mathrm{i}} ; \theta_{\mathrm{i}}\right)$ 不是用来估计累积期望回报的，但是这里依然叫它为值函数。

分布式的策略可以通过对每个 $Q\left(\tau_i, a_i ; \theta_i\right)$ 取max得到。

$V D N$ 假设中心式的 $\mathrm{Q}_{\mathrm{tot}}$ 可以分解为各个 $\mathrm{Q}_{\mathrm{a}}$ 的线性相加，而 $\mathrm{QMIX}$ 可以视为 $V D N$ 的拓展[1]


[1]: https://blog.csdn.net/wzduang/article/details/115874734?spm=1001.2014.3001.5502
[2]: http://turingai.ia.ac.cn/share_algorithm/detail/51

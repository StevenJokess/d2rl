

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-01 04:16:38
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-01 01:55:08
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Partially observable Markov decision processes (POMDP)

## 回顾 MDP

马尔可夫决策过程 (Markov decision process, MDP) 是一个具备完全信息情形下的决策过程。即智能体在每个时刻可以观测到其真实的状态 $s_t, s_t \in S$, 在经历行动 $a_t \in A$ 之后, 可以到达下一个可观测的状态, $s_{t+1}$, 从而可以观测到真实的状态转移 $T\left(s_t, a_t, s_{t+1}\right)$ :

$$
T\left(s_t, a_t, s_{t+1}\right) = P\left(s_{t+1} \mid s_t, a_t\right): S \times A \times \mathcal{S} \rightarrow \mathbb{R}^{+}
$$

也可以在每个时刻获得奖励的具体数值 $r_t\left(s_t, a_t, s_{t+1}\right)$ :

$$
r_t\left(s_t, a_t, s_{t+1}\right): S \times A \times S \rightarrow \mathbb{R}
$$

## 部分可观测（partially observed）

部分可观测马尔可夫决策过程（partially observab
但对于那些由于各种客观原因的限制, 无法观测到完全信息的决策场景和系统, 我们可以预设真实状态的决策过程依然是一个标准的 MDP。只是智能体不能直接观察底层真实状态, 仅能观察到其中一部分的信息 $o_t \in O$ , 此刻称这个环境是部分可观测（partially observed）的。

## 部分可观测马尔可夫决策过程（partially observable Markov decision process，POMDP）

在这种情况下，强化学习通常被建模成马尔可夫决策过程的泛化形式————部分可观测马尔可夫决策过程（partially observable Markov decision process, POMDP）的问题。

我们发现, 相比于标准的马尔可夫决策过程 , 在部分观测的信息限制之下, 真实状态依然可以完整确定一个观测, 但部分观测已经无法作为一个完备统计量, 完整还原真实状态, 即对于任意一个观测序列 $\left[o_t\right]$ 和动作序列 $\left[a_t\right]$, 其对应的真实状态 $s_t$ 可能是整个状态空间 $S$ 中的任意一个, 无法唯一确定:

$$
s_t \sim P\left(\cdot \mid\left[o_1, a_1, o_2, \ldots, o_{t-1}, a_{a-1}, o_t\right]\right)
$$

因此, POMDP 可以被视为某种 Infinite-State MDP。此外, 相比于马尔可夫决策过程, 部分观测马尔可夫决策过程可以额外引入观测信息 $o_t$ 和真实状态 $s_t$ 之间的函数关系, 称为观测函数, $\Pi\left(o_{t+1}, s_t, a_t\right):$

$$
\Pi\left(o_{t+1}, s_t, a_t\right)=P\left(o_{t+1} \mid s_t, a_t\right): S \times A \times \mathcal{O} \rightarrow \mathbb{R}^{+}
$$

一般来说, 解决 POMDP 问题有几种常用的方法, 在课程正文中也已分别从数据建模, 网络结构, 训练方法等角度展开讲解。



[1]: http://coregroup.snu.ac.kr/teaching/
[2]: https://www.zhihu.com/question/496058048/answer/2889510108
[3]: https://github.com/opendilab/PPOxFamily/blob/main/chapter5_time/chapter5_supp_belief.pdf

TODO:Introduction to Robotics and Autonomous Systems, Spring 2019 (SNU)

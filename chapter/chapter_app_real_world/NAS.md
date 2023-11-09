

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-11-09 07:00:17
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-11-09 07:04:44
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 神经结构搜索（Neural Architecture Search, NAS）

## 词源
深度学习自从2016年发展到现在，越来越多的网络结构被研究出来解决不同领域的问题。但随着研究的深入，深度网络的模型参数经常需要大量的调试。2017年的国际机器学习大会 (International Conference on Machine Learning, ICML) 上，谷歌提出应用强化学习方法优化神经网络的结构和参数，从此开启了一个新的研究方向一神经结构搜索。

## 基本内容

神经结构搜索的主要研究问题包括搜索空间构建和优化算法设计。
神经结构搜索的搜索空间被认为是一个带有约束子空间。神经结构搜索的搜索空间直接影响到优化的难度，神经结构搜索的研究重点之一就在于如何构造一个高效的搜索空间。由于深度学习的结构比较复杂，层次化的结构已经被证实十分有效，因此一开始的搜索空间的构造仍然以链式结构为主。链式搜索空间 (chain-structured search space) 首先被提出，主要的思想是将不同的操作单元组合在一起，这样的搜索空间也被称为全局搜索空间（global search space）。全局搜索空间限制了神经网络的整体架构和链接方向，神经结构搜索需要调整每一层所做的操作和对应的参数。每一层的操作有不同的选择，例如可以是卷积、池化、线性变换等。全局搜索空间相对来讲比较灵活，可以允许神经网络变换出各种结构（只要设计时允许跨层连接或者层间连接），但是问题也很明显，那就是巨大的搜索空间使得很多优化算法都没办法快速解决它。全局搜索空间带来了十分昂贵的计算代价。

对于非常耗时的神经结构搜索来说，需要更高效的优化方法来减少搜索次数。最早被用于神经结构搜索的搜索方法是强化学习 (reinforcement learning)。强化学习将每一层的网络结构作为一个动作，这个动作的奖励就用这个模型的评估结果来表示。神经结构搜索中不同的强化学习算法的差别是如何设计智能体的搜索策略，比较常用的方法有REINFORCE、Q-Learning及 Monte Carlo Tree Search。

## 应用

神经结构搜索已成为深度学习成功应用的必要步骤，其发展已成为提升深度学习可解释性、鲁棒性的重要途径。神经结构搜索的主要研究方向包括给出搜索结构的理论保证、提升搜索算法的计算效率等。[1]

[1]: https://www.zgbk.com/ecph/words?SiteID=1&ID=413463&Type=bkzyb&SubID=201750

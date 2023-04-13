

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-13 01:20:35
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-13 01:44:40
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 自我博弈（self-play）

## FP

虚拟博弈（Fictitious play， FP）是在规范式博弈（单步博弈）中学习纳什均衡的常用方法。虚拟玩家们选择最优反应（都以最大化自身利益为原则而做出的动作）。
FP是一种从自我对弈中学习的博弈论模型，可以求解纳什均衡，但是要求有近似最佳反应和扰动的平均策略更新（解决了这两个核心问题，纳什均衡也就基本解出来了），这其实特别适合机器学习。（可以用一个网络（Q网络）去估计最佳反应，而平均策略更新可以通过不停更新一个policy网络实现）

FP通常用于规范形式的博弈（单步博弈），很难用到扩展形式的博弈（多步博弈）。

## XFP

XFP解决了这个局限，将策略表示原本规范形式中的各玩家策略线性组合（序列化之后应该还是原本n个玩家策略的线性组合，没毛病，因为搞来搞去就n个策略，当前的策略肯定从这n个策略中组合产生），然后用一个传统算法去算平均策略。

XFP没有用机器学习的方法，而FSP相当于是一种基于机器学习的近似XFP。FSP将两个核心问题（计算最优反应和更新平均策略）用两个网络去做，就可以完美地近似XFP了。

## FSP

FSP将单步博弈拓展到多步博弈（extensive-form game，扩展式博弈）。

## NFSP

当应用到冷扑（Leduc poker）时，神经虚拟自我博弈（Neural Fictitious Self-Play，NFSP）达到了纳什均衡，而普通的强化学习方法不行。在现实世界游戏德州扑克中，NFSP取得了最领先的成绩，超越了人类。[2]

NFSP 就是引入神经网络近似函数的 FSP，是一种利用强化学习技术来从自我博弈中学习近似纳什均衡的方法。解决了三个问题：

1. 无先验知识 NFSP agent 学习
1. 运行时不依赖局部搜索
1. 收敛到自我对局的近似纳什均衡

这是一般的不完美信息二人零和博弈。虚拟对弈同样也会收敛到合作、势力场博弈的纳什均衡。所以 NFSP 也能够成功应用在这些博弈上。另外，近期的研究关于连续空间行动的强化学习（Lillicrap et al. 2015）也能够应用在连续行动博弈中，目前的博弈论方法并不能直接处理这样的情形。

然而我们需要知道了解相关的基础，才能够真切地了解这篇文章给我们带来了什么启发。本文在对原文理解基础上进行翻译，后续会继续增加向前的回溯和向后的扩展。

当应用于Leduc扑克时，NFSP解出了纳什均衡，而常见的强化学习方法则会得到不同的结果。 在现实世界规模的扑克游戏——有限注德州扑克中，NFSP学到了一种基于领域专业知识的策略，该策略可以超越人类领先水平。

[1]:
[2]: https://blog.csdn.net/weixin_37837522/article/details/91907661

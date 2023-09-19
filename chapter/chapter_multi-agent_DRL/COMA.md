

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-12 20:38:49
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-14 21:57:06
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 反事实多智能体策略梯度法方法（Counterfactual Multi-Agent Policy Gradients, COMA）

在合作式的多智能体学习问题中，每个智能体共享奖励（即在同一个时刻获得相同的奖励），此时会存在一个 “置信分配” 问题（credit assignment）：如何去评估每个智能体对这个共享奖励的贡献？

COMA 方法在置信分配中利用了一种反事实基线：将智能体当前的动作和默认的动作进行比较，如果当前动作能够获得的回报高于默认动作，则说明当前动作提供了好的贡献，反之则说明当前动作提供了坏的贡献；默认动作的回报，则通过当前策略的平均效果来提供（即为反事实基线）。在对某个智能体和基线进行比较的时，需要固定其他智能体的动作。当前策略的平均效果和优势函数的定义如下：

COMA 方法结合了集中式训练、分布式执行的思想：分布式的个体策略以局部观测值为输入、输出个体的动作；中心化的 critic 使用特殊的网络结构来输出优势函数值。

具体地，critic 网络的输入包括了全局状态信息 s、个体的局部观测信息 o、个体的编号 a 以及其他智能体的动作，首先输出当前智能体不同动作所对应的联合 Q 值。然后， 再经过 COMA 模块，使用输入其中的智能体当前策略和动作，计算反事实基线以及输出最终的优势函数。

图 8：(a) COMA 方法中的 actor-critic 框架图，(b) actor 的网络结构，(c) critic 的网络结构（包含了核心的 COMA 模块来提供优势函数值）。图源：[10]

## 优缺点


优点：

1. 引入反事实推理的概念非常具有创新性，且给出了方法理论上的证明；
1. 感觉确实是从问题本身出发，一步步解决各种上到顶层算法设计下到降低算法复杂度的各种方法

缺点：

1. 论文的方法未对比一些其他集中训练分步执行的算法，对比的baselines都比较弱；
https://mayi1996.top/2020/08/13/Counterfactual-Multi-Agent-Policy-Gradients/

[1]: https://www.thepaper.cn/newsDetail_forward_9829763

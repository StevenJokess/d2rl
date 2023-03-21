

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-21 22:38:59
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-22 02:27:10
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 博弈论

## Le Her 游戏

冯·诺伊曼的极小极大值定理也就是说，具有有限多个纯策略的两人零和博弈在Maximin和Minimax 策略相同的情况下有解。这可以保证玩家在最坏的情况下最小化可能的损失。[1]


# Min-max搜索

1928：John von Neumann 的 minimax 定理给出了关于对手树搜索的方法，这形成了计算机科学和人工智能的从诞生至今的决策制定基础。

其思路为，如果树的层数比较浅，我们可以穷举计算每个节点输赢的概率，那么可以使用一种最简单的策略，叫做minmax算法。基本思路是这样的，从树的叶子结点开始看，如果是本方回合就选择max的，如果是对方回合就选择min的（实际上这也是假设对方是聪明的也会使用minmax算法）。这样在博弈论里面就达到一个纳什均衡点。因此，我们可以推出著名的几个理论比如井字棋是必定和棋，五子棋在8*8以下的棋盘是和棋，以上的则是先手必胜。


流程如上图所示。当然，我们可以使用alpha-beta对这个搜索树剪枝。

但是如果每一层的搜索空间都很大，这种方法就极其得低效率了，以围棋为例我们把围棋的每一步所有可能选择都作为树的节点，第零层只有1个根节点，第1层就有361种下子可能和节点，第2层有360种下子可能和节点，这是一颗非常大的树。如果我们只有有限次评价次数，minimax方法就是不可行的（往往只能跑2-3层，太浅了）[3]



## minimax theorem

令 X \subset \mathbb{R}^n 和 Y \subset \mathbb{R}^m 是紧凸集，如果 f: X \times Y \rightarrow \mathbb{R} 是一个连续的凸凹（convex-concave）函数，即：

f(\cdot, y): X \rightarrow \mathbb{R} 对于固定的 y 是凸的，且：

f(x, \cdot): Y \rightarrow \mathbb{R} 对于固定的 x 是凹的.

那么我们有

\min_{x \in X} \max_{y \in Y} f(x, y) = \max_{y \in Y} \min_{x \in X} f(x, y).

这个定理说的是对于一类特殊的函数，该函数沿着一个变量变化的方向是凸的，沿着另一个变量变化的方向是凹的（这可以视为一个马鞍面），那么在鞍点的时候，上式成立。[2]

[1]: https://zh.wikipedia.org/wiki/%E6%9C%80%E5%B0%8F%E6%9C%80%E5%A4%A7%E5%80%BC%E5%AE%9A%E7%90%86

[2]: https://www.zhihu.com/question/51080557/answer/671522746
[3]: ttps://zhuanlan.zhihu.com/p/520638488

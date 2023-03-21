

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-21 22:38:59
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-21 23:01:20
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 博弈论

## Le Her 游戏

冯·诺伊曼的极小极大值定理也就是说，具有有限多个纯策略的两人零和博弈在Maximin和Minimax 策略相同的情况下有解。这可以保证玩家在最坏的情况下最小化可能的损失。[1]




## minimax theorem

令 X \subset \mathbb{R}^n 和 Y \subset \mathbb{R}^m 是紧凸集，如果 f: X \times Y \rightarrow \mathbb{R} 是一个连续的凸凹（convex-concave）函数，即：

f(\cdot, y): X \rightarrow \mathbb{R} 对于固定的 y 是凸的，且：

f(x, \cdot): Y \rightarrow \mathbb{R} 对于固定的 x 是凹的.

那么我们有

\min_{x \in X} \max_{y \in Y} f(x, y) = \max_{y \in Y} \min_{x \in X} f(x, y).

这个定理说的是对于一类特殊的函数，该函数沿着一个变量变化的方向是凸的，沿着另一个变量变化的方向是凹的（这可以视为一个马鞍面），那么在鞍点的时候，上式成立。[2]

[1]: https://zh.wikipedia.org/wiki/%E6%9C%80%E5%B0%8F%E6%9C%80%E5%A4%A7%E5%80%BC%E5%AE%9A%E7%90%86

[2]: https://www.zhihu.com/question/51080557/answer/671522746

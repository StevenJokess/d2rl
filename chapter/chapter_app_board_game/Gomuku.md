

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-20 13:52:24
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-20 14:03:12
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 五子棋

## 五子棋的规则

五子棋( Gomuku ，又称 Five in a row) 是在 15x15 的棋盘上进行的游戏。

五子棋有许多不同的规则，其中最为简单的是无约束规则 (Free-style Gomuku) 无约束规则是这样的:

在一个回合中，两个玩家依次在交叉点上放置自己的棋子，当某个玩家让自己的棋子在垂直方向、水平方向或对角方向中任意一个方向有至少5个棋子(包括5个)连成一条线，则该玩家获胜 如果棋盘已满，但是没有任何一方获胜，则该回合为平局 五子棋还有其他规则，如 Swap2 规则、 Soosorv-8 规则等。

## 五子棋与井字棋

井字棋(Tic-Tac-Toe) 是在 3x3 棋盘上进行的游戏。它的规则类似于五子棋的无约束规则，任意一方将三个自己的棋子连成一线即可获胜。

无约束规则的五子棋和井字棋都是 (m,n,k) 连线游戏( (m,n,k) k-in-a-row game 的特殊情形 在学术界 (m k) 连线游戏定义为在 m x n 棋盘上的回合制游戏 在一个回合中，两个玩家依次在交叉点上放置自己的棋子，当某个玩家让自己的棋子在垂直方向、水平方向或对角方向中任意 个方向有至少k个棋子(包括k个)连成一条线，则该玩家获胜果棋盘己满，但是没有任何一方获胜，则该回合为平局 无约束五子棋就是 (15,15,5) 连线游戏，井字棋就是 (3,3,3) 连线游戏。研究人员已经证明(m,n,k)连线游戏在(m,n,k)的许多取值下采用最佳策略的对弈结果 根据 Wikipedia 上" k-game" 的条目，目前已经证明，在双方都采用最优策略的情况下，有以下结论：

- $k=1$ 和 $k=2$ : 黑棋胜, 除了 $(1,1,2)$ 和 $(2,1,2)$ 显然是平局。
- $k=3$ : 井字棋 $(3,3,3)$ 是平局, $\min \{m, n\}<3$ 也是平局, 其他情况黑棋胜。实际上, 对于 $k \geqslant 3$ 并且 $k>\min \{m, n\}$ 的情况, 都是平局。
- $k=4:(5,5,4)$ 和 $(6,6,5)$ 是平局, $(6,5,4)$ 黑棋胜, $(m, 4,4)$ 对于 $m \geqslant 30$ 是黑棋胜, 对 于 $m \leqslant 8$ 是平局。
- $k=5:(m, m, 5)$ 在 $m=6,7,8$ 的情况下是平局, 对于无限制五子棋的情况 $(m=15)$ 是 黑棋胜。
- $k=6,7,8: k=8$ 在无限大的棋盘上是平局, 在有限大的情况下没有完全分析清楚。 $k=6$ 或 $k=7$ 在无限大的棋盘下也没有分析清楚。 $(9,6,6)$ 和 $(7,7,6)$ 是平局。
- $k \geqslant 9$ 是平局。[1]

开源库：https://github.com/colingogogo/gobang_AI#gobang_ai

[1]: E:/BaiduNetdiskDownload/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8BPDF+%E6%BA%90%E4%BB%A3%E7%A0%81/%E3%80%8A%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E5%8E%9F%E7%90%86%E4%B8%8Epython%E5%AE%9E%E7%8E%B0%E3%80%8B.pdf

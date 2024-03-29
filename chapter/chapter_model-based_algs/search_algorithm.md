

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-26 23:58:54
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-12 17:59:34
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 搜索求解

- 许多复杂的问题可以“逐步”解决。
- 每走一“步”，问题就达到新的状态。

## 搜索算法（search algorithm）的定义

搜索算法（search algorithm）：利用计算机的高性能来有目的地穷举一个问题解空间（solution space）的部分或所有的可能情况，从而求出问题的解的一种方法。一般有枚举算法、深度优先搜索（Depth First Search, DFS）、广度优先搜索（Breadth First Search, BFS）、Ａ*算法、回溯算法、蒙特卡洛树搜索、散列函数等算法。

通常通过搜索前，根据条件降低搜索规模，根据问题约束条件进行剪枝，利用搜索过程中的中间解，避免重复计算这几种方法进行优化。

搜索算法可以分为树搜索和图搜索。

## 状态转移图（state transition graph）与搜索树（search tree）

### 状态转移图（state transition graph）

当前步和下一步之间存在联系，下一个状态画成图，就成为“状态转移图”。[4]

状态转移图（state transition graph）或状态空间图 是一种图形化表示方式，用来表示系统在不同状态下通过不同动作转移到下一状态的关系。

状态转移图通常由一组节点和边组成，其中节点表示状态，边表示状态之间的转移。目标测试是当前节点是否是目标节点

其搜索目标就是在状态转移图中寻找最优的路线，又称为“状态图搜索”方法。

注意：每个状态只出现一次！状态图有时很大难以完全构建，但它是一个有用的概念。

### 搜索树（search tree）

搜索树是状态转移图的一种特例，可以看作是状态转移图在搜索算法中的一种实现方式，它在搜索过程中记录了搜索路径和搜索状态的展开情况，帮助算法在搜索空间中进行有效的搜索。

搜索树的建立。树是一种数据结构，是包含n个节点的有穷集，其中每个元素称为结点（node），有一个特定结点被称为根结点或树根（root），度为零的结点称为叶节点或终端结点，排在前的结点叫父结点，其后的结点叫子结点。

完成搜索的过程就是找到一条从根节点到目标结点的路径，找出一个最优解。

### 状态图 vs 搜索树

![状态图 vs 搜索树](../../img/state_graph_vs__search_tree.png)

有一个4节点状态图,它的搜索树有多大（从S 状态开始）？无穷大

![状态图 vs 搜索树 例子](../../img/state_graph_vs__search_tree2.png)

存在许多重复的树枝结构！[6]

## 图搜索与数搜索

搜索算法可以分为树搜索和图搜索。

简单来说，树搜索算法可能会导致状态的重复，所以在图上往往使用图搜索，当然如果你可以保证你的算法不会遇到重复状态的话，也可以放心的使用树搜索。[5]

### 图搜索的伪代码：

- **function** GRAPH-SEARCH(problem) **returns** a solution,or failure
- initialize the frontier using the initial state of problem
- **initialize the explored set to be empty**
- **loop do**
  - **if** the frontier is empty **then return** failure
  - choose a leaf node and remove it from the frontier
  - **if** the node contains a goal state **then return** the corresponding solution
  - add the node to the explored set
  - expand the chosen node,adding the resulting nodes to the frontier **only if not in the frontier or explored set**

### 树搜索的伪代码：

- **function** TREE-SEARCH(problem)**returns** a solution,or failure
  - initialize the frontier using the initial state of problem
  - loop do
    - **if** the frontier is empty **then return** failure
    - choose a leaf node and remove it from the frontier
    - **if** the node contains a goal state **then return** the corresponding solution
    - expand the chosen node,adding the resulting nodes to the frontier

### 图搜索与树搜索的区别

容易看出，图搜索比树搜索多维护了一个`explored`队列，这个队列用来记录算法经过的节点，通过检查新的节点是否在这个队列中，图搜索避免了重复。

而两者都有的`frontier`队列，则是用来记录将要探索的节点。

算法开始的时候把初始节点放到 `frontier`里，接下来一直探索，如果`frontier`空了却没有找到最终的目标，说明目标是不存在的，算法返回failure。

### 搜索算法的组成部分

搜索算法划分成两个部分：控制结构（扩展结点的方式）和产生系统（扩展结点），所有的算法优化和改进主要都是通过修改其控制结构来完成的。

## 搜索算法的形式化描述

搜索算法的形式化描述：<状态、动作、状态转移、路径、测试目标> [2]

“最短路径问题”问题：寻找从Arad到Bucharest的一条路任，两足路径最短时问最少、价钱最经济？

![Romania Map作为例子](../.../img/Romania_Map_Example.png)

- 状态转移：对某一时刻对应状态进行某一状态转移种操作后，所能够到达状态。
- 路径：一个状态序列。该状态序列被路径一系列操作所连接。如从Arad到Bucharest所形成的路径。
- 目标测试：评估当前状态是否为所求解的目标状态。

### 盲目搜索（Un-informed Search）

盲目搜索（Un-informed Search）:其实称为 Brute-force Search （暴力搜索）更加合理一些。就是完全不考虑AI的目标和效用，纯粹的去以某种固定的方式去遍历游戏的状态树，以求找到一种合理的路径。这里最常用的两种搜索就是 广度优先（Breadth-first）和深度优先（Depth-first）。具体就不详细展开了，应该是任何一本讲数据结构的书上都会讲到的。

显然这样的算法效率是很低的，基本上是不实用的。但是其一些变体（iterative width search， Sentient Sketchbook）还是能在游戏的某些方面上应用的。[3]更多的关于Uninformed Search的内容可以参考  Artificial Intelligence and Games - A Springer Textbook 第四章的内容。

## 启发式搜索（heuristic search）

搜索算法：启发式搜索（有信息搜索）

在搜索的过程中利用与所求解问题相关的辅助信息，其代表算法为贪婪最佳优先搜索(Greedy best-first search)和A*搜索。

- 辅助信息：所求解问题之外、与所求解问题相关的特定信息或知识
- 评价函数(evaluation function)f(n)：从当前节点出发，根据评价函数来选择后续节点
- 启发函数(heuristic function)h(n)：计算从节点到目标节点之间所形成路径的最小代价值。这里将两点之间的直线距离作为启发函数。

### 贪心最优先搜索（Greedy　best-first search）

贪心算法的含义是：评价函数ｆ(n)=启发函数 h(n)

求解问题时，总是做出对当前来讲最好的选择。它是“短视”算法，是最低开销的启发式。

#### 不足之处：

1. 贪婪最佳优先搜索不是最优的。
2. 启发函数代价最小化这一目标会对错误的起点比较敏感。
3. 贪婪最佳优先搜索也不是完备的。所谓不完备即它可能沿着一条无限的路径走下去而不回来做其他的选择尝试，因此无法找到最佳路径这一答案。
4. 在最坏的情况下，贪婪最佳优先搜索的时间复杂度和空间复杂度都是 $O(b^m)$,其中b是节点的分支因子数目、m是搜索空间的最大深度。

因此，需要设计一个良好的启发函数

### A*搜索

A*(A-Star) 算法是一种静态路网中求解最短路径最有效的直接搜索方法。

评价函数 f(n)=g(n)+h(n)，g(n)是当前最小开销代价， h(n）是后续最小开销代价

保证找到最短路径（最优解）的条件，关键在于估价函数f(n)的选取，

距离估计与实际值越接近，估价函数取得就越好

- f(n)是从初始状态经由状态 n 到目标状态的代价估计，
- g(n)是在状态空间中从初始状态到状态 n 的实际代价。
- h(n)是从状态ｎ到目标状态的最佳路径的估计代价。

对于路径搜索问题，状态就是图中的结点，代价就是距离。

### Ａ*算法最优的条件

为保证Ａ*算法是最优（optimal），需要启发函数是可容的（admissible Heuristic）和一致的（consistency），也称单调性（monotonicity）

- 最优：不存在另外一个解法能得到比A*算法所求得解法具有更小开销代价。
- 可容(admissible)：专门针对启发函数而言，即启发函数不会过高估计(over-estimate)从节点n到目标结点之间的实际开销代价（即小于等于实际开销）。如可将两点之间的直线距离作为启发函数，从而保证其可容。
- 一致性（单调性）：假设节点n的后续节点是n',则从n到目标节点之间的开销代价一定小于从n到n'的开销再加上从n'到目标节点之间的开销，即 $h(n)≤c(n,a,n)+h(n')$。这里n'是n经过行动a所抵达的后续节点，c(n,a,n)指n'和n之间的开销代价。

### 应用在最短路径上的A*算法是否最优

即是否满足条件：启发函数是可容的（admissible Heuristic）和一致的（consistency）

证明：

- 可容的：
  - 将直线距离作为启发函数h(n)，则启发函数一定是可容的，因为其不会高估开销代价。
  - g(n)是从起始节点到节点n的实际代价开销，且f(n)=g(n)+h(n)，因此f(n)不会高估经过节点n路径的实际开销。
- 一致的：
  - h(n)≤c(n,a,n')+h(n')构成了三角不等式。这里节点n,节点n'和目标结点 $G_n$ 之间组成了一个三角形。如果存在一条经过节点n'，从节点n到目标结点$G_n$的路径，其代价开销小于h(n),则破坏了h(n)是从节点n到目标结点 $G_n$ 所形成的具有最小开销代价的路径这一定义。

### A*的应用与改进

A* Planner算法赢得了2009年马里奥AI大赛的冠军。

很自然的，A*算法可以用来做游戏和其他领域的寻路算法。因为在寻路(pathfinding)上，目标和当前状态的距离很好定义，可以简单的定义为当前点和终点的物理意义上的距离，因此可以很好的定义在当前状态下的最好的下一个状态。

当然，为了提高在游戏庞大的状态空间中的寻路效率，也有很多该算法的改进，如grid-based pathfinding，在不少游戏上都有公开的benchmarks (http://movingai.com/benchmarks): 星际争霸(StarCraft),魔兽世界3: 混乱之治(Warcraft III Reign of Chaos)，龙腾世纪:起源（Dragon Age: Origins）。

此外，在某些特定游戏中，只要我们能够很好的定义游戏中的状态之间，以及最终的目标的距离，A*算法也可以构建出非常不错的NPC。比如像超级马里奥(Super Mario Bros)这样的横版或者竖版过关游戏，其当前状态都是和Agent在当前关卡所处的坐标相关的，而最终的目标永远是关卡最右端的结束点，因此可以在这个基础上加入一些当前的障碍物，怪物等的信息，就能比较准确的估计出当前状态的好坏。

### 相关结论

- Tree-search的A* 算法中，如果启发函数 h(n) 是可容的，则A* 算法是最优的和完备的；
- 在Graph-search的A* 算法中，如果启发函数 h(n) 是一致的，A* 算法是最优的。
- 如果函数满足一致性条件，则一定满足可容条件；反之不然。
- 直线最短距离函数既是可容的，也是一致的。

### 证明

如果A*算法将节点n选择作为具有最小代价开销的路径中一个节点，则n一定是最优路径中的一个节点。即最先被选中扩展的节点在最优路径中。

证明：反证法。假设上述结论不成立。则存在一个未被访问的节点n'位于从起始节点到节点n的最佳路径上。根据非递减性质，存在f(n)≥f(n')，则n'应该已经被访问过了(expanded)。

因此，无论什么时候，一旦一个节点被访问到，它一定位于从起始节点到它自己之间的最佳路径上。


[1]: https://zhuanlan.zhihu.com/p/48740530#%E7%AC%AC%E4%BA%8C%E8%AE%B2%E3%80%80%E6%90%9C%E7%B4%A2%E6%B1%82%E8%A7%A3
[2]: https://www.youtube.com/watch?v=kwWyKjeuL3E
[3]: https://cloud.tencent.com/developer/news/257574
[4]: https://www.bilibili.com/video/BV1sV4y1G7ob/?spm_id_from=333.337.search-card.all.click&vd_source=bca0a3605754a98491958094024e5fe3
[5]: https://zhuanlan.zhihu.com/p/187283548#%E6%90%9C%E7%B4%A2%E7%AE%97%E6%B3%95%E7%9A%84%E8%83%8C%E6%99%AF%E5%92%8C%E5%AE%9A%E4%B9%89
[6]: https://qiqi789.github.io/teaching/AI/lecture03.pdf

> https://chat.openai.com/chat;搜索树是状态转移图的特例吗？

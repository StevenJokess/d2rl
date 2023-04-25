

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-29 20:42:36
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-25 22:40:15
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 围棋

围棋是一项历史悠久的棋类项目。围棋棋盘是19×19的网格，纵横各19路直线，形成361个交叉点。围棋棋子分为黑白两种，白子180颗，黑子181颗，共361颗棋子。

## 规则

围棋规则比较简单，主要有以下7条规则。

（1）博弈黑白双方，依次落子。落子在纵横网格的交叉点上。
（2）空棋盘开局，黑方先走，落黑子；白方后走，落白子。
（3）在轮到一方落子时，一方可以选择放弃落子（虚着）[4]。如果双方都放弃，比赛结束。
（4）下子后棋子不能重复再下在相同位置，也不能向其他点移动。
（5）一片同色棋子的所有连线相邻的交叉点（纵横线），都被对手占满后，则这一片同色棋子就被对手吃掉而清空。
（6）一方的领土是他的棋子占的位置以及包围的交叉点。
（7）占位多的一方获胜。

### 胜负判定

- 棋盘总点数的一半180.5点为归本数。一方总得点数超过此数为胜，等于此数为和，小于此数为负。
- 黑棋由于先行，需贴还3又3/4，即需贴7.5子[3]7目半是中国的规则，日韩是贴6目半。[5]

### 水平判断

围棋的水平可分为初学者、业余与职业三类。其中初学者（Beginner）分为30级，业余（Master）分为7段，职业（Professional）分为9段，职业9段是顶级水准。成为职业9段的选手，不仅需要有天赋，还需要付出数十年的时间和精力。职业9段的棋力水准也是计算机围棋需要达到的目标

### 属于完美信息博弈

围棋有着明确的游戏规则，其中不存在随机或运气成分（如掷骰子或洗牌）。这类游戏都可以归类为所谓的完美信息博弈（perfect information games）。在完美信息博弈中，每时点参与人采取一个行动，每个参与人在其决策的同时知道以前所有的行动。[6]

## 计算机围棋

计算机围棋围棋是一项搜索空间巨大的棋类游戏，被称为人工智能挑战的一项王冠。计算机棋类游戏被认为是AI界的果蝇（Fruit Fly），是人们研究最多的AI游戏。这类游戏提供了一个验证新想法，与人类较量水平，以及AI算法之间容易互相比较的实验沙箱。

据senseis.xmp.net网站介绍，第一个使用UCT算法的围棋程序是MoGo。一个职业五段在九乘九的棋盘上以二比一战胜围棋程序MoGo，MOGO 在9X9 棋盘上战胜了台湾的围棋世界冠军周俊勋，而且在19X19 棋盘上被让7 子的情况下战胜周俊勋。[10]MoGo在2008年的美国围棋公开赛上，第一次在19x19的全尺寸棋盘上击败了职业选手（当然与AlphaGo不同，这位职业选手让了9个子）。[6]

2009年，计算机围棋Fuego第一次在9×9棋盘上击败9段选手。在接下来的几年里，在有让子的情况下，计算机围棋在19×19的棋盘上也能击败人类职业选手。东京电气大学杯（UEC杯）是比较知名的计算机围棋的比赛。自2007年开始，已经累计办了10届。比较知名的计算机围棋有Fuego（第4届冠；Remi Coulom）作者、CrazyStone（第1、2、6、8届冠；作者Remi Coulom）、Zen（第5、7届冠；DeepZenGo 第9届冠）、Erica（第五届亚；黄世杰）、Darkforest（第9届亚，作者田渊栋），人们使用这些围棋先后参赛并获得名次。

2017年，腾讯公司的计算机围棋程序绝艺（Fineart）击败DeepZenGo，获得了第10届UEC杯冠军。

关于计算机科学家征服计算机围棋的计划，参见以下文献：Sylvain Gelly，Levente Kocsis，Marc Schoenauer，et al.The Grand Challenge of Computer Go：Monte-Carlo Tree Search and Extensions.Communications of the ACM，Vol.55，no.3(2012)：106-113.


## 强化学习元素

### 第一结构

- 环境（Environment）：棋盘
- 智能体（Agent）：我和对手
- 目标（Goal）：赢

### 第二结构

- 状态（State）
- 行动（Action）
- 奖励（Reward）

- 起始状态 $S_1$：Empty Board
- 第一步行动 $A_1$：白棋星位落子
- 第一步奖励 $R_1$：0

对手：黑子落在对角的小目

- 下一状态 $S_2$：一颗白棋在一个角的星位、一颗黑子落在对角的小目
- $A_2$
- $R_2$

- ...

### 第三结构

- 价值函数
  - 状态价值函数
  - 状态行动价值函数：$S_1$有361个可能的行动，可能的行动的价值。价值是赢棋的概率
- 策略：选择价值最大，即赢棋概率最大的行动。

### 符合特点

- 试错 trial and error：各种定式，可复盘学习价值
- 延迟奖励 Delayed reward：到棋局结束才获得奖励

## 重要问题

Exploration vs. Exploitation

例子：AlphaGo带来新定式、骚操作，尖顶。

## 复杂性

- 状态空间复杂度（State-space Complexity）：$3^361 ≈ 10^172$；对比，Tic-Tac-Toe：$10^4$，Chess：$10^43$
- 游戏-树尺寸（Game-tree Size）：$361! ≈ 10^768$
- 排除规则中不可能的，游戏-树复杂度（Game-tree Size）Complexity）：$250^150 ≈ 10^360$；对比，Tic-Tac-Toe：$10^5$，Chess：$10^123$，用质子去填满全宇宙，需要：$10^122$个

#

## 下棋

- 作弊：悔棋，
- 计划：在头脑里预演，来提前放弃不好的动作。

[1]: https://www.bilibili.com/video/BV1LZ4y1u7Rn/?spm_id_from=333.999.0.0&vd_source=bca0a3605754a98491958094024e5fe3
[2]: https://www.bilibili.com/video/BV1464y127i7/?spm_id_from=333.999.0.0&vd_source=bca0a3605754a98491958094024e5fe3
[3]: https://www.bilibili.com/video/BV1V44y1n793?p=32&vd_source=bca0a3605754a98491958094024e5fe3
[4]: https://baike.baidu.com/item/%E4%B8%AD%E5%9B%BD%E5%9B%B4%E6%A3%8B%E8%A7%84%E5%88%99/5425065
[5]: https://www.zhihu.com/question/25576990/answer/269988801
[6]: https://gwb.tencent.com/community/detail/106017
[7]: https://www.global-sci.org/intro/article_detail/auth/11441.html

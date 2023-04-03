

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-29 20:42:36
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-03 23:31:35
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 围棋

## 规则

### 下法

1. 对局双方各执一色棋子。
1. 空枰开局。
1. 黑先白后，交替着一子于棋盘的点上。
1. 棋子下定后，不再向其他点移动。
1. 轮流下子是双方的权利，但允许任何一方放弃下子权而使用虚着。[4]

### 胜负判定

- 棋盘总点数的一半180.5点为归本数。一方总得点数超过此数为胜，等于此数为和，小于此数为负。
- 黑棋由于先行，需贴还3又3/4，即需贴7.5子[3]7目半是中国的规则，日韩是贴6目半。[5]

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

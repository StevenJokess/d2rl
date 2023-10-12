# 吃豆人

主机游戏吃豆人( PacMan ，见图 1-2 )

街机游戏吃豆人(本图片改编自 https://en.wikipedia.org/wiki/Pac-Man\#Gameplay

## 基本组件

- agent: 大嘴小怪物
- 环境:整个迷宫中的所有信息
- 奖励:agent每走一步,需要扣除1分,吃掉小球得10分，吃掉敌人得200分,被吃掉游戏结束。
- 动作:在每种状态下,agent能够采用的动作,比如上下左右移动。
- 策略:在每种状态下,采取最优的动作。
- 学习目标:获得最优的策略,以使累计奖励最大(即Score)。

[1]: https://aitechtogether.com/article/4681.html

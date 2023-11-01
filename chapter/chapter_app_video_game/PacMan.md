

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-20 14:33:32
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-11-02 00:09:10
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 吃豆人

主机游戏吃豆人( PacMan ，见图 1-2 )

街机游戏吃豆人(本图片改编自 https://en.wikipedia.org/wiki/Pac-Man\#Gameplay

## 基本规则

吃豆人(Pacman) ，相比Flappy Bird是个更复杂的例子。比如，Flappy Bird要躲避的障碍物都是静止的，Pacman的敌人是移动的，穷追不舍。

重点是，为了抓到 Pacman，四个敌人还有各自不同的运动规则:

- 红色鬼，直接瞄准Pacman的位置进发；
- 粉色鬼，瞄准Pacman前方的第四格；
- 蓝色鬼，利用Pacman和红色鬼的位置来搞伏击;
- 橙色鬼，原本和红色鬼一样，但当它和Pacman的距离近到8格以内，就会朝一个角落退缩，那是它出发的地方。

Pacman吃到无敌大豆豆的时候，敌人还会从追击模式转成逃跑模式。 [2]

## 基本组件

- agent: 大嘴小怪物
- 环境:整个迷宫中的所有信息
- 奖励:agent每走一步,需要扣除1分,吃掉小球得10分，吃掉敌人得200分,被吃掉游戏结束。
- 动作:在每种状态下,agent能够采用的动作,比如上下左右移动。
- 策略:在每种状态下,采取最优的动作。
- 学习目标:获得最优的策略,以使累计奖励最大(即Score)。


[1]: https://aitechtogether.com/article/4681.html
[2]: https://www.zhihu.com/question/31497611/answer/601785142

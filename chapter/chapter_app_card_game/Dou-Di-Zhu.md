

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-05-28 01:26:49
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-28 01:26:57
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 斗地主

在AI战胜德州扑克后，研究者将目光转移到了更复杂的扑克游戏上，如斗地主这类决策轮数更多，牌数更的游戏上面。由于博弈树变得更宽而且更深，而Libratus 和Pluribus 的基础算法CFR假设每一次的搜索都能够到结束节点，这在斗地主上面是不容易满足的。

最新的研究成果DeltaDou [36] 和DouZero [98] 都是基于强化学习算法，它只需要向下搜索一步或几步就可以进行更新，具有更强的适用性。其中DouZero 打败了之前的所有344 个AI 智能体，成为了当前最强的算法。[1]

## 斗地主介绍

斗地主是由中国人发明的纸牌游戏，该游戏道具由54张牌组成，包含两张joker牌，分别是彩色的大Joker（又称大王）与黑白的小Joker（又称小王），游戏人数为三人，一人为地主，另外两人为农民。三方出牌对战，地主先出牌，只要有一人出完牌，则该人获胜。该游戏起源于华中，目前于中国部分地区流行。斗地主可有第四名玩家参加，但需要用到两副扑克牌，而规则有所不同。

“斗地主”被描述为易学难精，需要战略思维以及精心策划。 花色牌与“斗地主”无关，因为斗地主是不计算花色的。 玩家可以轻松地使用一套“斗地主”扑克牌来玩游戏，而无需在意卡片上的印花色。 在中国确实存在不太流行的游戏变体，例如四人和五人“斗地主”玩两副牌。[1]

[1]: https://personal.ntu.edu.sg/boan/Chinese/%E5%88%86%E5%B8%83%E5%BC%8F%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E7%AE%80%E4%BB%8B.pdf
[2]: https://zh.wikipedia.org/zh-sg/%E9%AC%A5%E5%9C%B0%E4%B8%BB

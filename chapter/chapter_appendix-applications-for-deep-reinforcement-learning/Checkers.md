

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-16 21:03:36
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-16 21:05:04
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 西洋跳棋



该网络分为两部分，一部分用于s'，另一部分用于s''，并通过比较两部分的输出来选择更好的网络。以这样的方式，每一部分都将学习到一个评价函数[插图]。Neurogammon在1989年举行的计算机奥林匹克竞赛中夺冠，这是有史以来第一个赢得计算机游戏锦标赛的学习程序，但它从未超越特索罗自己发挥中等时的水平。由特索罗开发的另一个系统TD-Gammon(Tesauro, 1992)采用了里奇·萨顿(Rich Sutton)最近出版的书中的TD学习方法——本质上类似于由塞缪尔发展的探索方法，但该方法对如何正确地操作有了更深入的技术层面的理解。其评价函数是一个带有包含80个节点的隐藏层的全连接神经网络。（它还借鉴了一些Neurogammon所使用的人工设计的输入特征。）经过30万局游戏的训练后，它达到了与世界排名前三的人类玩家相当的水平。世界排名前十的玩家基特·伍尔西(Kit Woolsey)说道：“毫无疑问，在我看来，它对局面的判断远比我优秀。”

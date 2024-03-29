

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-16 21:03:36
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-11 22:34:45
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 西洋跳棋

由《Science》杂志评选的 2007年十大科技突破中，就还包括了加拿大阿尔波特大学的科研成果——解决了西洋跳棋(Checker)博弈问题[3]，也就是说，在西洋跳棋的博弈中计算机将永远“立于不败之地”。[4]

## 状态复杂度的定义

博弈过程的局面称之为状态，博弈问题的状态复杂度是指从初始局面出发，产生的所有合法局面的总和。然而，精确计算博弈问题（比如：国际象棋、围棋等）的状态复杂度几乎是不可能的[38]。一般以该棋类可能的局面总数的上限值为标准。它为通过完全列举求解博弈问题的复杂度提供了一个边界值。

## 状态复杂度

8×8 的西洋跳棋（Checkers）于2007 年得到了理论解[39]，证明过程中，采用了三种方法：证据计数法，残局阶段采用了数据库，通过两个程序实现对节点的估值。不仅证明了一种不败的策略，而且计算了8×8 的西洋跳棋可能会产生500,995,484,682,338,672,639（约5×1020）个合法局面。可见，只有得到了理论解的博弈问题，才能比较精确地计算其状态复杂度。[1]

## 软件

约瑟夫塞·缪尔开发出下西洋跳棋（见图14.10（b））的软件，是第一款应用机器学习算法的程序，现在
这个算法被人们称为强化学习。在早期的游戏中，AI都集中在解决经典棋类游戏的问题上，人们相信人类挑战了几百年甚至上千年的游戏，必定是人类智能的精华所在。然后，三十年的努力，人们在树搜索技术上取得突破。1994年，乔纳森·斯卡费尔的西洋跳棋程序Chinook打败了人类冠军马里恩·汀斯雷[54]；2007年，他在《科学》杂志宣布“Checkers is solved”（西洋跳棋已被攻克）[55]。[2]




该网络分为两部分，一部分用于s'，另一部分用于s''，并通过比较两部分的输出来选择更好的网络。以这样的方式，每一部分都将学习到一个评价函数[插图]。Neurogammon在1989年举行的计算机奥林匹克竞赛中夺冠，这是有史以来第一个赢得计算机游戏锦标赛的学习程序，但它从未超越特索罗自己发挥中等时的水平。由特索罗开发的另一个系统TD-Gammon(Tesauro, 1992)采用了里奇·萨顿(Rich Sutton)最近出版的书中的TD学习方法——本质上类似于由塞缪尔发展的探索方法，但该方法对如何正确地操作有了更深入的技术层面的理解。其评价函数是一个带有包含80个节点的隐藏层的全连接神经网络。（它还借鉴了一些Neurogammon所使用的人工设计的输入特征。）经过30万局游戏的训练后，它达到了与世界排名前三的人类玩家相当的水平。世界排名前十的玩家基特·伍尔西(Kit Woolsey)说道：“毫无疑问，在我看来，它对局面的判断远比我优秀。”

1956 年 Samuel 利用第一台商用计算机 IBM 701 编写了跳棋（checkers）走子程序，并在 1959年发表论文总结了该程序的设计思想和原理[19].  该跳棋走子程序使用了min-max 搜索.[3]

最早在人机对抗研究中引入学习的是Samuel，他 1957 年完成的跳棋走子程序不仅使用了min-max 搜索，同时也引入了两种―学习‖机制[19]：死记硬背式学习（rote learning ）和泛化式学习（learning by generalization）.  前者通过存储之前下
棋过程中计算得到的局面得分来减少不必要的搜索，后者则**根据下棋的不同结果来更新评估函数中不同参数的系数**来得到一个更好的评估函数.  此外，该论文也第一次提到了自我对局（self-play）.  此后，这种通过学习来提升机器能力的思想就一直没
能引起重视.[3]

[1]: https://www.ambchina.com/data/upload/image/20220226/2017%E4%B8%AD%E5%9B%BD%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E7%B3%BB%E5%88%97%E7%99%BD%E7%9A%AE%E4%B9%A6--%E6%99%BA%E8%83%BD%E5%8D%9A%E5%BC%88-2017.pdf
[2]: https://pdf-1307664364.cos.ap-chengdu.myqcloud.com/%E6%95%99%E6%9D%90/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/%E3%80%8A%E7%99%BE%E9%9D%A2%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AE%97%E6%B3%95%E5%B7%A5%E7%A8%8B%E5%B8%88%E5%B8%A6%E4%BD%A0%E5%8E%BB%E9%9D%A2%E8%AF%95%E3%80%8B%E4%B8%AD%E6%96%87PDF.pdf
[3]: http://cjc.ict.ac.cn/online/onlinepaper/zl-202297212302.pdf
[4]: http://computergames.caai.cn/download/%E8%AE%A1%E7%AE%97%E6%9C%BA%E5%8D%9A%E5%BC%88%E5%8E%9F%E7%90%86%E4%B8%8E%E6%96%B9%E6%B3%95%E5%AD%A6%E6%A6%82%E8%BF%B0.pdf

[39]: Allis V. Searching for Solutions in Games and Artificial Intelligence[J]. Hosp Pract, 1994, 25(6).
[40]: Jonathan S, Neil B, Yngvi B, et al. Checkers is solved[J]. Science,
2007, 317(5844):1518-22

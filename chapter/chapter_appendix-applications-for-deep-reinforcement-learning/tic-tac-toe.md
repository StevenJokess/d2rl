

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-05-12 01:44:22
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-12 02:20:49
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# tic-tac-toe

以tic-tac-toe（三子连珠棋）为例，估算了此博弈问
题的状态复杂度和博弈树复杂度。tic-tac-toe 共有9 个位置可以落子，能够形成的局面较少，因此其复杂度的估算相对容易。

## 具体估算过程如下：

具体估算过程如下：

（1）对于其状态复杂度，由于棋盘上每个位置有三种状态（双方的棋子和空白），因此，状态复杂度可估算为39，根据此博弈问题的走棋规则，在棋盘上形成连3则游戏结束，出现两个以上的连3 的局面属于非法局面。而对称相同的多个局面应该只算作一个局面。将这些考虑在内，则更精确的状态复杂度为5478；
（2）对于其博弈树复杂度，平均深度约为9，第i（1≤  i  ≤9）层时，走棋方可能的走法有9-i 个，因此，此博弈树的叶子节点数（即博弈树复杂度）为9！。[1]

## 第一款成功下棋的软件

第一款成功下棋的软件诞生于1952年，记录在道格拉斯的博士论文中，玩的是最简单的Tic-Tac-Toe游戏。[2]

[1]: https://www.ambchina.com/data/upload/image/20220226/2017%E4%B8%AD%E5%9B%BD%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E7%B3%BB%E5%88%97%E7%99%BD%E7%9A%AE%E4%B9%A6--%E6%99%BA%E8%83%BD%E5%8D%9A%E5%BC%88-2017.pdf
[2]: https://pdf-1307664364.cos.ap-chengdu.myqcloud.com/%E6%95%99%E6%9D%90/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/%E3%80%8A%E7%99%BE%E9%9D%A2%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AE%97%E6%B3%95%E5%B7%A5%E7%A8%8B%E5%B8%88%E5%B8%A6%E4%BD%A0%E5%8E%BB%E9%9D%A2%E8%AF%95%E3%80%8B%E4%B8%AD%E6%96%87PDF.pdf

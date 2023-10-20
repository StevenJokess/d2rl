

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-21 02:48:01
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-21 02:48:41
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# MountainCar-v0

强化学习经典控制问题 30 年后惊现闭式解
MountainCar-v0 是 Andrew Moore 在 1990 年提出的控制问题，提出后受到强化学习界的广泛关注，并收录到强化学习影响力最大的环境库之一 Gym 中，成为大多数强化学习教程中必用的环境。

图1 经典控制问题MountainCar-v0（图片来源：https://gym.openai.com/envs/MountainCar-v0/）

2019 年，OpenAI Gym Leaderboard 上出现了对该环境求解的突破性进展：上面显示了一个仅用很少的回合就可以完成学习的强化学习代码，又给出了一个不需要学习的闭式解。

更令人惊奇的是，这个闭式解只依赖于一个不等式，并且这个不等式只用到四次多项式。他在满足这个不等式时采用一种动作，不满足时采用另外一种动作，这样竟然就能解决这个问题。

这两种解法目前位于 OpenAI Gym 的排行榜的前两名。

第一名就是用不等式的闭式解，第二名则用了强化学习中的资格迹算法。

第二名解法用了75个回合数据就解决了问题，数据利用率是排名第三的算法（用了341个回合数据）的4.5倍。

图2 MountainCar-v0排行榜页面（图片来源：https://github.com/openai/gym/wiki/Leaderboard）

求解的代码已经放在 GitHub 上，可以通过排行榜上的链接进入。[1]

[1]: https://blog.csdn.net/zhiqingxiao/article/details/102870708

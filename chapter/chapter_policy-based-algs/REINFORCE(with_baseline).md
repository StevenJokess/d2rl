

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-05-23 23:55:42
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-23 23:56:20
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# REINFORCE (with baseline)

## 背景

REINFORCE算法使用了MC方法，因而具有了一个明显的缺点，那就是方差太大（后面的 R_{k} 从t时刻到n时刻都是随机变量，随机变量维度高，导致的方差大[2]）。

baseline则是一种可以减小其方差的办法。具体做法就是为动作价值函数找到一个基线与之对比。

\nabla J(\theta)\propto\sum_s \mu(s)\sum_a (q_\pi(s,a)-b(s))\nabla\pi(a|s,\theta)

只要基线与动作无关(即与 \theta 无关)，便可以保证上式成立,因为减的项为零。

\sum_a b(s)\nabla\pi(a|s,\theta)=b(s)\nabla\sum_a \pi(a|s,\theta)=b(s)\nabla1 = 0

合理的 b(s) 可以有效的减小使用MC方法估计的梯度的方差。

到这里，相信大家都可以看出 q_\pi(s,a)-b(s) 这一项实际上就是advantage的雏形了。

Advantege怎么计算：

早先的时候，有人把样本中的平均收益作为baseline使用，这样也能起到一定的效果。然而对于马尔可夫过程来说，baseline应该根据状态变化的，对于所有动作价值都大的状态baseline应该较大，反之亦然。

一个自然能想到baseline便是状态价值函数 v(s) ，实际上在A2C,A3C等算法中，正是使用了q_\pi(s,a)-v(s)作为advantage，也取得了很好的效果。（顺带一提Dueling DQN中的也是专门有一个网络输出来估计这个advantage）。

然而，伯克利的大神们结合了 TD(\lambda) 的思想，提出了估计更加平滑，方差更为可控的advantage:GAE(General Advantage Estimation) .

[1]: https://zhuanlan.zhihu.com/p/343943792
[2]: https://zhuanlan.zhihu.com/p/437626120



<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-14 04:02:05
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-14 04:03:33
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# RL算法考量

样本利用效率（Sample Efficiency）：![](../../img/Sample_Efficiency.png)

算法稳定性与易用性（Stability and ease of use）

1. 算法收敛吗？如果收敛，收敛到哪个点？
1. 该算法每次都收敛吗？会不会换个随机种子就不收敛了？（呜呜呜

该算法是on-policy还是off-policy的？还是两者都可以？

算法的参数：

1. 算法适用的state 与 action 是连续(continuous)还是离散的discrete？
1. 算法的轨迹(trajectory)是有限长(Episodic)还是无限长的(infinite horizon)？
1. 算法得到的策略policy是确定的(deterministic)还是随机的(stochastic)？

实际问题的考量：
1. 实际问题中，policy表示起来容易吗？方便吗？
1. 实际问题中，transition model表示起来容易吗？方便吗？

[1]: https://blog.csdn.net/weixin_40056577/article/details/104109073



<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-11 20:04:31
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-11 20:06:03
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# VDN (Value Decomposition Networks)

VDN (value decomposition networks) 也是采用对每个智能体的值函数进行整合，得到一个联合动作值函数。VDN假设中心式的Qtot可以分解为各个Qa的线性相加。 令 $\tau=\left(\tau_1, \ldots, \tau_{\mathrm{n}}\right)$ 表示联合动作-观测历史，其中 $\tau_{\mathrm{i}}=\left(\mathrm{a}_{\mathrm{i}, 0}, \mathrm{o}_{\mathrm{i}, 0}, \ldots, \mathrm{a}_{\mathrm{i}, \mathrm{t}-1}, \mathrm{o}_{\mathrm{i}, \mathrm{t}-1}\right)$ 为动作-观测历史， $\mathrm{a}=\left(\mathrm{a}_1, \ldots, \mathrm{a}_{\mathrm{n}}\right)$ 表示联合动作。

$\mathrm{Q}_{\mathrm{tot}}$ 为联合动作值函数， $\mathrm{Q}\left(\tau_{\mathrm{i}}, \mathrm{a}_{\mathrm{i}} ; \theta_{\mathrm{i}}\right)$ 为智能体的局部动作值函数，局部值函数只依赖于每个智能体的局部观测。

VDN采用的方法就是直接相加求和的方式
$\mathrm{Q}_{\mathrm{tot}}=\sum_{\mathrm{i}=1}^{\mathrm{n}} \mathrm{Q}\left(\tau_{\mathrm{i}}, \mathrm{a}_{\mathrm{i}} ; \theta_{\mathrm{i}}\right)$

虽然 $\mathrm{Q}\left(\tau_{\mathrm{i}}, \mathrm{a}_{\mathrm{i}} ; \theta_{\mathrm{i}}\right)$ 不是用来估计累积期望回报的，但是这里依然叫它为值函数。

分布式的策略可以通过对每个 $Q\left(\tau_i, a_i ; \theta_i\right)$ 取max得到。

$V D N$ 假设中心式的 $\mathrm{Q}_{\mathrm{tot}}$ 可以分解为各个 $\mathrm{Q}_{\mathrm{a}}$ 的线性相加，而 $\mathrm{QMIX}$ 可以视为 $V D N$ 的拓展[1]


[1]: https://blog.csdn.net/wzduang/article/details/115874734?spm=1001.2014.3001.5502

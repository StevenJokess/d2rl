

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-06 21:46:51
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-06 22:03:04
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 分位数回归算法

## 带效用的分位数回归算法

我们可以把VNM效用或Yarri效用应用在分位数回归算法（包括分位数回归深度 $Q$ 网络算法和含蓄分位网络算法) 中。
应用VNM效用的方法和12.3.2节中介绍的方法类似，就是在决策时考虑效用函数 的期望。具体而言，把效用函数 $u$ 应用在分位函数网络的输出 $\phi(\cdot)$ 上，得到 $u(\phi(\cdot))$ ， 并据此估计期望来决定使用哪个动作。在算法 12-3和算法12-4中，需要修改 2.2.1.1步和2.2.2.2步。
下面介绍应用Yarri效用的方法。采用Yarri效用时，也是要确定带扭曲函数的动作价 值期望值 $q\langle\beta\rangle(s, a)$ 作出决策，即:
$$
\pi^{\langle\beta\rangle}(s)=\underset{a}{\operatorname{argmax}} q^{\langle\beta\rangle}(s, a) 。
$$
由于它也只是对期望进行了修改，所以也只需要修改算法12-3和算法12-4中的 2.2.1.1步和2.2.2.2步即可。
分位数回归深度 $Q$ 网络算法可以采用分段近似来计算扭曲期望:
$$
\begin{aligned}
\mathrm{E}^{\langle\beta\rangle}[X] & =\int_0^1 \phi_X(\omega) \mathrm{d} \beta^{-1}(\omega) \\
& =\sum_{i=0}^{|\mathcal{I}|-1} \int_{i /|\mathcal{I}|}^{(i+1) /|\mathcal{I}|} \phi_X(\omega) \mathrm{d} \beta^{-1}(\omega) \\
& \approx \sum_{i=0}^{|\mathcal{I}|-1} \phi_X\left(\frac{i+0.5}{|\mathcal{I}|}\right)\left[\beta^{-1}\left(\frac{i+1}{|\mathcal{I}|}\right)-\beta^{-1}\left(\frac{i}{|\mathcal{I}|}\right)\right] 。
\end{aligned}
$$


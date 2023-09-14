

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-04 15:59:41
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-04 16:14:00
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Hoeffding霍夫丁不等式

机器学习中，算法的泛化能力往往是通过研究泛化误差的概率上界所进行的，这个就称为泛化误差上界。直观的说，在有限的训练数据中得到的规律，则认为真实的总体数据中也是近似这个规律的。比如一个大罐子里装满了红球和白球，各一半，我随手抓了一把，然后根据这些红球白球的比例预测整个罐子也是这样的比例，这样做不一定很准确，但结果总是近似的，而且如果抓出的球越多，预测结果也就越可信。

对于两种不同的学习方法，通常比较他们的误差上界来决定他们的优劣。hoeffding不等式于1963年被Wassily Hoeffding提出并证明，用于计算随机变量的和与其期望值偏差的概率上限。下面我们理清hoeffding 不等式的来龙去脉。

## 1.伯努利随机变量的特例

我们假定一个硬币 $\mathrm{A}$ 面朝上的概率为 $p$ ，则 $\mathrm{B}$ 面朝上的概率为 $1-p$ 。 抛 $\mathrm{n}$ 次硬币， $\mathrm{A}$ 面朝上次数的期望 值为 $n * p$ 。则 $\mathrm{A}$ 面朝上的次数不超过 $\mathrm{k}$ 次的概率为:

$$
P(H(n) \leq k)=\sum_{i=0}^k C_n^i p^i(1-p)^{n-i}=\sum_{i=0}^k \frac{n !}{i !(n-i) !} p^i(1-p)^{n-i}
$$

其中 $H(n)$ 为抛 $\mathrm{n}$ 次硬币 $\mathrm{A}$ 面朝上的次数。

对某一 $\varepsilon>0$ 当 $k=(p-\varepsilon) n$ 时，有Hoeffding不等式

$$
P(H(n) \leq(p-\varepsilon) n) \leq e^{-2 \varepsilon^2 n}
$$

对应的，当 $k=(p+\varepsilon) n$ 时，

$$
P(H(n) \geq(p+\varepsilon) n) \leq e^{-2 \varepsilon^2 n}
$$

由此我们可以推导出

$$
P((p-\varepsilon) n \leq H(n) \leq(p+\varepsilon) n) \geq 1-2 e^{-2 \varepsilon^2 n}
$$

特别的，当 $\varepsilon=\sqrt{\frac{\ln n}{n}}$ 时，

$$
P(|H(n)-p n| \leq \sqrt{n \ln n}) \geq 1-2 e^{-2 \ln n}=1-\frac{2}{n^2}
$$

## 伯努利随机变量的一般情况

令独立同分布随机变量 $X_1, X_2, \ldots, X_n$ ，其中 $X_i \in\left[a_i, b_i\right]$ ，则这些变量的经验均值为:

$\bar{X}=\frac{X_1+X_2+, \ldots,+X_n}{n}$ 对于任意 $t>0$ 有

$P(|\bar{X}-E(\bar{X})| \geq t) \leq 2 e^{-\frac{2 n^2 t^2}{\sum_{i=1}^n\left(b_i-a_i\right)^2}}$

或 $S_n=X_1+X_2+, \ldots,+X_n$

$P\left(\left|S_n-E\left(S_n\right)\right| \geq t\right) \leq 2 e^{\left.-\frac{2 t^2}{\sum_{i=1}^n\left(b_i-a_i\right)^2}\right.}$

证明如下:

霍夫丁引理: 假设 $X$ 为均值为 0 的随机变量且满足 $P(X \in[a, b])=1$, 有以下不等式成 立: $E\left(e^{s X}\right) \leq e^{\left.\frac{s^2(b-a)^2}{8}}$ 则对于独立随机变量 $X_1, X_2, \ldots, X_n$ 满足 $P\left(X_i \in\left[a_i, b_i\right]\right)=1$ ，对于 $t>0$ :

$$
\begin{aligned}
& P\left(S_n-E\left(S_n\right) \geq t\right)=P\left(e^{s\left(S_n-E\left(S_n\right)\right)} \geq e^{s t}\right) \\
& \leq e^{-s t} E\left(e^{s\left(S_n-E\left(S_n\right)\right)}\right) \\
& =e^{-s t} \prod_{i=1}^n E\left(e^{s\left(X_i-E\left(X_i\right)\right)}\right) \\
& \leq e^{-s t} \prod_{i=1}^n E\left(e^{\frac{s^2\left(b_i-a_i\right.}{8}}\right)^2 \\
& =\exp \left(-s t+0.125 s^2 \sum_{i=1}^n\left(b_i-a_i\right)^2\right) \\
& \text { 令 } g(s)=-s t+0.125 s^2 \sum_{i=1}^n\left(b_i-a_i\right)^2 \text { ，则 } g(s) \text { 为二次函数，当 } s=\frac{4 t}{\sum_{i=1}^n\left(b_i-a_i\right)^2} \text { 时函数获得最 }
\end{aligned}
$$
小值。因此: $P\left(S_n-E\left(S_n\right) \geq t\right) \leq e^{\left.-\frac{2 t^2}{\sum_{i=1}^n\left(b_i-a_i\right.}\right)^2}$

[1]: https://xhhszc.github.io/2018/04/17/Hoeffding%E9%9C%8D%E5%A4%AB%E4%B8%81%E4%B8%8D%E7%AD%89%E5%BC%8F%E5%8F%8A%E5%85%B6%E5%9C%A8%E9%9B%86%E6%88%90%E5%AD%A6%E4%B9%A0%E7%90%86%E8%AE%BA%E7%9A%84%E5%BA%94%E7%94%A8/

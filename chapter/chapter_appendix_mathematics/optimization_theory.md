

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-02 17:02:47
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-21 21:13:42
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 最优化理论（Optimization theory）


## 凸优化

凸集与凸函数/最优化问题/梯度下降法/牛顿法/带约束优化问题

### 凸集

测试一个集合是不是凸的，只要任意取集合中的俩个点并连线，如果说连线段完全被包含在此集合中，那幺这个集合就是凸集。[2]

## 凸函数

若函数 $\mathrm{f}$ 二阶可微, 则函数为凸函数。

当前仅当dom为凸集, 且

$$
\nabla^2 f(x) \succ=0
$$

- 若f是一元函数, 上式表示二阶导大于等于 0
- 若f是多元函数, 上式表示二阶导Hessian矩阵半正定。

### 举例

- 指数函数 $f(x)=e^{a x}$
- 幂函数 $f(x)=x^a, x \in R^{+}, a \geq 1$ 或 $a \leq 0$
- 负对数函数 $f(x)=-\ln x$
- 负狡函数 $f(x)=x \ln x$
- 范数函数 $f(\vec{x})=\| x \mid$
- 最大值函数 $f(\vec{x})=\max \left(x_1, x_2, \cdots x_{r_{\mathrm{i}}}\right)$
- 指数线性函数 $f(\vec{x})=\log \left(e^{e_1}+e^{r_2}+\cdots+e^{x_n}\right)$
  - $\log$ Sum $\operatorname{Exp} \rightarrow \max (x 1, x 2 \ldots x n)$

### Jensen不等式

基本Jensen不等式

$$
f(\theta x+(1-\theta) y) \leq \theta f(x)+(1-\theta) f(y)
$$

若 $\theta_1, \ldots, \theta_k \geq 0, \theta_1+\cdots+\theta_k=1$
则 $f\left(\theta_1 x_1+\cdots+\theta_k x_k\right) \leq \theta_1 f\left(x_1\right)+\cdots+\theta_k f\left(x_k\right)$

若 $p(x) \geq 0$ on $S \subseteq \operatorname{dom} f: \int_S p(x) d x=1$
则 $f\left(\int_S p(x) x d x\right) \leq \int_S f(x) p(x) d x \quad f(\mathbf{E} x) \leq \mathbf{E} f(x)$

### 优化算法

TODO:[3]

## 非凸优化

TODO:
https://www.cnblogs.com/orion-orion/p/15418056.html

[1]: https://www.bilibili.com/video/BV1UV411z7um/?spm_id_from=333.999.0.0&vd_source=bca0a3605754a98491958094024e5fe3
[2]: https://flashgene.com/archives/23923.html
[3]: https://datawhalechina.github.io/unusual-deep-learning/#/08.%E4%BC%98%E5%8C%96%E7%AE%97%E6%B3%95

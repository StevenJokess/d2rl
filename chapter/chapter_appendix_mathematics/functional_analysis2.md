

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-08-25 03:35:00
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-08-25 03:42:21
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
#

https://zhuanlan.zhihu.com/p/39279611#%E5%A6%82%E4%BD%95%E8%AF%81%E6%98%8E%E8%BF%AD%E4%BB%A3%E5%BC%8F%E7%AD%96%E7%95%A5%E8%AF%84%E4%BB%B7%E3%80%81%E5%80%BC%E8%BF%AD%E4%BB%A3%E5%92%8C%E7%AD%96%E7%95%A5%E8%BF%AD%E4%BB%A3%E7%9A%84%E6%94%B6%E6%95%9B%E6%80%A7%EF%BC%9F

在Sutton的动态规划一节中，提到了三种算法：

迭代式策略评价：用来估计给定策略下的值函数[Math Processing Error]$v_\pi$
策略迭代：求解最优策略 [Math Processing Error]$\pi_*$
值迭代：求解最优策略 [Math Processing Error]$\pi_*$

书中提到了这三种迭代算法的收敛性，但是确没有给出证明。正所谓知其然，也要知其所以然。我们就来探究一下其收敛性的证明。其中收敛性主要包括三个：

- 是否收敛？
- 解是否唯一？
- 以什么速度收敛？


## 压缩映射定理

压缩映射定理
整个证明过程需要用到contracting mapping定理，所以首先我们来讲一下这个定理

Contracting mapping theorem
对于完备的metric space \langle M, d\rangle ，如果 f: M\mapsto M 是它的一个压缩映射，那么
* 在该metric space中，存在唯一的点 x_* 满足 f(x_*) = x_* 。
* 并且，对于任意的 x\in M , 定义序列 f^2(x) = f(f(x)) , f^3(x) = f(f^2(x)) , \cdots, f^n(x)=f(f^{n-1}(x)), 该序列会收敛于 x_* 即 \lim_{n\rightarrow \infty} f^n(x) = x_*


突然看到这个定理是不是一下子有点懵逼，别急，我们来对它慢慢剖析。

Metric Space
wiki

非正式定义 ：如果一个集合中所有的点与点之间的距离都被定义了，那么这个集合被称为metric space。这些距离被称为该space的一个metric。

本质上metric其实是广义的欧式距离。欧式距离就是我们平时所说的直线距离。而最常见的metric space就是3D欧式空间（我们高中学立体几何时的xyz坐标轴描述的空间）

metric的概念比欧式距离更加丰富。比如，我们可以把每个人的心看成一个点，然后定义所有的心与心的距离（抖个机灵）。此时，所有的心就构成了一个metric space, 心与心的距离就是该space的metric。

好了，下面来一个正式版的定义吧。


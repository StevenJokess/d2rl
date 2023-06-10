

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-07 13:53:47
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-07 14:12:00
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# 扩展形式博弈

在标准形式博弈中，所有个体只决策一次，但现实中一些博弈是需要进行多次决策之后才能知道博弈结果的，比如围棋、德州扑克等，对于这样有明显先后顺序的博弈（又称为回合制博弈）可以用扩展形式博弈来描述：


$\chi: H \rightarrow 2^{|A|}, \chi(h)$ 表示在博弈历史为 $h$ 时个体的合法 动作集合; $\mathcal{R}_i: Z \rightarrow \mathbb{R}$ 表示个体 $i$ 在博恋结束时的收 益; $I_i=\{h \in H \mid \rho(h)=i\}$ 是个体 $i$ 所有需要做决策 的节点集合, 如果 $I_{i 1}, I_{i 2}, \ldots, I_{i k_i}$ 是 $I_i$ 的一个划分, 且 满足对于任意的 $j \in\left\{1,2 \ldots, k_i\right\}$, 对于任意的节点 $h, h^{\prime} \in I_{i j}, \rho(h)=\rho\left(h^{\prime}\right)$ 并且 $\chi(h)=\chi\left(h^{\prime}\right)$, 则称 $I_{i j}$ 是 个体 $i$ 的一个信息集.

在以上定义中, 如果所有个体的所有信息集都为单点集, 则称博弈为完美信息博弈, 此时每个个体在做决策时都知道当前完整的博弈历史; 否则, 博弈就称为不完美信息博弈。

特别地, 当 $n=2$ 且对 于任意的博弈结束节点 $z \in Z$ 都有 $\mathcal{R}_1(z)+\mathcal{R}_2(z)= 0$ 时, 称为二人零和扩展形式博弈。

对于扩展形式博弈, 以下定义都假设完美回忆 (perfect recall), 也就是到达任意一个信息集时, 任何参与博弈的个体都**记得他之前的所有经历**（包括执行的动作和到达的信息集）. 完美回忆是在研究扩展形式博弈时一个较为普遍的假设, 在这个假设下, 根据 Kuhn 定理, 以下策略的定义具有一般性：


http://cjc.ict.ac.cn/online/onlinepaper/zl-202297212302.pdf



<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-07 23:22:59
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-21 20:16:00
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

# LQR（Linear Quadratic Regulator）

现有一些随机策略 $\mathrm{p}_\theta\left(\mathrm{u}_{\mathrm{t}} \mid \mathrm{x}_{\mathrm{t}}\right)$ 收集的样本:

$$
\tau^{\mathrm{i}}=\left(\mathrm{x}_1^{\mathrm{i}}, \mathrm{u}_1^{\mathrm{i}}, \mathrm{x}_2^{\mathrm{i}}, \mathrm{u}_2^{\mathrm{i}}, \ldots, \mathrm{x}_{\mathrm{T}}^{\mathrm{i}}, \mathrm{u}_{\mathrm{T}}^{\mathrm{i}}, \mathrm{x}_{\mathrm{T}+1}^{\mathrm{i}}\right), \mathrm{i}=1,2 \ldots, \mathrm{N}
$$

$\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}$ 即为第 $t$ 诗刻的状态与动作，此为控制论符号的表述 $\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)=\left(\mathrm{s}_{\mathrm{t}}, \mathrm{a}_{\mathrm{t}}\right)$ ，两者可进行混用。lec1-lec4中提到，NN的cost function $\mathrm{c}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)$ 实际上是一种immediate的监督信号，RL的reward function $\mathrm{r}\left(\mathrm{s}_{\mathrm{t}}, \mathrm{a}_{\mathrm{t}}\right)$ 实际上是一种delayed的监督信息，因此在一个time step下，有 $\mathrm{c}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)=-\mathrm{r}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)+$ constant。下面给出一个术语表格，基础知识主要使用控制领域的术语表述。

|  | Control | RL |
| :---: | :---: | :---: |
| 状态state | $\mathrm{x}_{\mathrm{t}}$ | $\mathrm{s}_{\mathrm{t}}$ |
| 动作action | $\mathrm{u}_{\mathrm{t}}$ | $\mathrm{a}_{\mathrm{t}}$ |
| 监督信号 | $\mathrm{c}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)$ | $\mathrm{r}\left(\mathrm{s}_{\mathrm{t}}, \mathrm{a}_{\mathrm{t}}\right)$ |
| 动态模型 | $\mathrm{x}_{\mathrm{t}+1} \sim \mathrm{f}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)$ | $\mathrm{s}^{\prime} \sim \mathrm{p}\left(\mathrm{s}^{\prime} \mid \mathrm{s}, \mathrm{a}\right)$ |


## LQR问题下的设定
1. 动态模型是deterministic的，即 $\mathrm{x}_{\mathrm{t}+1}=\mathrm{f}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right), \mathrm{f}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)$ 不是一个概率分布的模型。 消耗的代价，其中函数 $g$ 为标量。因此 $c\left(x_t, u_t\right)$ 即为performace measure中的函数 $g$ 。LQR的监督信号 $c\left(x_t, u_t\right)$ 是已知的，输入 $x_t, u_t$ ，输出一个标量 $s c a l a r$ 。
3. LQR 的目的是，给定一个初始状态 $x_1$ ，终止状态或任务目标状态 $x_{T+1}$ ，已知环境动态模型 $x_{t+1}=f\left(x_t, u_t\right)$ ，求出一串动作序列 $u_1, u_2, \ldots, u_T$ 使得累积cost最小，即
4.
$$
\min _{\mathrm{u} 1, \ldots, \mathrm{uT}_{\mathrm{T}}} \sum_{\mathrm{t}=1}^{\mathrm{T}} \mathrm{c}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right) \quad \text { s.t } \quad \mathrm{x}_{\mathrm{t}}=\mathrm{f}\left(\mathrm{x}_{\mathrm{t}-1}, \mathrm{u}_{\mathrm{t}-1}\right)
$$

1. LQR的approximation， linear体现在对 $f\left(x_t, u_t\right)$ 采用一阶线性近似，quadratic体现在对 $c\left(x_t, u_t\right)$ 采用二阶近似，即

$$
\begin{gathered}
\mathrm{x}_{\mathrm{t}+1}=\mathrm{f}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right) \approx \mathrm{F}_{\mathrm{t}}\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]+\mathrm{f}_{\mathrm{t}}=\left[\begin{array}{ll}
\mathrm{F}_{\mathrm{x}_{\mathrm{t}}} & \mathrm{F}_{\mathrm{u}_{\mathrm{t}}}
\end{array}\right]\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]+\mathrm{f}_{\mathrm{t}} \\
\mathrm{c}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)=\frac{1}{2}\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]^{\mathrm{T}}\left[\begin{array}{ll}
\mathrm{C}_{\mathrm{x}_{\mathrm{t}}, \mathrm{x}_{\mathrm{t}}} & \mathrm{C}_{\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}} \\
\mathrm{C}_{\mathrm{u}_{\mathrm{t}}, \mathrm{x}_{\mathrm{t}}} & \mathrm{C}_{\mathrm{u}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}}
\end{array}\right]\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]+\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]^{\mathrm{T}}\left[\begin{array}{l}
\mathrm{c}_{\mathrm{x}_{\mathrm{t}}} \\
\mathrm{c}_{\mathrm{u}_{\mathrm{t}}}
\end{array}\right]=\frac{1}{2}\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]^{\mathrm{T}} \mathrm{C}_{\mathrm{t}}\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]+\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]^{\mathrm{T}} \mathrm{c}_{\mathrm{t}}
\end{gathered}
$$

所以LQR的优化问题表述为:

$$
\begin{gathered}
\min _{\mathrm{u} 1, \ldots, \mathrm{uT}_{\mathrm{T}}} \sum_{\mathrm{t}=1}^{\mathrm{T}} \mathrm{c}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right) \quad \text { s.t } \quad \mathrm{x}_{\mathrm{t}}=\mathrm{f}\left(\mathrm{x}_{\mathrm{t}-1}, \mathrm{u}_{\mathrm{t}-1}\right) \\
\mathrm{f}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)=\mathrm{F}_{\mathrm{t}}\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]+\mathrm{f}_{\mathrm{t}} \\
\mathrm{c}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)=\frac{1}{2}\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]^{\mathrm{T}} \mathrm{C}_{\mathrm{t}}\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]+\left[\begin{array}{l}
\mathrm{x}_{\mathrm{t}} \\
\mathrm{u}_{\mathrm{t}}
\end{array}\right]^{\mathrm{T}} \mathrm{c}_{\mathrm{t}}
\end{gathered}
$$
其中 $\mathrm{F}_{\mathrm{t}}, \mathrm{f}_{\mathrm{t}}, \mathrm{C}_{\mathrm{t}}, \mathrm{c}_{\mathrm{t}}$ 均已知，下面推到用到其展开形式!

LQR求解主要有Backward Pass与Forward Pass两大过程，先看看对于LQR这个问题，什么是已知的，什么是未知的。
- 已知: initial state 初始状态 $\mathrm{x}_1$, goal state目标状态 $\mathrm{x}_{\mathrm{T}+1}$, 动态模型 $\mathrm{f}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)$ 与 $\operatorname{cost}$ function $\mathrm{c}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)$ 的结构参数 $\mathrm{F}_{\mathrm{t}}, \mathrm{f}_{\mathrm{t}}, \mathrm{C}_{\mathrm{t}}, \mathrm{c}_{\mathrm{t}}$
- 未知: $\mathrm{x}_2, \mathrm{x}_3, \ldots, \mathrm{x}_{\mathrm{T}}, \mathrm{u}_1, \mathrm{u}_2, \ldots, \mathrm{u}_{\mathrm{T}}$ 。因为 $\mathrm{x}_2=\mathrm{f}\left(\mathrm{x}_1, \mathrm{u}_1\right), \mathrm{x}_3=\mathrm{f}\left(\mathrm{x}_2, \mathrm{u}_2\right), \ldots, \mathrm{x}_{\mathrm{T}}=\mathrm{f}\left(\mathrm{x}_{\mathrm{T}-1}, \mathrm{u}_{\mathrm{T}-1}\right), \mathrm{x}_{\mathrm{T}+1}=\mathrm{f}\left(\mathrm{x}_{\mathrm{T}}, \mathrm{u}_{\mathrm{T}}\right)$ ，所以实际上未知的就是动作序列 $\mathrm{u}_1, \ldots, \mathrm{u}_{\mathrm{T}}$ 可以看成:
$$
\begin{aligned}
& \min _{\mathrm{u}_1, \ldots \mathrm{u}_{\mathrm{T}}} \mathrm{c}\left(\mathrm{x}_1, \mathrm{u}_1\right)+\mathrm{c}\left(\mathrm{f}\left(\mathrm{x}_1, \mathrm{u}_1\right), \mathrm{u}_2\right)+\cdots+\mathrm{c}\left(\mathrm{f}\left(\mathrm{f}(\ldots() \ldots), \mathrm{u}_{\mathrm{T}}\right)-\mathrm{V}\left(\mathrm{x}_{\mathrm{T}+1}\right)\right. \\
& \max _{\mathrm{u}_1, \ldots, \mathrm{u}_{\mathrm{T}}} \mathrm{r}\left(\mathrm{x}_1, \mathrm{u}_1\right)+\mathrm{r}\left(\mathrm{f}\left(\mathrm{x}_1, \mathrm{u}_2\right), \mathrm{u}_2\right)+\cdots+\underbrace{\mathrm{r}\left(\mathrm{x}_{\mathrm{T}}, \mathrm{u}_{\mathrm{T}}\right)+\mathrm{V}\left(\mathrm{x}_{\mathrm{T}+1}\right)}_{\mathrm{Q}} \\
= & \max _{\mathrm{u}_1, \ldots, \mathrm{u}_{\mathrm{T}}} \mathrm{r}\left(\mathrm{x}_1, \mathrm{u}_1\right)+\mathrm{r}\left(\mathrm{f}\left(\mathrm{x}_1, \mathrm{u}_2\right), \mathrm{u}_2\right)+\cdots+\underbrace{\mathrm{Q}\left(\mathrm{x}_{\mathrm{T}}, \mathrm{u}_{\mathrm{T}}\right)}_{\mathrm{u}_{\mathrm{T}}=\mathrm{K}_{\mathrm{T}} \mathrm{x}_{\mathrm{T}}+\mathrm{k}_{\mathrm{T}}} \\
= & \max _{\left.\mathrm{u}_1, \ldots, \mathrm{uT}_{\mathrm{T}}, 1, \mathrm{u}_{\mathrm{T}}-1\right)} \mathrm{r}\left(\mathrm{x}_1, \mathrm{u}_1\right)+\mathrm{r}\left(\mathrm{f}\left(\mathrm{x}_1, \mathrm{u}_2\right), \mathrm{u}_2\right)+\cdots+\mathrm{r}\left(\mathrm{x}_{\mathrm{T}-1}, \mathrm{u}_{\mathrm{T}-1}\right)+\mathrm{V}\left(\mathrm{x}_{\mathrm{T}}, \mathrm{u}_{\mathrm{T}}\right) \\
= & \max _{\mathrm{u}_1, \ldots, \mathrm{u}_{\mathrm{T}}} \mathrm{r}\left(\mathrm{x}_1, \mathrm{u}_1\right)+\mathrm{r}\left(\mathrm{f}\left(\mathrm{x}_1, \mathrm{u}_2\right), \mathrm{u}_2\right)+\cdots+\mathrm{Q}\left(\mathrm{x}_{\mathrm{T}-1}, \mathrm{u}_{\mathrm{T}-1}\right) \\
= & \max _{\mathrm{u}_1, \ldots, \mathrm{uT}_{\mathrm{T}}} \mathrm{Q}\left(\mathrm{x}_1, \mathrm{u}_1\right)
\end{aligned}
$$
求解思路，固定一个变量，调整其它变量，一个个求嘛，但如果是固定 $\mathrm{u}_1$ ，即Forward Pass前向算法，会经过多个动态模型 $\mathrm{f}$ 的迭代，很难求解，于是先从BackWard角度考虑，即从 $\mathrm{u}_{\mathrm{T}}$ 入手。

Backward Pass步骤:
1. T时刻的 $\nabla_{\mathrm{u}_{\mathrm{T}}} \mathrm{Q}\left(\mathrm{x}_{\mathrm{T}}, \mathrm{u}_{\mathrm{T}}\right)=0$ ，得控制律 $\mathrm{u}_{\mathrm{T}}=\mathrm{K}_{\mathrm{T}} \mathrm{x}_{\mathrm{T}}+\mathrm{k}_{\mathrm{T}}$
2. 计算 $\mathrm{V}\left(\mathrm{x}_{\mathrm{T}}\right)$ ，并利用 $\mathrm{x}_{\mathrm{T}}=\mathrm{f}\left(\mathrm{x}_{\mathrm{T}-1}, \mathrm{u}_{\mathrm{T}-1}\right)$ 消元，从而得到 $\mathrm{Q}\left(\mathrm{x}_{\mathrm{T}-1}, \mathrm{u}_{\mathrm{T}-1}\right)=-\mathrm{c}\left(\mathrm{x}_{\mathrm{T}-1}, \mathrm{u}_{\mathrm{T}-1}\right)+\mathrm{V}\left(\mathrm{x}_{\mathrm{T}}\right)=-\mathrm{c}\left(\mathrm{x}_{\mathrm{T}-1}, \mathrm{u}_{\mathrm{T}-1}\right)+\mathrm{V}\left(\mathrm{f}\left(\mathrm{x}_{\mathrm{T}-1}, \mathrm{u}_{\mathrm{T}-1}\right)\right)$
3. $\nabla_{\mathrm{uT}} \mathrm{Q}\left(\mathrm{x}_{\mathrm{T}}, \mathrm{u}_{\mathrm{T}}\right)=0$ ，得控制律 $\mathrm{u}_{\mathrm{T}-1}=\mathrm{K}_{\mathrm{T}-1} \mathrm{x}_{\mathrm{T}-1}+\mathrm{k}_{\mathrm{T}-1}$
4. 计算 $\mathrm{V}\left(\mathrm{x}_{\mathrm{T}-1}\right)$ ，利用动态模型消元，得 $\mathrm{Q}\left(\mathrm{x}_{\mathrm{T}-2}, \mathrm{u}_{\mathrm{T}-2}\right)$
5. $\left.\nabla_{\mathrm{u}_{\mathrm{T}-2}} \mathrm{Q}\left(\mathrm{x}_{\mathrm{T}-2}, \mathrm{u}_{\mathrm{T}-2}\right)\right)=0$ ，得控制律 $\mathrm{u}_{\mathrm{T}-2}=\mathrm{K}_{\mathrm{T}-2} \mathrm{x}_{\mathrm{T}-2}+\mathrm{k}_{\mathrm{T}-2}$
6. 以此类推。
Forward Pass步骤:
利用动态模型计算下一状态，利用控制律，计算出相应动作[1]

### LQR算法流程
- Backward Pass for $t=T$ to $t=1$ :
$$
\begin{aligned}
& \mathrm{V}\left(\mathrm{x}_{\mathrm{t}}\right)=\text { const }+\frac{1}{2} \mathrm{x}_{\mathrm{t}}^{\mathrm{T}} \mathrm{V}_{\mathrm{t}} \mathrm{x}_{\mathrm{t}}+\mathrm{x}_{\mathrm{t}}^{\mathrm{T}} \mathrm{v}_{\mathrm{t}} \\
&
\end{aligned}
$$
- Forward Pass for $\mathrm{t}=1$ to $\mathrm{t}=\mathrm{T}$ :
$$
\begin{aligned}
& \mathrm{u}_{\mathrm{t}}=\mathrm{K}_{\mathrm{t}} \mathrm{x}_{\mathrm{t}}+\mathrm{k}_{\mathrm{t}} \\
& \mathrm{x}_{\mathrm{t}+1}=\mathrm{f}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)
\end{aligned}
$$
- LQR模块
即使上述过程不是很透彻，亦可如下图所示将LQR看成一个黑盒模块，对于已知deterministic的dynamics，使用 $L Q R$ 算法，便可得到一组动作序列 $\mathrm{u}_1, \ldots, \mathrm{u}_{\mathrm{T}}$ ，并可计算出状态序列。
输入: 初始状态 $\mathrm{x}_1$ ，目标状态 $\mathrm{x}_{\mathrm{T}+1}$ ，动态模型 $\mathrm{f}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)$ ，代价函数 $\mathrm{c}\left(\mathrm{x}_{\mathrm{t}}, \mathrm{u}_{\mathrm{t}}\right)$
输出: 动作序列 $\mathrm{u}_1, \ldots, \mathrm{u}_{\mathrm{T}}$ 和状态序列 $\mathrm{x}_1, \ldots, \mathrm{x}_{\mathrm{T}}$ ，即一条轨迹

[1]: https://blog.csdn.net/weixin_40056577/article/details/104270668

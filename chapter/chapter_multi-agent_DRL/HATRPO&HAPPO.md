MATRPO (HATRPO) 逻辑很清晰，理论推导做得很扎实，附录的公式推导近10页。code，作者 @动词大词动 ，ICLR2022
作者写了 论文解析blog，核心点解释得蛮清晰，不涉及复杂的数学推导。认真读下其blog就能懂个大概，再重读论文便轻松了
全文共7k字，纯手敲，此为下篇，主要讲 HATRPO 和 HAPPO。建议先读上篇，了解 Multi Agents 下的 Trust Region Learning，HATRPO 和 HAPPO 是其思想的实现。看该论文前需了解 single agent上Natural PG，TRPO 和 PPO。本博客含大量手写笔记，含大量个人主观理解。如有错误，欢迎指正
CSDN (内容同，排版不同)
​blog.csdn.net/qq_45832958/article/details/123644900?spm=1001.2014.3001.5501

强化学习 | Multi Agents | Trust Region | HATRPO | HAPPO (上)
124 赞同 · 9 评论文章

HATRPO
原理





伪代码

HAPPO
原理
To further alleviate the computation burden from Hessian Matrix in HATRPO,one can follow the idea of PPO by considering only using first order derivatives. This is achieved by making agent [Math Processing Error]
 choose a policy parameter [Math Processing Error]
 which maimises the clipping objecvite of

[Math Processing Error]

The optimisation process can be performed by stochastic gradient methods such as Adam.

伪代码

实验情况
SMAC
任务

SMAC (StarCraftll Multi-Agent Challenge) contains a set of StarCraft maps in which a team of ally units aims to defeat the opponent team.

结果

在该任务上，IPPO、MAPPO 这类 parameter sharing 算法，和 HATRPO、HAPPO 这类 non-parameter sharing 算法都达到了100%


分析

SMAC任务较简单，non-parameter sharing is not necessarily required，sharing policies is sufficient to solve SMAC tasks.

Multi-Agent MuJoCo
任务

A continuous control task. MuJoCo tasks challenge a robot to learn an optimal way of motion; Multi-Agent MuJoCo models each part of a robot as an independent agent, for example, a leg for a spider or an arm for a swimmer.

结果

HATRPO and HAPPO enjoy superior performance over those of parameter-sharing methods:IPPPO and MAPPO, and the gap enlarges with the number of agents increases.

HATRPO and HAPPO also outperform non-parameter sharing MADDPG with both in terms of reward values and variance.


分析

该任务较复杂，能较好与其它算法拉开差距，体现HATRPO和其背后原理的优越性

HATRPO比 参数共享方法 (MAPPO等) 性能好得多。而且随着智能体数目增加，两类算法差距越拉越大，这说明了modelling heterogeneous policies的必要性

HATRPO性能表现优于HAPPO，认为是 hard KL constraint 相较于 clipping 更接近原理描述



[1]:https://zhuanlan.zhihu.com/p/492954968

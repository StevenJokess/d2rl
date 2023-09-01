# 计算复杂性介绍(Introduction of computation complexit

9月19日[1]

* 参考文献：计算机与难解性问题
* 参考文献：[UCI讲座](https://www.ics.uci.edu/~eppstein/161/960312.html)

## 比较不同算法的优缺点

* 实验？需要在相同环境下运行
* 复杂性分析。独立于环境
* **规模**示例：
	* 二叉树搜索（深度 = n）：![方程](http://latex.codecogs.com/svg.latex?2%5En)
	* 旅行推销员问题（TSP，n个城市）：![方程](http://latex.codecogs.com/svg.latex?n%21)

## 问题分类

![复杂性](https://upload.wikimedia.org/wikipedia/commons/a/a0/P_np_np-complete_np-hard.svg)

![P、NP和NP完全](https://upload.wikimedia.org/wikipedia/commons/b/bc/Complexity_classes.svg)

* **P**：可以在多项式时间内解决的问题
* [**NP**](https://en.wikipedia.org/wiki/NP_%28complexity%29)：这代表着“非确定性多项式时间”，其中非确定性只是一种讨论**猜测**解决方案的高级方式
	* 不代表“非多项式”！
	* 如果您可以快速（在多项式时间内）测试一个解决方案是否正确（而不必担心找到解决方案有多难），那么问题就属于NP
	* ![方程](http://latex.codecogs.com/svg.latex?P%20%5Csubseteq%20NP)
* [减少（多项式减少）](https://en.wikipedia.org/wiki/Reduction_%28complexity%29)
	* 例如：`PA`：对整个班级的成绩进行排序；`PB`：找出班级中最高的分数
	* 如果能够有效地解决`PB`（如果存在）的算法也可以用作解决`PA`的子程序，则`PA`可以被多项式减少到`PB`
	* ![方程](http://latex.codecogs.com/svg.latex?P_A%20%5Cpropto%20P_B)
	* **`PB`至少与`PA`一样困难**
* [**NP完全性**](https://en.wikipedia.org/wiki/NP-completeness)：判断问题![方程](http://latex.codecogs.com/svg.latex?P_1%20%5Cin%20NP)，对于所有其他决策问题![方程](http://latex.codecogs.com/svg.latex?P%27%20%5Cin%20NP)，我们有![方程](http://latex.codecogs.com/svg.latex?P%27%20%5Cpropto%20P_1)，那么![方程](http://latex.codecogs.com/svg.latex?P_1)是NP完全性
	* NP完全性是所有NP问题中最难的问题
	* 对于任何给定的NP完全问题的解决方案都可以在多项式时间内验证
	* 但是没有已知的有效方法来首次找到解决方案
* 面对NP完全问题时该如何处理？
* NP完全集
* **NP难**

---


19th, Sept.

* Reference: computers and intractability
* Reference: [UCI lecture](https://www.ics.uci.edu/~eppstein/161/960312.html)

## Compare pros and cons of different algorithm
* Experiment? Need to run in the same environment
* Complexity analysis. Independent from environment
* **Scale** examples:
	* binary tree search (depth = n): ![equation](http://latex.codecogs.com/svg.latex?2%5En)
	* Traveling Salesman Problem (TSP, n cities): ![equation](http://latex.codecogs.com/svg.latex?n%21)

## Classification of problems
![complexity](https://upload.wikimedia.org/wikipedia/commons/a/a0/P_np_np-complete_np-hard.svg)

![P,NP and NP complete](https://upload.wikimedia.org/wikipedia/commons/b/bc/Complexity_classes.svg)

* **P**: Problems that can be solved in polynomial time
* [**NP**](https://en.wikipedia.org/wiki/NP_%28complexity%29): This stands for "nondeterministic polynomial time" where nondeterministic is just a fancy way of talking about **guessing** a solution
	* Does not stand for "non-polynomial"!!!
	* A problem is in NP if you can quickly (in polynomial time) test whether a solution is correct (without worrying about how hard it might be to find the solution)
	* ![equation](http://latex.codecogs.com/svg.latex?P%20%5Csubseteq%20NP)
* [Reduction (polynomial reduction)](https://en.wikipedia.org/wiki/Reduction_%28complexity%29)
	* Eg: `PA`: sort scores of the whole class; `PB`: find the highest score in the class
	* `PA` is polynomial reducible to `PB` if an algorithm for solving `PB` efficiently (if it existed) could also be used as a subroutine to solve `PA` efficiently
	* ![equation](http://latex.codecogs.com/svg.latex?P_A%20%5Cpropto%20P_B)
	* **`PB` is at least as hard as `PA`**
* [**NP-completeness**](https://en.wikipedia.org/wiki/NP-completeness): judgment problem ![equation](http://latex.codecogs.com/svg.latex?P_1%20%5Cin%20NP), for all other decision problems ![equation](http://latex.codecogs.com/svg.latex?P%27%20%5Cin%20NP), we have ![equation](http://latex.codecogs.com/svg.latex?P%27%20%5Cpropto%20P_1), then ![equation](http://latex.codecogs.com/svg.latex?P_1) is NP-completeness
	* NP completeness is the most hard problem of all NP problems
	* Any given solution to an NP-complete problem can be verified in polynomial time
	* But there is no known efficient way to locate a solution in the first place
* What to do when facing NP-complete problem?
* NP-complete set
* **NP hard**

[1]: https://chaonan99-note.readthedocs.io/en/latest/AI/03_complexity.html

> https://chat.openai.com/
> translate these into Chinese
> give me the markdown words

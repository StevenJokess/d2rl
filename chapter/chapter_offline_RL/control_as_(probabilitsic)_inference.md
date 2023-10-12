

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-12 21:47:16
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-12 21:56:57
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# RL and Control as Probabilistic Inference


作者：黄伟
链接：https://zhuanlan.zhihu.com/p/77099984
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## 阅读动机

将强化学习和概率推断模型结合起来是最近一个比较新颖的研究方向。我们把这个框架叫做control as (probabilitsic) inference，它被证明等价于最大熵(maximum entropy)框架，也就是诞生Soft Q-learning以及Soft actor-critic算法的那个最大熵框架。其实，关于control as inference的研究已经有比较久的历史了，Berkeley大牛Sergey Levine在2018年写了关于它的教程和综述文章：Reinforcement Learning and Control as Probabilistic Inference: Tutorial and Review。我们试图结合上面给的CS294-112的视频和PPT资料来对这篇综述文章做一个论文的阅读笔记，认识和理解一下所谓的control as inference。

### 阅读背景

学习强化学习的同学可能思考一个问题，就是它可以提供一个合理的模型来描述人类或者动物的行为吗？这里有个关于猴子的实验，让它操控一个物体移动到另一个位置：<img src="https://pic2.zhimg.com/v2-77ba54d99adfaf70b8276f30dae7e5d9_b.png" data-caption="" data-size="small" data-rawwidth="143" data-rawheight="212" class="content_image" width="143"/>

结果是：猴子每次都会完成目标，但是每次路线都不一样，而且都不是严格意义上最优路径：黑色的直线。和最优策略不同，猴子的行为近似最优，而且带有随机性。但是另一个角度分析，这些好的行为相似程度也很大。从这个例子可以看出来，优化问题已经不足以描述猴子的行为，而是要引入概率模型

Control as inference#1 概率图模型

TODO:

#4 policy computation

现在我们来推导策略：

$$
p\left(\mathbf{a}_t \mid \mathbf{s}_t, \mathcal{O}_{t: T}\right)=\frac{p\left(\mathbf{s}_t, \mathbf{a}_t \mid \mathcal{O}_{t: T}\right)}{p\left(\mathbf{s}_t \mid \mathcal{O}_{t: T}\right)}=\frac{p\left(\mathcal{O}_{t: T} \mid \mathbf{s}_t, \mathbf{a}_t\right) p\left(\mathbf{a}_t \mid \mathbf{s}_t\right) p\left(\mathbf{s}_t\right)}{p\left(\mathcal{O}_{t: T} \mid \mathbf{s}_t\right) p\left(\mathbf{s}_t\right)} \propto \frac{p\left(\mathcal{O}_{t: T} \mid \mathbf{s}_t, \mathbf{a}_t\right)}{p\left(\mathcal{O}_{t: T} \mid \mathbf{s}_t\right)}=\frac{\beta_t\left(\mathbf{s}_t, \mathbf{a}_t\right)}{\beta_t\left(\mathbf{s}_t\right)}
$$

其中，前几个等号过程为：

$$
\begin{aligned}
p\left(\mathbf{a}_t \mid \mathbf{s}_t, \mathcal{O}_{1: T}\right) & =\pi\left(\mathbf{a}_t \mid \mathbf{s}_t\right) \\
& =p\left(\mathbf{a}_t \mid \mathbf{s}_t, \mathcal{O}_{t: T}\right) \\
& =\frac{p\left(\mathbf{a}_t, \mathbf{s}_t \mid \mathcal{O}_{t: T}\right)}{p\left(\mathbf{s}_t \mid \mathcal{O}_{t: T}\right)} \\
& =\frac{p\left(\mathcal{O}_{t: T} \mid \mathbf{a}_t, \mathbf{s}_t\right) p\left(\mathbf{a}_t, \mathbf{s}_t\right) / p\left(\mathcal{O}_{t: T}\right)}{p\left(\mathcal{O}_{t: T} \mid \mathbf{s}_t\right) p\left(\mathbf{s}_t\right) / p\left(\mathcal{O}_{t: T}\right)} \\
& =\frac{p\left(\mathcal{O}_{t: T} \mid \mathbf{a}_t, \mathbf{s}_t\right) p\left(\mathbf{a}_t, \mathbf{s}_t\right) }{p\left(\mathcal{O}_{t: T} \mid \mathbf{s}_t\right) p\left(\mathbf{s}_t\right) }
\end{aligned}
$$



让我们把Q和V带入进来：

$$
\pi\left(\mathbf{a}_t \mid \mathbf{s}_t\right)=\exp \left(Q_t\left(\mathbf{s}_t, \mathbf{a}_t\right)-V_t\left(\mathbf{s}_t\right)\right)=\exp \left(A_t\left(\mathbf{s}_t, \mathbf{a}_t\right)\right)
$$

## 总结

我们通过一个的概率图模型，通过Inference的方法，得到了backward messages以及策略。它被证明和maximum entropy框架等价。这对于我们加深理解soft q-learning以及soft actor-critic算法有很大的帮助，同时也为我们研究强化学习增加一条强有力的道路。

[1]: https://zhuanlan.zhihu.com/p/77099984

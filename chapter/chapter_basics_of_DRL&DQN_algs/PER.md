

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-11 11:29:38
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-11 18:37:01
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Prioritized Experience Replay(PER)



## Prioritized replay 算法


这一套算法重点就在我们 batch 抽样的时候并不是随机抽样，而是按照 Memory 中的样本优先级来抽。 所以这能更有效地找到我们需要学习的样本。

那么样本的优先级是怎么定的呢? 原来我们可以用到 TD-error，也就是 Q现实 - Q估计 来规定优先学习的程度。 如果 TD-error 越大，就代表我们的预测精度还有很多上升空间，那么这个样本就越需要被学习，也就是优先级 p 越高。

有了 TD-error 就有了优先级 p，那我们如何有效地根据 p 来抽样呢? 如果每次抽样都需要针对 p 对所有样本排序，这将会是一件非常消耗计算能力的事。 好在我们还有其他方法，这种方法不会对得到的样本进行排序. 这就是这篇 [paper](https://arxiv.org/abs/1511.05952) 中提到的 SumTree。

SumTree 是一种树形结构，每片树叶存储每个样本的优先级 p，每个树枝节点只有两个分叉，节点的值是两个分叉的合，所以 SumTree 的顶端就是所有 p 的合. 正如下面图片(来自[Jaromír Janisch](https://jaromiru.com/2016/11/07/lets-make-a-dqn-double-learning-and-prioritized-experience-replay/))，最下面一层树叶存储样本的 p，叶子上一层最左边的 13 = 3 + 10，按这个规律相加，顶层的 root 就是全部 p 的合了。

![SumTree](../../img/Sum_Tree.png)

抽样时，我们会将 p 的总合 除以 batch size，分成 batch size 那么多区间，(n=sum(p)/batch_size). 如果将所有 node 的 priority 加起来是42的话，我们如果抽6个样本，这时的区间拥有的 priority 可能是这样。

[0-7)，[7-14)，[14-21)，[21-28)，[28-35)，[35-42]

然后在每个区间里随机选取一个数. 比如在第区间 [21-28) 里选到了24，就按照这个 24 从最顶上的42开始向下搜索

选择方法：**如果 左边的 child 比自己手中的值大，那我们就走左边这条路**，[1]

- 首先看到最顶上 42 下面有两个 child nodes，拿着手中的24对比左边的 child 29，
- 接着再对比 29 下面的左边那个点 13，这时，手中的 24 比 13 大，那我们就走右边的路；
- 并且将手中的值根据 13 修改一下，变成 24-13 = 11；
- 接着拿着 11 和 13 左下角的 12 比，结果 12 比 11 大，
- 那我们就选 12 当做这次选到的 priority，并且也选择 12 对应的数据。


可以看出, 有 Prioritized replay 的可以高效的利用这些不常拿到的奖励, 并好好学习他们. 所以 Prioritized replay 会更快结束每个 episode, 很快就到达了小旗子.



[1]: https://mofanpy.com/tutorials/machine-learning/reinforcement-learning/prioritized-replay#%E8%A6%81%E7%82%B9
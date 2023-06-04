

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-04 18:51:46
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-04 18:56:49
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# PDPG

Pathwise Derivative Policy Gradient。这个方法可以看成是 Q-learning 解连续动作的一种特别的方法，也可以看成是一种特别的 Actor-Critic 的方法。[1]

## 比喻

用棋灵王来比喻的话，阿光是一个 actor，佐为是一个 critic。阿光落某一子以后，

- 如果佐为是一般的 Actor-Critic，他会告诉阿光说这时候不应该下小马步飞，他会告诉你，你现在采取的这一步算出来的 value 到底是好还是不好，但这样就结束了，他只告诉你说好还是不好。因为一般的这个 Actor-Critic 里面那个 critic 就是输入状态或输入状态跟动作的对(pair)，然后给你一个 value 就结束了。所以对 actor 来说，它只知道它做的这个行为到底是好还是不好。
- 但如果是在 pathwise derivative policy gradient 里面，这个 critic 会直接告诉 actor 说采取什么样的动作才是好的。所以今天佐为不只是告诉阿光说，这个时候不要下小马步飞，同时还告诉阿光说这个时候应该要下大马步飞，所以这个就是 Pathwise Derivative Policy Gradient 中的 critic。critic **会直接告诉** actor 做什么样的动作才可以得到比较大的 value。


从 Q-learning 的观点来看，Q-learning 的一个问题是你在用 Q-learning 的时候，考虑 continuous vector 会比较麻烦，比较没有通用的解决方法(general solution)，怎么解这个优化问题呢？

我们用一个 actor 来解这个优化的问题。本来在 Q-learning 里面，如果是一个连续的动作，我们要解这个优化问题。但是现在这个优化问题由 actor 来解，假设 actor 就是一个 solver，这个 solver 的工作就是给定状态 s，然后它就去解，告诉我们说，哪一个动作可以给我们最大的 Q value，这是从另外一个观点来看 pathwise derivative policy gradient 这件事情。

在 GAN 中也有类似的说法。我们学习一个 discriminator 来评估东西好不好，要 discriminator 生成东西的话，非常困难，那怎么办？因为要解一个 arg max 的问题非常的困难，所以用 generator 来生成。

所以今天的概念其实是一样的，Q 就是那个 discriminator。要根据这个 discriminator 决定动作非常困难，怎么办？另外学习一个网络来解这个优化问题，这个东西就是 actor。

所以两个不同的观点是同一件事。从两个不同的观点来看，

一个观点是说，我们可以对原来的 Q-learning 加以改进，我们学习一个 actor 来决定动作以解决 arg max 不好解的问题。
另外一个观点是，原来的 actor-critic 的问题是 critic 并没有给 actor 足够的信息，它只告诉它好或不好，没有告诉它说什么样叫好，那现在有新的方法可以直接告诉 actor 说，什么样叫做好。

[1]: https://blog.csdn.net/qq_44766883/article/details/113062953

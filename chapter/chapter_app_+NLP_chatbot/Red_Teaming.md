

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2024-06-22 09:53:58
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2024-06-22 10:23:24
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Red Teaming Language Models to Reduce Harms: Methods, Scaling Behaviors, and Lessons Learned

论文地址：https://arxiv.org/abs/2209.07858

## 摘要：

我们描述了早期红蓝语言模型的工作，发现、测量并尝试减少其潜在的有害输出。

我们作出了三个主要贡献。

1. 首先，我们研究了红队在3种模型大小(2.7B, 13B和52B参数)和4种模型类型中的缩放行为: 普通语言模型(LM);一个有用、诚实、无害的提示LM;有抑制抽样的LM;以及一个使用从人类反馈中强化学习(RLHF)训练成有益无害的模型。我们发现RLHF模型随着规模的扩大对红队来说越来越困难，我们发现其他模型类型的规模呈平缓趋势。
2. 其次，我们发布了38,961次红队攻击的数据集，供其他人分析和学习。我们提供了自己对数据的分析，并发现了各种有害的输出，从攻击性语言到更微妙的有害非暴力不道德输出。
3. 第三，我们详尽地描述了我们的指示、流程、统计方法和关于红队的不确定性。我们希望这种透明度能够加速我们作为一个社区一起工作的能力，以便为红队语言模型开发共享的规范、实践和技术标准。[2]



在本文中，作者描述了他们早期进行手动红队测试的努力，旨在提高模型的安全性并测量模型的安全性。他们针对三种不同规模的模型（2.7B、13B和52B参数）和四种不同类型的模型进行了红队测试，包括纯文本语言模型（Plain LM）；被提示要做到有益、诚实和无害的LM（HHH Prompted LM）；具有拒绝抽样（Rejection Sampling, RS）的LM，该抽样返回由有益和无害偏好模型排名最佳的2个样本；以及通过人类反馈使用强化学习训练为有益和无害的模型（RLHF），该模型使用相同的偏好模型，RS和RLHF模型依赖于从HHH提示的LM中生成的数据。


图1的中间部分显示了以下情况：（1）随着模型的扩展，RLHF模型越来越难进行红队测试，（2）纯文本LM、提示LM和RS模型在不同规模下没有明显变化，（3）提示LM进行红队测试并没有比纯文本LM更难，这与我们以前使用静态评估结果展示HHH提示是有效的安全干预措施的结果不一致，（4）RS模型在任何规模下都最难进行红队测试；然而，从定性上看，它们往往通过回避方式来减少危害。[1]

[1]: https://zhuanlan.zhihu.com/p/664096097
[2]: https://www.cnblogs.com/xuehuiping/p/17139990.html

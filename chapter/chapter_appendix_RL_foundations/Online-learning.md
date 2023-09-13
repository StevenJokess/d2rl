

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-12 15:41:57
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-12 23:26:55
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
https://cs.nju.edu.cn/zlj/Talk/2017_ISICDM.pdf

# 在线预测（Online Prediction）

在线预测（Online Prediction）问题是一类智能体需要为未来做出预测的问题。假如你在夏威
夷度假一周，需要预测这一周是否会下雨；或者根据一天上午的石油价格涨幅来预测下午石油的
价格。在线预测问题需要在线解决。在线学习和传统的统计学习有以下几方面的不同：

- 样本是以一种有序的（Ordered）方式呈现的，而非无序的批（Batch）的方式。
46
- 我们更多需要考虑最差情况而不是平均情况，因为我们需要保证在学习过程中随时都对事
情有所掌控。




<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-01 00:05:39
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-01 00:05:58
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 金融领域的强化学习

RL is a natural solution to some finance and economics problems (Hull, 2014; Luenberger, 1997),
like option pricing (Longstaff and Schwartz, 2001; Tsitsiklis and Van Roy, 2001; Li et al., 2009),
and multi-period portfolio optimization (Brandt et al., 2005; Neuneier, 1997), where value function
based RL methods were used. Moody and Saffell (2001) proposed to utilize policy search to learn
to trade; Deng et al. (2016) extended it with deep neural networks. Deep (reinforcement) learning
would provide better solutions in some issues in risk management (Hull, 2014; Yu et al., 2009). The
market efficiency hypothesis is fundamental in finance. However, there are well-known behavioral
biases in human decision-making under uncertainty, in particular, prospect theory (Prashanth et al.,
2016). A reconciliation is the adaptive markets hypothesis (Lo, 2004), which may be approached by
reinforcement learning.

It is nontrivial for finance and economics academia to accept blackbox methods like neural networks;
Heaton et al. (2016) may be regarded as an exception. However, there is a lecture in AFA 2017
annual meeting: Machine Learning and Prediction in Economics and Finance. We may also be
aware that financial firms would probably hold state-of-the-art research/application results.
FinTech has been attracting attention, especially after the notion of big data. FinTech employs
machine learning techniques to deal with issues like fraud detection (Phua et al., 2010), consumer
credit risk (Khandani et al., 2010), etc.

---

翻译成中文： RL 是一些金融和经济问题的自然解决方案 (Hull, 2014; Luenberger, 1997)，比如期权定价（Longstaff 和 Schwartz，2001 年；Tsitsiklis 和 Van Roy，2001 年；Li 等人，2009 年），和多周期投资组合优化 (Brandt et al., 2005; Neuneier, 1997)，其中价值函数使用了基于 RL 的方法。 Moody 和 Saffell (2001) 提出利用策略搜索来学习
交易;邓等。 (2016) 用深度神经网络扩展它。深度（强化）学习将为风险管理中的一些问题提供更好的解决方案（Hull，2014；Yu et al.，2009）。这市场效率假说是金融学的基础。但是，有一些众所周知的行为不确定性下人类决策的偏差，特别是前景理论（Prashanth 等人，2016 年）。调和是适应性市场假设（Lo，2004），可以通过强化学习。

财经学术界接受神经网络这样的黑盒方法是非同小可的；希顿等人。 （2016）可能被视为一个例外。然而，AFA 2017中有一个讲座年会：经济学和金融学中的机器学习和预测。我们也可能是意识到金融公司可能拥有最先进的研究/应用成果。
金融科技一直备受关注，尤其是在大数据概念之后。金融科技雇用机器学习技术来处理诸如欺诈检测（Phua 等人，2010 年）、消费者信用风险 (Khandani et al., 2010) 等。[1]

[1]: https://arxiv.org/abs/1701.07274

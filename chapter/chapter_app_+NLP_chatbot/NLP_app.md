

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-11-06 06:54:54
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-11-06 06:57:13
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# NLP应用

相当长时间里，NLP 领域都有大量细分任务，由于 NLP 领域面对的问题太过庞杂，因此前面很多年 NLP 领域任务都被拆分的非常细，比如（下列任务大家看个感觉就好，暂时不搞懂细节不影响理解）：

- 命名实体识别（Named Entity Recognition，NER）：比如对于输入语句「擎天柱回到赛博坦」得到输出「B-PER, I-PER, E-PER, O, O, B-LOC, I-LOC, E-LOC」，其中 B、I、E 分别表示开始、中间、结束，PER、LOC 分别表示人物、地点，O 表示其他无关。
- 文本蕴含（Text Entailment）：比如对于文本 T「我在杭州」和如下三个假设 H1「我在浙江」、H2「我在上海」、H3「我是杭州人」之间的蕴含关系就是 Positive、Negative、Neutral，其实是个三分类问题。
- 常识推理（Common Sense Reasoning）：比如一个测试 LM 是否具备常识推理的例子，在句子 A「奖杯无法放进到箱子里，因为它太了」中的「它」指的谁？在句子 B「奖杯无法放进到箱子里，因为它大了」中「它」指的谁？。
- 问答（Question Answering）。
- 词性标注（POS Tagging）。
- 情感分析（Sentiment Analysis，SA）。
- 自然语言推理（Natural Language Inference，NLI）。
- 总结摘要（Summarization）。
……

太多细分任务了，这里不一一举例。每个任务也自然有它的测试数据集，由研究人员开发出新的任务解法（可能是模型创新，也可能是训练方法创新，甚至是一些小 tricks）后去「打榜」，也就是检验下目标任务在测试数据集上的表现如何。下面我引用《A Comprehensive Survey on Pretrained Foundation Models: A History from BERT to ChatGPT》一文中对 NLP 领域数据集的汇总。

[1]: https://www.mikecaptain.com/2023/03/06/captain-aigc-2-llm/

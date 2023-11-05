

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-11-03 09:05:19
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-11-04 09:34:01
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Encoder-Decoder

## Encoder-Decoder

Cho在2014年的《Learning Phrase Representations using RNN Encoder–Decoder for Statistical Machine Translation》中提出了Encoder–Decoder结构，它由两个RNN组成，另外本文还提出了GRU的门结构，相比LSTM更加简洁，而且效果不输LSTM。

![Encoder–Decoder结构](../../img/Encoder-Decoder.png)

## Encoder-Decoder框架

生成目标句子单词的过程成了下面的形式，其中f1是Decoder的非线性变换函数：

$\begin{aligned} & \mathbf{Y}_{\mathbf{1}}=\mathbf{f} \mathbf{1}(\mathbf{C}) \\ & \mathbf{Y}_2=\mathbf{f} \mathbf{1}\left(\mathbf{C}, \mathbf{Y}_{\mathbf{1}}\right) \\ & \mathbf{Y}_3=\mathbf{f} \mathbf{1}\left(\mathbf{C}, \mathbf{Y}_{\mathbf{1}}, \mathbf{Y}_2\right)\end{aligned}$

Encoder和Decoder部分可以是任意的文字、语音、图像和视频数据，模型可以采用CNN，RNN，BiRNN、LSTM、GRU等等，所以基于Encoder-Decoder的结构，我们可以设计出各种各样的应用算法。比如：

1. 文字-文字：机器翻译，对话机器人，文章摘要，代码补全；
2. 音频-文字：语音识别；
3. 图片-文字：图像描述生成

> Encoder-Decoder的出现，对于很多领域的影响是非常深远的，比如机器翻译的任务，再也不需要做词性分析、词典查询、语序调整和隐马尔可夫模型等等方案，几乎都是基于Encoder-Decoder框架的神经网络方案。

## 序列到序列模型（sequence-to-sequence）


## Encoder-Decoder和Seq2Seq的关系

这两种叫法基本都是前后脚被提出来的，其实是技术发展到一定阶段自然的一次演进，基本上可以划上等号，如果非要讲他们的差别，那么就只能说下面着两条了。

Seq2Seq使用的具体方法基本都属于Encoder-Decoder模型的范畴。
Seq2Seq不特指具体方法，只要满足输入序列到输出序列的目的，都可以统称为Seq2Seq模型，即Seq2Seq强调目的，Encoder-Decoder强调方法。


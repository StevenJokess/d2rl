

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-11-01 22:19:47
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-11-03 11:59:15
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# LLM(Transformer)

## Transformer


### 简介

Transformer最早是Google在2017年的[Attention Is All You Need](https://arxiv.org/abs/1706.03762)论文中提出，用于解决解决传统的序列到序列（sequence-to-sequence，Seq2Seq）模型在**处理可变长序列**时遇到的问题。

> 序列到序列（Seq2Seq）：指的是模型的输入是一段序列，模型输出也是序列；比如语音识别中给模型一段中文语音序列，让模型给出中文文字序列

在传统的序列建模方法中，如循环神经网络（RNN）和卷积神经网络（CNN），在处理长序列均存在限制：

- RNN在处理长序列时容易出现梯度消失或梯度爆炸的问题，模型难以捕捉到长距离的依赖关系，**不能并行计算**；
- CNN卷积操作通常要求输入具有固定的尺寸，在处理可变长序列时，为了使序列具有相同的长度，需要对较短的序列进行填充，导致**计算效率低下**。

而Transformer通过自注意力机制（self-attention）来解决了这些问题，从而使得Transformer成为解决NLP中许多问题的首选模型，取代了较老的循环神经网络模型，如长短期记忆（LSTM）。由于Transformer模型有利于在训练过程中实现更多的并行化，因此它已经实现了在更大的数据集上进行训练，而这在它被引入之前是不可能的。


##

在引入Transformer之前，大多数最先进的NLP系统都依赖于门控递归神经网络（RNNs），如LSTMs和门控递归单元（GRU），并增加了注意力机制。Transformer在不使用RNN结构的情况下，在这些注意技术的基础上，强调了在不进行递归顺序处理的情况下，仅凭注意机制就足以达到带注意的RNN的性能。

注意力机制让模型直接查看、提取句子中任何较早期的状态。注意力层可以访问所有之前的状态，并根据一些学习到的与当前令牌相关性的衡量标准对它们进行权衡，提供更清晰的远期相关令牌的信息。注意力实用性的一个明显例子是在翻译中。在英译法系统中，法语输出的第一个词很可能在很大程度上取决于英语输入的开头。然而，在经典的编码器-解码器LSTM模型中，为了产生法语输出的第一个单词，模型只被赋予最后一个英语单词的状态向量。理论上，这个向量可以编码整个英语句子的信息，给模型提供所有必要的知识，但在实际应用中，这些信息往往不能很好地保存下来。如果引入注意力机制，模型反而可以学会在产生法语输出的开头时关注早期英语向量的状态，使其对翻译的内容有更好的概念。

和之前发明的模型一样，Transformer是一个编码器-解码器的架构。编码器由一组编码层组成，对输入进行一层又一层的迭代处理，解码器由一组解码层组成，对编码器的输出做同样的事情。

每个编码器层的功能是处理它的输入，生成编码，包含输入中哪些部分是相互相关的信息。它把它的一组编码作为输入传递给下一个编码器层。每一个解码层则反其道而行之，将所有的编码进行处理，利用它们所包含的上下文信息生成一个输出序列。为了达到这个目的，每一个编码器层和解码层都利用一个注意机制，对于每一个输入，它都会权衡其他每一个输入的相关性，并相应地从中抽取信息，从而产生输出。每一层解码器还有一个额外的注意机制，在解码层从编码中抽取信息之前，它先从前面解码器的输出中抽取信息。编码器层和解码器层都有一个前馈神经网络，用于对输出进行额外处理，并包含残余连接和层归一化步骤[2]。

TODO:

![Transformer](../../img/Transformer.png)

## 实战代码

Hugging Face Transformers:


## 小结

随着自注意力的发展，RNN 单元被完全抛弃。被称为多头注意力的自注意力与前馈神经网络形成了Transformer。

Transformer 作为一个重要的里程碑，促进了预训练系统的发展，如BERT（Bidirectional Encoder Representations from Transformers）和GPT（Generative Pre-trained Transformer），这些系统已经用巨大的通用语言数据集进行了训练，并可以针对特定的语言任务进行微调。


## 问题测验

1. 强化学习和大模型之间的关联是什么
1. 如何评估大模型中数据集的质量
1. 除了数据之外，还有哪些方向的工作可以进一步优化大模型的效果
1. 大语言模型是怎么输出的，观察过输出的概率值吗[3]
1. 介绍下对Transformer的了解
1. Transformer和RNN有什么相同点与不同点？
1. 网络结构相比于LSTM有什么不同
1. Transformer里用到的正则化方法有哪些
1.

## 附录：问题答案


Transformer和RNN的相同点与不同点：

与RNN相比：都处理顺序数据，但不要求一定按顺序

- 相同点：与递归神经网络（RNNs）一样，Transformers也被设计用来处理顺序数据，如自然语言，以完成翻译和文本摘要等任务。
- 不同点：与RNNs不同的是，Transformers并**不要求顺序处理顺序数据**。
  - 例如，如果输入数据是一个自然语言句子，Transformer就不需要先处理开头再处理结尾。
  - 由于这个特点，Transformer可以比RNNs的**并行化**程度高得多，因此可以减少训练时间。


[1]: https://developer.aliyun.com/article/1229038
[2]: https://zhuanlan.zhihu.com/p/165184447
[3]: https://zhuanlan.zhihu.com/p/659551066
[3]:

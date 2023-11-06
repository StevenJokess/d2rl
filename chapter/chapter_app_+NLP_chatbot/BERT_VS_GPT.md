

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-23 12:54:05
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-11-03 09:08:11
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# BERT与GPT

## 起源

- BERT: Bidirectional Encoder Representations from Transformers
- GPT: Generative Pre-trained Transformer


## BERT

BERT关键技术创新是将流行的注意力模型 Transformer 的双向训练应用于语言建模。论文的结果表明，双向训练的语言模型比单向语言模型可以更深入地感知语言上下文环境。在论文中，研究人员详细介绍了一种名为 Masked LM (MLM) 的新技术，该技术能够使得模型进行双向训练。BERT 模型是由 Transformer 模型的 Encoder 部分组成的。

许多模型预测序列中的下一个单词（例如 The child came home from ___），这种定向方法本质上限制了上下文学习。为了克服这一挑战，BERT 使用了两种训练策略：

### Masked Language Model（MLM）

分词是自然语言处理领域中的基础任务，是文本预处理的重要步骤。简单来说，就是将文本段落分解为基本语言单位，亦可称之为词元 ( token )。

MLM 是 BERT 能够不受单向语言模型所限制的原因。简单来说就是以 15% 的概率用 mask token （[MASK]）随机地对每一个训练序列中的 token 进行替换，然后预测出 [MASK] 位置原有的单词。然而，由于 [MASK] 并不会出现在下游任务的微调（fine-tuning）阶段，因此预训练阶段和微调阶段之间产生了不匹配（这里很好解释，就是预训练的目标会令产生的语言表征对 [MASK] 敏感，但是却对其他 token 不敏感）。因此 BERT 采用了以下策略来解决这个问题：

首先在每一个训练序列中以 15% 的概率随机地选中某个 token 位置用于预测，假如是第 i 个 token 被选中，则会被替换成以下三个 token 之一

80% 的时候是 [MASK]。如，my dog is hairy——>my dog is [MASK]

10% 的时候是随机的其他 token。如，my dog is hairy——>my dog is apple

10% 的时候是原来的 token。如，my dog is hairy——>my dog is hairy

再用该位置对应的 Unexpected text node: 'T_i'
T
i
​
  去预测出原来的 token（输入到全连接，然后用 softmax 输出每个 token 的概率，最后用交叉熵计算 loss）。该策略令到 BERT 不再只对 [MASK] 敏感，而是对所有的 token 都敏感，以致能抽取出任何 token 的表征信息。

那么为啥要以一定的概率使用随机词呢？这是因为 Transformer 要保持对每个输入 token 分布式的表征，否则 Transformer 很可能会记住这个 [MASK] 就是"hairy"。至于使用随机词带来的负面影响，文章中解释说，所有其他的 token（即非"hairy" 的 token）共享 15% * 10% = 1.5% 的概率，其影响是可以忽略不计的。Transformer 全局的可视，又增加了信息的获取，但是不让模型获取全量信息。

## Next Sentence Prediction（NSP）
一些如问答、自然语言推断等任务需要理解两个句子之间的关系，而 MLM 任务倾向于抽取 token 层次的表征，因此不能直接获取句子层次的表征。为了使模型能够有能力理解句子间的关系，BERT 使用了 NSP 任务来预训练，简单来说就是预测两个句子是否连在一起。具体的做法是：对于每一个训练样例，我们在语料库中挑选出句子 A 和句子 B 来组成，50% 的时候句子 B 就是句子 A 的下一句（标注为 IsNext），剩下 50% 的时候句子 B 是语料库中的随机句子（标注为 NotNext）。接下来把训练样例输入到 BERT 模型中。




## BERT与GPT的相同点与不同点：



相同点：

- 都是大规模预训练语言模型。
- 都是基于Transformer架构的深度学习网络。![BERT（左）和GPT（右）[1]](../../img/Transformer(BERT&GPT).png)

不同点：
  - BERT使用了Transformer架构中解码器（Encoder）模块进行预训练。
    - 论文：Google大厂。
    - 目标：中间。具体来说，对输入文本进行随机遮挡，训练「完形填空」的过程，比如The __ sat on the mat，让算法学习被遮住cat单词的信息。
    - 优点：其对**文本理解和分类**有着非常好的效果，这使得BERT经过微调后能够快速适应多种下游任务，如：BertForSequenceClassification、BertForQuestionAnswering、BertForTokenClassification (NER)。
    - 缺点：
      1. 由于BERT训练时，包含了预测任务不必要的后文内容，所以不适合用于预测任务。
      2. 由于BERT是双向模型，故相比GPT较大，训练需要更多的计算资源和时间，可能在某些情况下不划算。
    - 预训练：大小（size）1；
  - GPT利用了Transformer中的生成器Decoder模块进行预训练。
    - 论文：OpenAI实验室。
    - 目标：最后。让模型续写The cat sat on __，即生成后面的内容。
    - 优点：
      1. 预测能力强，导致文本生成得很通顺。由于使用的Masked Self-Attention是对后面遮住的部分进行生成训练，模型在生成文本时能够使用当前位置之前的文本信息进行预测，通过Attention机制更好地保留上下文信息，并提高生成文本的连贯性。
      2. 泛化能力强。支持少样本学习（few shot learning），即在下游任务中只需少量的用户提示。
    - 缺点：TODO:

![BERT和GPT在自注意力机制和目标的不同点](../../img/BERT&GPT_difference.jpg)


## 业界青睐：之前BERT，目前GPT

在GPT训练参数规模上升之前，业界更看好BERT这种训练方法。但是GPT参数规模上升后涌现出来的few-shots能力让它也有了快速适应下游任务能力，再加之优秀的文本生成能力，以及添加了强化学习之后的ChatGPT的惊艳效果，使得GPT快速抢过了BERT的风头。[2]

## 后续

https://transformers.run/back/transformer/#%E6%B3%A8%E6%84%8F%E5%8A%9B%E5%B1%82

## 问题测验

1. BERT分为哪两种任务，各自的作用是什么；
2. 在计算MLM预训练任务的损失函数的时候，参与计算的Tokens有哪些？是全部的15%的词汇还是15%词汇中真正被Mask的那些tokens？
3. 在实现损失函数的时候，怎么确保没有被 Mask 的函数不参与到损失计算中去；
4. BERT的三个Embedding为什么直接相加
5. BERT的优缺点分别是什么？
6. 你知道有哪些针对BERT的缺点做优化的模型？
7. BERT怎么用在生成模型中？

[1]: https://www.youtube.com/watch?v=ewjlmLQI9kc
[2]: https://juejin.cn/post/7218048201982787645
[3]: https://blog.csdn.net/qq_38915354/article/details/131054219
[4]: https://transformers.run/back/transformer/#%E6%B3%A8%E6%84%8F%E5%8A%9B%E5%B1%82

> Prompt：为何说，因为BERT能看到后文的内容，因此对预测能力的训练不是非常有利



<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-11-03 07:51:05
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2024-07-22 01:22:22
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 语言模型（RNN、LSTM、GRU）

## 语言模型

语言模型是一个用来估计文本概率分布的数学模型，它可以帮助我们了解某个词语序列在自然语言中出现的概率。一个句子是否合理，取决于其出现在自然语言中的可能性的大小。概率越大，那么这个句子越合理，那么如何表示一个句子在整个自然语言空间中出现的概率大小呢？

### 任务

一、语言模型（LM）的任务就是预测一段文字接下来会出现什么词。[1]

即一个语言模型应该有能力计算下面这个公式的值:

$$
P\left(x^{(t+1)} \mid x^{(t)}, \ldots, x^{(1)}\right)
$$

翻译过来就是，在已知一句话的前 $\mathrm{t}$ 个词的基础上，通过LM可以计算出下一个词是某个词的概率。

二、语言模型（LM）给一段文本赋予一个概率。

即对于一段文字 $x^{(1)}, x^{(2)}, \ldots, x^{(T)}$ ，LM可以计算出这句话出现的概率:

$$
\begin{aligned}
& P\left(x^{(1)}, \ldots, x^{(T)}\right) \\
& =P\left(x^{(1)}\right) \cdot P\left(x^{(2)} \mid x^{(1)}\right) \cdot P\left(x^{(3)} \mid x^{(2)}, x^{(1)}\right) \ldots \\
& =\Pi_{t=1}^T P\left(x^{(t)} \mid x^{(t-1)}, \ldots, x^{(1)}\right)
\end{aligned}
$$

其中

$$
P\left(x^{(t)} \mid x^{(t-1)}, \ldots, x^{(1)}\right)
$$

就是LM可以计算出来的。


### 应用

- 输入法联想。比如我在搜狗输入法里写一个“我”，输入法会给我推荐几个词：“女朋友”、“刚刚”、“的”等等。这会暴露我们个人的一些习惯。
- 搜索联想。基于神经网络的语言模型演进历程

### 如何学习语言模型：n-gram模型

所谓的N-gram，就是指一堆连续的词。

根据这一堆词的个数，我们可以分为
1. unigram(1-gram),
2. bigram(2-gram),
3. trigram(3-gram),
4. n-gram 等等。

如果我们需要得到一个N-gram的LM，它的意思就是希望我们可以通过N-1个词预测第N个词的概率。

那么如何学习得到一个N-gram的LM呢？一个直接的思路就是，我们可以收集关于语料中的各个N-gram出现的频率信息。

对于一个N-gram的LM，我们需要做一个假设：某个词出现的概率只由其前N-1个词决定。

比如我们想得到一个3-gram的LM，那么就是说想预测一段文本的下一个词是什么的话，只用看这个词前面2个词即可。


假设 S 表示一个有意义的句子，由一连串按特定顺序排列的词 $W_1,W_2...W_N$ 组成，那么我们要求S在整个自然语言的空间内出现的可能性，即P(S)，由于 $W_1....W_n$ 同时在这个句子中出现，因此整个句子出现的概率就是这些词的联合概率，即：
$P(S)=P(W_1,W_2,...,M_3) \\$利用条件概率公式展开，得，

$ \begin{aligned} P(S)&=P(W_1,W_2,...,M_3) \\ &=P(W_1)\cdot P(W_2|W_1)\cdot P(W_3|W_2,W_1)\cdot\cdot\cdot P(W_N|W_{N-1},W_N,...,W_1) \end{aligned}\\$ 利用马尔科夫假设，假设当前的词仅仅与他前面出现的一个词相关，那么上式可以转化为：

$ \begin{aligned} P(S)&=P(W_1,W_2,...,M_3) \\ &=P(W_1)\cdot P(W_2|W_1)\cdot P(W_3|W_2)\cdot\cdot\cdot P(W_N|W_{N-1}) \end{aligned}\\$这就是一个N-gram模型，并且由于每一次算条件概率时，词组的滑窗长度为2，所以这是一个2-gram模型 ，N-gram是一个基于统计的语言模型，我们需要不断地在语料空间中计算每个滑窗出现的概率大小，这样在我们生成时候，我们才能找出出现概率最大的词当做下一个词，不断地迭代这个过程，直到达到终止条件。下面是一个流程：


代码实现：
接下来我们基于这个流程进行bigram（2-gram）整体代码的编写：

①构建语料库
我们随意构建一个用于模型训练的语料库，然后将基于该语料库训练得到的模型用于文字生成，语料库越大，质量越高，越接近自然语言空间，生成的效果肯定越好，由于我们的重点是捋清其实现思路，所以构建一个简单的语料库，如下：

corpus = [ "我喜欢吃苹果",
        "我喜欢吃香蕉",
        "她喜欢吃葡萄",
        "他不喜欢吃香蕉",
        "他喜欢吃苹果",
        "她喜欢吃草莓"]
②分词
需要把每句话拆成一个个单词（token），这样我们才能进行条件概率的计算，这里我们简单以每个字作为一个token，

def tokenize(text):
    return [char for char in text]  # 将文本拆分为单字列表

# 对每个文本进行分词，并打印出对应的单字列表
print("单字列表:")
for text in corpus:
    tokens = tokenize(text)
    print(tokens)
③计算bigram的词频
# 定义计算N-Gram词频的函数
from collections import efaultdict, Counter
def count_ngrams(corpus, n):
    ngrams_count = defaultdict(Counter)  # 创建一个字典存储N-Gram计数
    for text in corpus:  # 遍历语料库中的每个文本
        tokens = tokenize(text)  # 对文本进行分词
        for i in range(len(tokens) - n + 1):  # 遍历分词结果生成N-Gram
            ngram = tuple(tokens[i:i+n])  # 创建一个N-Gram元组
            prefix = ngram[:-1]  # 获取N-Gram的前缀
            token = ngram[-1]  # 获取N-Gram的目标单字
            ngrams_count[prefix][token] += 1  # 更新N-Gram计数
    return ngrams_count
bigram_counts = count_ngrams(corpus, 2) # 计算Bigram词频
print("Bigram词频:") # 打印Bigram词频
for prefix, counts in bigram_counts.items():
    print("{}: {}".format("".join(prefix), dict(counts)))
④计算bigram概率
# 定义计算N-Gram概率的函数
def ngram_probabilities(ngram_counts):
    ngram_probs = defaultdict(Counter)
    for prefix, tokens_count in ngram_counts.items():  # 遍历N-Gram前缀
        total_count = sum(tokens_count.values())  # 计算当前前缀的N-Gram计数
        for token, count in tokens_count.items():  # 遍历每个前缀的N-Gram
            ngram_probs[prefix][token] = count / total_count  # 计算每个N-Gram概率
    return ngram_probs
bigram_probs = ngram_probabilities(bigram_counts) # 计算bigram概率
print("\nbigram概率:", bigram_probs) # 打印bigram概
⑤定义生成下一个词的函数
# 定义生成下一个词的函数
def generate_next_token(prefix, ngram_probs):
    if not prefix in ngram_probs:  # 如果前缀不在N-Gram中，返回None
        return None
    next_token_probs = ngram_probs[prefix]  # 获取当前前缀对应的下一个词的概率
    next_token = max(next_token_probs,
                     key=next_token_probs.get)  # 选择概率最大的词作为下一个词
    return next_token
⑥生成文本
# 定义生成连续文本的函数
def generate_text(prefix, ngram_probs, n, length=6):
    tokens = list(prefix)  # 将前缀转换为字符列表
    for _ in range(length - len(prefix)):  # 根据指定长度生成文本
        # 获取当前前缀对应的下一个词
        next_token = generate_next_token(tuple(tokens[-(n-1):]), ngram_probs)
        if not next_token: # 如果下一个词为None，跳出循环
            break
        tokens.append(next_token) # 将下一个词添加到生成的文本中
    return "".join(tokens) # 将字符列表连接成字符串
最终以“我”开头让模型写出一段话：

# 输入一个前缀，生成文本
generated_text = generate_text("我", bigram_probs, 2)
print("\n生成的文本:", generated_text)

模型生成的语句是：我喜欢吃苹果


## RNN的必要性

在时间序列数据中，当前的观察依赖于之前的观察，因此观察之间不是相互独立的。然而，传统的神经网络将每个观察视为独立的，这就导致了循环神经网络(RNN)的兴起，它通过包含数据点之间的依赖关系将记忆的概念引入神经网络。



但是RNN是如何实现这种记忆的呢？

RNN通过神经网络中的反馈回路实现记忆，这其实是RNN与传统神经网络的主要区别。反馈回路**允许信息在层内传递**，而前馈神经网络的信息仅在层之间传递。

为此，演化出了不同类型的RNN：

循环神经网络(RNN)
长短期记忆网络(LSTM)
门控循环单元网络(GRU)

本文将介绍RNN、LSTM和GRU的概念和异同点，以及它们的一些优点和缺点。

循环神经网络(RNN)
通过反馈回路，一个RNN单元的输出也被同一单元用作输入。因此，每个RNN都有两个输入：过去和现在。使用过去的信息会产生短期记忆。

为了更好地理解，可以展开RNN单元的反馈循环。展开单元格的长度等于输入序列的时间步数。

可以看到过去的观察结果是如何作为隐藏状态通过展开的网络传递的。在每个单元格中，当前时间步的输入、前一时间步的隐藏状态和偏置组合，然后通过激活函数限制以确定当前时间的隐藏状态步。

RNN可用于一对一、一对多、多对一和多对多预测。

### RNN的优点

由于其短期记忆，RNN可以处理顺序数据并识别历史数据中的模式。此外，RNN能够处理不同长度的输入。

### RNN的缺点

RNN存在梯度下降消失的问题。在这种情况下，用于在反向传播期间更新权重的梯度变得非常小。将权重与接近于零的梯度相乘会阻止网络学习新的权重。停止学习会导致RNN忘记在较长序列中看到的内容。梯度下降消失的问题随着网络层数的增加而增加。

由于RNN仅保留最近的信息，所以该模型在考虑过去的观察时会出现问题。因此，RNN只有短期记忆而没有长期记忆。

此外，由于RNN使用反向传播及时更新权重，网络也会遭受梯度爆炸的影响，如果使用ReLu激活函数，则会受到死亡ReLu单元的影响。前者可能会导致收敛问题，而后者会导致停止学习。

## 长短期记忆(LSTM)

长短期记忆网络（LSTM）是一种 RNN 变体，使模型能够扩展其内存容量，适应更长的时间线需要。RNN 只能记住近期输入。无法使用来自前几个序列的输入来改善其预测。[3]

LSTM的关键是单元状态，它从单元的输入传递到输出。单元状态允许信息沿着整个链流动，仅通过三个门进行较小的线性动作。因此，单元状态代表LSTM的长期记忆。这三个门分别称为遗忘门、输入门和输出门。这些门用作过滤器并控制信息流并确定保留或忽略哪些信息。

遗忘门决定了应该保留多少长期记忆。为此，使用了一个sigmoid函数来说明单元状态的重要性。输出在0和1之间变化，0即不保留任何信息；1则保留单元状态的所有信息。

输入门决定将哪些信息添加到单元状态，从而添加到长期记忆中。

输出门决定单元状态的哪些部分构建输出。因此，输出门负责短期记忆。

总的来说，状态通过遗忘门和输入门更新。

TODO:

### 优缺点

- 优点：类似于RNN，主要优点是它们可以捕获序列的长期和短期模式。因此，它们是最常用的RNN。
- 缺点：由于结构更复杂，LSTM的计算成本更高，从而导致训练时间更长。由于LSTM还使用时间反向传播算法来更新权重，因此LSTM存在反向传播的缺点，如死亡ReLu单元、梯度爆炸等。

## 门控循环单元(GRU)

门控循环单元（GRU）是支持选择性内存保留的 RNN。该模型添加了更新，并遗忘了其隐藏层的门，隐藏层可以在内存中存储或删除信息。[3]

与LSTM类似，GRU解决了简单RNN的梯度消失问题。然而，与LSTM的不同之处在于GRU使用较少的门并且没有单独的内部存储器，即单元状态。因此，GRU完全依赖隐藏状态作为记忆，从而导致更简单的架构。

重置门负责短期记忆，因为它决定保留和忽略多少过去的信息。

更新门负责长期记忆，可与LSTM的遗忘门相媲美。

当前时间步的隐藏状态是基于两个步骤确定的：

首先，确定候选隐藏状态。候选状态是当前输入和前一时间步的隐藏状态以及激活函数的组合。前一个隐藏状态对候选隐藏状态的影响由重置门控制。

第二步，将候选隐藏状态与上一时间步的隐藏状态相结合，生成当前隐藏状态。先前的隐藏状态和候选隐藏状态如何组合由更新门决定。

如果更新门给出的值为0，则完全忽略先前的隐藏状态，当前隐藏状态等于候选隐藏状态。如果更新门给出的值为1，则相反。

GRU的优势：由于与LSTM相比有着更简单的架构，GRU的计算效率更高，训练速度更快，只需要更少的内存。此外，GRU已被证明对于较小的序列更有效。

GRU的缺点：由于GRU没有单独的隐藏状态和细胞状态，因此它们可能无法像LSTM那样考虑过去的观察结果。与RNN和LSTM类似，GRU也可能遭受反向传播及时更新权重的缺点，即死亡ReLu单元、梯度爆炸。


[1]: https://zhuanlan.zhihu.com/p/147322049
[2]: https://developer.aliyun.com/article/174256
[3]: https://aws.amazon.com/cn/what-is/recurrent-neural-network/

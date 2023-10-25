#

## Bert与GPT

Bert与GPT都是基于Transformer架构的深度学习网络，区别在于Bert使用了Transfomer架构中Encoder模块进行预训练，而GPT利用了Transfomer中的Decoder模块进行预训练：

Bert是对输入文本进行，类似完形填空的过程，比如The __ sat on the mat，让算法学习被遮住cat单词的信息。而GPT则是训练生成器Decoder部分，让模型续写The cat sat on __ 生成后面的内容。

在Bert训练过程中，Bert是将文本内容进行随机遮挡来训练「完形填空」的能力，但是因为能看到后文的内容，因此对预测能力的训练不是非常有利，但是对文本理解和分类有着非常好的效果，这使得Bert经过微调后能够快速适应多种下游任务。而GPT使用的Masked Self-Attention是对后面遮住的部分进行生成训练，模型在生成文本时能够使用当前位置之前的文本信息进行预测，通过Attention机制更好地保留上下文信息并提高生成文本的连贯性。
在GPT训练参数规模上升之前，业界更看好Bert这种训练方法。但是GPT参数规模上升后涌现出来的few-shots能力让它也有了快速适应下游任务能力，再加之优秀的文本生成能力，以及添加了强化学习之后的ChatGPT的惊艳效果，使得GPT快速抢过了Bert的风头。

[2]:https://juejin.cn/post/7218048201982787645


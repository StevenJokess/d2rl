

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-11-06 06:47:28
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-11-06 06:52:49
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# GPT3

ChatGPT 的爆火让很多 NLPer 大吃一惊，焦虑感爆棚，它的思路和方法都不复杂，但效果却出奇的好。我想任何研究成果的爆发都不可能是一蹴而就的，期间必然包含这一系列小的创新和优化。于是，重新把 GPT3 的 Paper 拉出来读了一遍，重点关注了实验结果之外的东西，果然发现不少细节。因此，本文还是以 GPT3 为主。

从微调到In-Context Learning
GPT3 的初衷再次一看，其实非常 make sense：预训练时代 NLP 的典型方法是在不同下游任务上进行微调（fine-tuning），但是，人类只需要几个示例或简单的说明就可以执行新的任务。这里面潜在的意思是，大语言模型内部其实已经学到了所有这些能力，我们要做的是「合理」地去挖掘这些能力，而不是给一大批数据让它重新学习（fine-tuning）。这样做的原因和意义包括：

每个新任务需要一批标注数据，这就限制了语言模型应用范围。
预训练-微调机制本身的问题：预训练模型时在大范围内获取信息，微调时则在非常窄的任务上。这种虚假相关性随着模型表现力和训练分布的狭窄而增长。也就是说，模型能力很强，但被卡在特别窄的任务上，这种强其实是一种被放大的强，其泛化性可能很差【1】【2】。
人类不需要一大批监督数据学习大多数的语言任务，我们往往会将多个任务和技能无缝混合或自由切换。
这里有两个概念要先澄清一下：

Meta-Learning：训练时开发了一系列广泛的技能和模式识别能力，推理时使用这些能力快速适应或识别所需的任务。
In-Context Learning：使用预训练语言模型的文本输入作为任务范式的一种形式：以自然语言指令（instruction）或任务的一些样例为条件，期望通过预测接下来会发生什么来完成任务的下一个实例。
这里有个图片：



Meta-Learning 捕获通用方法的内循环/外循环结构，而 In-Context Learning 指代 Meta-Learning 的内循环。

而之所以有 GPT3 这篇 paper 就是因为这种 In-Context 的能力还比较差，离微调这种范式还有一段距离。但是他们又坚信这种 In-Context 的范式是值得探究的（上面提到的原因），这才一直沿着这条道一路走了下去。

还有个很重要的点需要强调——模型规模，有研究【3】表明，模型规模和效果有相关性（大模型有大智慧），GPT3 验证了这个假设。

对于 In-Context Learning 的验证，使用了三种不同的 condition：

- Few-Shot Learning：10-100 个样例（N=2048）作为上下文
- One-Shot Learning：一个样例
- Zero-Shot Learning：没有样例，只有一个自然语言指令（instruction）

效果如下：



而且，模型大小和上下文中样例数量的一般趋势适用于大多数任务。更大的模型是更熟练的元学习者。

更加可喜的是，在 Zero-Shot 和 One-Shot 下效果都不错，Few-Shot 下甚至偶尔能超过 SOTA。One-Shot 和 Few-Shot 也显示出了快速的适应或即时推理能力。当然，也有一些 GPT3 + Few-Shot 都搞不定的情况，比如 NLI 和一些阅读理解任务。

除此之外，文章还研究了数据污染问题，偏见、公平性以及广泛的社会影响。

方法和数据
关于方法，主要是前面提到的三种设置，文章给了一个非常直观的样例：



对此不再赘述，不过有一点比较有意思，就是与人类学习的细微差别：人一般会通过问一些问题来确认或进一步确认他们已经理解了要回答的问题，一般是以一种交互的方式；而 GPT3 是通过一些样例来指导模型（或者可以理解为让模型先了解再判断）。它们之间的根本区别可以简单的概括为：人类以理解（推理）为核心，而 GPT3 以表征为核心。当然，你也可以认为后者也是一种「理解」。

接下来是数据，这里最核心的是一个提高数据质量的操作，共包括三步：

根据一系列高质量参考语料库的相似性下载并过滤 CommonCrawl，结果就是 45TB 过滤到 570GB。
在数据集内和数据集之间的文档级别进行模糊重复数据删除（去掉了 10%），防止冗余，并保证衡量过拟合的验证集的真实性。
将已知的高质量参考语料库添加到训练数据集合中，增强 CommonCrawl 并增加多样性。
最终数据集如下：



值得注意的是它的采样策略，并不是根据不同数据集的 size 进行采样，而是根据数据质量——数据质量越高的数据集越有可能被采样到。比如 CommonCrawl 和 Book2 的数据有可能采样不到一次（不一定全部被使用），但其他数据集可能被采样 2-3 次。

局限和影响
局限

局限这一部分篇幅不长但非常精彩，尤其是有了 ChatGPT 再回看时，建议阅读原文。

首先，就强调了它的不足性，尤其对「常识物理学」，比如：“如果把奶酪放到冰箱会不会融化？”事实上，这也是整个 AI 面临的难题。哲学家也对此进行过研究，比如维特根斯坦在《论确定性》中谈及的信念网分布问题，可以参考这篇笔记，大概意思是说，AI 擅长的是人类认为的复杂问题（也就是《思考，快与慢》中说的系统二），最不擅长的反而是人类看起来很简单，甚至没有意识到的问题（比如 “我有两只手”）。

第二，GPT3 在结构和算法上的局限性，主要是没有用到双向结构，或其他训练目标（如去噪）。

第三，受预训练目标的限制。即自监督预测已经到极限了，新的方法迫在眉睫。未来的方向包括：从人类学习目标函数【4】（这是 OpenAI 的文章）、强化学习微调或多模态（比如更好的世界模型）【5】。其实，OpenAI 在从人类学习这方面除了【4】还有不少研究，比如 2021 年的【6】和 WebGPT【7】，2022 年的 InstructGPT【8】和【9】。

第四，预训练时的低效率采样——相比人类，样本还是太多，尤其和人类学习相比。未来的一个重要方向是预训练采样的效率，可能来自物理世界的基础知识提供额外信息，或算法改进。

第五，不确定 Few-Shot 是不是在推理时学习到新的任务，还是识别出来了在训练时学到的任务。最终，甚至不清楚人类从零开始学习与从之前的样本中学习分别学到什么。准确理解 Few-Shot 的工作原理是一个未来的方向。

第六，贵而且不方便进行推理，在实际应用中可能是个问题。知识蒸馏在如此规模上进行也可能是新的挑战和机会。

最后，是深度学习的通病——结果不容易解释，而且还记住了训练数据上的偏见，这也是种瓜得瓜种豆得豆。

可以看出这部分内容提到的一些方向正是未来 ChatGPT 成功的关键，真的是看准一条道坚持走下去的典范。

影响

这部分内容主要包括三个：语言模型的滥用、偏见以及能耗，是 LLM 的通用问题了。

对于滥用，最突出的就是造假和欺骗，很多欺诈应用依赖人类写的高质量文本，现在这个瓶颈被 LLM 直接给填平了，这不知道算不算一个小小的潘多拉魔盒。这里面尤其是有组织的团体对 LLM 的恶意使用。

偏见主要包括：性别、种族和区域，小模型比大模型更严重。这里最主要的是要干预，这篇 Paper 未提及的去偏综述可以略作参考。不过文章有一点强调值得注意：去偏不应以指标为导向，而应该以整体方式进行。关于如何分析偏见（以及对应的规范），可以参考【10】。

关于能耗，这里提供了另一个视角：不仅考虑预训练，还应该考虑模型生命周期内的摊销。这玩意儿训练时是比较费电，不过一旦训练好使用起来成本就非常低了（边际成本几乎为 0）。

小结和展望

本文主要回顾了一下 GPT3，当时看这篇 Paper 并没有意识到其中 In-Context 的威力（谁让它当时效果一般呢，看来以后看文章不能光看在公开数据集上的效果），而且这方面其实是有研究的，比如 Facebook 的 MetaICL，OpenAI 厉害的地方是坚持这条路并一直想方设法优化（上面的《局限》小节）。除了 In-Context Learning，语料过滤和去重也是值得注意的两个点，具体参见论文附录 A。其中，过滤用了一个逻辑回归的二分类器，去重则用了 MinHashLSH，两个地方使用了相同的特征。再就是局限部分，体现了对 AI 认知的全面和深度，令读者受益匪浅。

[1]: https://yam.gift/2023/01/20/NLP/2023-01-20-GPT3/

## 附录：GPT-3: Language Models are Few-Shot Learners论文阅读

Abstract
Recent work has demonstrated substantial gains on many NLP tasks and benchmarks by pre-training on a large corpus of text followed by fine-tuning on a specific task. While typically task-agnostic in architecture, this method still requires task-specific fine-tuning datasets of thousands or tens of thousands of examples. By contrast, humans can generally perform a new language task from only a few examples or from simple instructions – something which current NLP systems still largely struggle to do. Here we show that scaling up language models greatly improves task-agnostic, few-shot performance, sometimes even reaching competitiveness with prior state-of-the-art finetuning approaches. Specifically, we train GPT-3, an autoregressive language model with 175 billion parameters, 10x more than any previous non-sparse language model, and test its performance in the few-shot setting. For all tasks, GPT-3 is applied without any gradient updates or fine-tuning, with tasks and few-shot demonstrations specified purely via text interaction with the model. GPT-3 achieves strong performance on many NLP datasets, including translation, question-answering, and cloze tasks, as well as several tasks that require on-the-fly reasoning or domain adaptation, such as unscrambling words, using a novel word in a sentence, or performing 3-digit arithmetic. At the same time, we also identify some datasets where GPT-3’s few-shot learning still struggles, as well as some datasets where GPT-3 faces methodological issues related to training on large web corpora. Finally, we find that GPT-3 can generate samples of news articles which human evaluators have difficulty distinguishing from articles written by humans. We discuss broader societal impacts of this finding and of GPT-3 in general.

最近的工作表明，通过对大量文本语料库进行预训练，然后对特定任务进行微调，许多 NLP 任务和基准测试取得了实质性进展。 虽然在体系结构中通常与任务无关，但此方法仍然需要特定于任务的微调数据集，其中包含数千或数万个示例。 相比之下，人类通常只能通过几个例子或简单的指令来执行一项新的语言任务——这是当前的 NLP 系统在很大程度上仍然难以做到的。 在这里，我们展示了扩大语言模型极大地提高了与任务无关的、小样本的性能，有时甚至可以与先前最先进的微调方法竞争。 具体来说，我们训练了 GPT-3，这是一种具有 1750 亿个参数的自回归语言模型，比之前的任何非稀疏语言模型多 10 倍，并在少样本设置中测试其性能。 对于所有任务，GPT-3 都在没有任何梯度更新或微调的情况下应用，任务和小样本演示完全通过与模型的文本交互来指定。 GPT-3 在许多 NLP 数据集上实现了强大的性能，包括翻译、问答和完形填空任务，以及一些需要即时推理或领域适应的任务，例如解读单词，在句子中使用一个新单词，或执行 3 位数算术。 同时，我们还确定了一些 GPT-3 的少样本学习仍然困难的数据集，以及一些 GPT-3 面临与大型网络语料库训练相关的方法论问题的数据集。 最后，我们发现 GPT-3 可以生成人类评估者难以区分与人类撰写的文章的新闻文章样本。 我们总体上讨论了这一发现和 GPT-3 的更广泛的社会影响。

结论
结构：与GPT-2相同的模型和架构，基于Transformers的堆叠；

模型大小：从125M 到 175B

训练语料：对近万亿个单词的 Common Crawl 数据集进行清洗和提高质量

数据量：GPT-2 tokenizer，300B个tokens

训练效率：GPT-3 3B 需 50 petaflop/s-days的计算量，GPT-3 175B 需数千petaflop/s-days;

能力：算术，新闻文章生成，学习和使用新词，纠正英语语法，翻译等

开源代码和模型：官方未开源

paddle: 开源的最大模型只有2.6B的；175B的gpt-3需要从头训练；
达摩院：开源了1.3B/2.7B/13B等；
hf: 开源的gpt-2模型为1.5B参数；
智源：
提高数据集的平均质量：

模糊去重，在数据集内和跨数据集
增加其多样性
1 Introduction
Recent years have featured a trend towards pre-trained language representations in NLP systems, applied in increasingly flexible and task-agnostic ways for downstream transfer. First, single-layer representations were learned using word vectors [MCCD13, PSM14] and fed to task-specific architectures, then RNNs with multiple layers of representations and contextual state were used to form stronger representations [DL15, MBXS17, PNZtY18] (though still applied to task-specific architectures), and more recently pre-trained recurrent or transformer language models [VSP+17] have been directly fine-tuned, entirely removing the need for task-specific architectures [RNSS18, DCLT18, HR18].

近年来，在 NLP 系统中出现了一种趋势，即在 NLP 系统中预训练语言表示，以越来越灵活和与任务无关的方式应用于下游传输。 首先，使用词向量 [MCCD13、PSM14] 学习单层表示并将其馈送到特定于任务的架构，然后使用具有多层表示和上下文状态的 RNN 来形成更强的表示 [DL15、MBXS17、PNZtY18]（尽管仍然 适用于特定任务的架构），最近预训练的循环或转换器语言模型 [VSP+17] 已经直接微调，完全消除了对特定任务架构的需要 [RNSS18、DCLT18、HR18]。

Figure 1.1: Language model meta-learning. During unsupervised pre-training, a language model develops a broad set of skills and pattern recognition abilities. It then uses these abilities at inference time to rapidly adapt to or recognize the desired task. We use the term “in-context learning” to describe the inner loop of this process, which occurs within the forward-pass upon each sequence. The sequences in this diagram are not intended to be representative of the data a model would see during pre-training, but are intended to show that there are sometimes repeated sub-tasks embedded within a single sequence.

图 1.1：语言模型元学习。 在无监督预训练期间，语言模型会发展出广泛的技能和模式识别能力。 然后它在推理时使用这些能力来快速适应或识别所需的任务。 我们使用术语“上下文学习”来描述这个过程的内部循环，它发生在每个序列的前向传递中。 此图中的序列并不是为了代表模型在预训练期间会看到的数据，而是为了表明有时会在单个序列中嵌入重复的子任务。

Figure 1.2: Larger models make increasingly efficient use of in-context information. We show in-context learning performance on a simple task requiring the model to remove random symbols from a word, both with and without a natural language task description (see Sec. 3.9.2). The steeper “in-context learning curves” for large models demonstrate improved ability to learn a task from contextual information. We see qualitatively similar behavior across a wide range of tasks.

图 1.2：模型越大，上下文信息的使用效率就越高。 我们展示了一项简单任务的上下文学习性能，该任务要求模型从单词中删除随机符号，无论是否有自然语言任务描述（参见第 3.9.2 节）。 大型模型的更陡峭的“上下文学习曲线”表明从上下文信息中学习任务的能力有所提高。 我们在广泛的任务中看到了性质相似的行为。

In the context of language models this has sometimes been called “zero-shot transfer”, but this term is potentially ambiguous: the method is “zero-shot” in the sense that no gradient updates are performed, but it often involves providing inference-time demonstrations to the model, so is not truly learning from zero examples. To avoid this confusion, we use the term “meta-learning” to capture the inner-loop / outer-loop structure of the general method, and the term “in context-learning” to refer to the inner loop of meta-learning. We further specialize the description to “zero-shot”, “one-shot”, or “few-shot” depending on how many demonstrations are provided at inference time. These terms are intended to remain agnostic on the question of whether the model learns new tasks from scratch at inference time or simply recognizes patterns seen during training – this is an important issue which we discuss later in the paper, but “meta-learning” is intended to encompass both possibilities, and simply describes the inner-outer loop structure.

在语言模型的上下文中，这有时被称为“零样本迁移”，但这个术语可能有歧义：该方法在不执行梯度更新的意义上是“零样本”，但它通常涉及提供推理- 模型的时间演示，所以并不是真正从零示例中学习。 为了避免这种混淆，我们使用术语“元学习”来描述一般方法的内循环/外循环结构，使用术语“上下文学习”来指代元学习的内循环。 我们进一步将描述专门化为“零样本”、“单样本”或“少量样本”，具体取决于在推理时提供了多少演示。 这些术语旨在对模型是在推理时从头开始学习新任务还是简单地识别训练期间看到的模式这一问题保持不可知——这是一个重要的问题，我们将在本文后面讨论，但“元学习”是 旨在涵盖这两种可能性，并简单地描述了内-外循环结构。

Figure 1.3: Aggregate performance for all 42 accuracy-denominated benchmarks While zero-shot performance improves steadily with model size, few-shot performance increases more rapidly, demonstrating that larger models are more proficient at in-context learning. See Figure 3.8 for a more detailed analysis on SuperGLUE, a standard NLP benchmark suite.

图 1.3：所有 42 个以准确度为基准的基准的综合性能，虽然零样本性能随着模型大小的增加而稳步提高，但少样本性能增加得更快，这表明更大的模型更擅长上下文学习。 有关标准 NLP 基准套件 SuperGLUE 的更详细分析，请参见图 3.8。

In this paper, we test this hypothesis by training a 175 billion parameter autoregressive language model, which we call GPT-3, and measuring its in-context learning abilities. Specifically, we evaluate GPT-3 on over two dozen NLP datasets, as well as several novel tasks designed to test rapid adaptation to tasks unlikely to be directly contained in the training set. For each task, we evaluate GPT-3 under 3 conditions: (a) “few-shot learning”, or in-context learning where we allow as many demonstrations as will fit into the model’s context window (typically 10 to 100), (b) “one-shot learning”, where we allow only one demonstration, and (c) “zero-shot” learning, where no demonstrations are allowed and only an instruction in natural language is given to the model. GPT-3 could also in principle be evaluated in the traditional fine-tuning setting, but we leave this to future work.

在本文中，我们通过训练一个 1750 亿参数的自回归语言模型（我们称之为 GPT-3）并测量其上下文学习能力来检验这一假设。 具体来说，我们在超过两打 NLP 数据集以及旨在测试快速适应不太可能直接包含在训练集中的任务的几个新任务上评估 GPT-3。 对于每项任务，我们在 3 种条件下评估 GPT-3：(a) “少量学习”，或上下文学习，我们允许尽可能多的演示适合模型的上下文窗口（通常为 10 到 100），（ b) “one-shot learning”，我们只允许一次演示，以及 (c) “zero-shot”学习，不允许演示，只给模型一个自然语言指令。 GPT-3 原则上也可以在传统的微调设置中进行评估，但我们将其留待未来的工作。

2 Approach
Our basic pre-training approach, including model, data, and training, is similar to the process described in [RWC+19], with relatively straightforward scaling up of the model size, dataset size and diversity, and length of training. Our use of in-context learning is also similar to [RWC+19], but in this work we systematically explore different settings for learning within the context. Therefore, we start this section by explicitly defining and contrasting the different settings that we will be evaluating GPT-3 on or could in principle evaluate GPT-3 on. These settings can be seen as lying on a spectrum of how much task-specific data they tend to rely on. Specifically, we can identify at least four points on this spectrum (see Figure 2.1 for an illustration):

我们的基本预训练方法，包括模型、数据和训练，类似于 [RWC+19] 中描述的过程，模型大小、数据集大小和多样性以及训练长度的扩展相对简单。 我们对上下文学习的使用也类似于 [RWC+19]，但在这项工作中，我们系统地探索了在上下文中学习的不同设置。 因此，我们通过明确定义和对比我们将评估 GPT-3 或原则上可以评估 GPT-3 的不同设置来开始本节。 这些设置可以被视为取决于他们倾向于依赖多少特定于任务的数据的范围。 具体来说，我们可以在这个频谱上识别出至少四个点（参见图 2.1 的说明）：

Figure 2.1: Zero-shot, one-shot and few-shot, contrasted with traditional fine-tuning. The panels above show four methods for performing a task with a language model – fine-tuning is the traditional method, whereas zero-, one-, and few-shot, which we study in this work, require the model to perform the task with only forward passes at test time. We typically present the model with a few dozen examples in the few shot setting. Exact phrasings for all task descriptions, examples and prompts can be found in Appendix G.

图 2.1：零样本、单样本和少样本，与传统的微调对比。 上面的面板显示了使用语言模型执行任务的四种方法——微调是传统方法，而我们在这项工作中研究的零、一次和少镜头需要模型执行任务 仅在测试时向前传球。 我们通常在少数镜头设置中向模型展示几十个示例。 所有任务描述、示例和提示的准确措辞可在附录 G 中找到。


表 2.1：我们训练的模型的大小、架构和学习超参数（令牌的批量大小和学习率）。 所有模型都接受了总共 3000 亿个令牌的训练。
2.1 Model and Architectures
We use the same model and architecture as GPT-2 [RWC+19], including the modified initialization, pre-normalization, and reversible tokenization described therein, with the exception that we use alternating dense and locally banded sparse attention patterns in the layers of the transformer, similar to the Sparse Transformer [CGRS19]. To study the dependence of ML performance on model size, we train 8 different sizes of model, ranging over three orders of magnitude from 125 million parameters to 175 billion parameters, with the last being the model we call GPT-3. Previous work [KMH+20] suggests that with enough training data, scaling of validation loss should be approximately a smooth power law as a function of size; training models of many different sizes allows us to test this hypothesis both for validation loss and for downstream language tasks.

我们使用与 GPT-2 [RWC+19] 相同的模型和架构，包括其中描述的修改后的初始化、预归一化和可逆标记化，除了我们在 Transformer，类似于稀疏变压器[CGRS19]。 为了研究 ML 性能对模型大小的依赖性，我们训练了 8 种不同大小的模型，范围超过三个数量级，从 1.25 亿个参数到 1750 亿个参数，最后一个是我们称为 GPT-3 的模型。 之前的工作 [KMH+20] 表明，在有足够的训练数据的情况下，验证损失的缩放应该近似于作为大小函数的平滑幂律； 许多不同规模的训练模型使我们能够针对验证损失和下游语言任务来检验这一假设。

Table 2.1 shows the sizes and architectures of our 8 models. Here nparams is the total number of trainable parameters, nlayers is the total number of layers, dmodel is the number of units in each bottleneck layer (we always have the feedforward layer four times the size of the bottleneck layer, dff = 4 ∗ dmodel), and dhead is the dimension of each attention head. All models use a context window of nctx = 2048 tokens. We partition the model across GPUs along both the depth and width dimension in order to minimize data-transfer between nodes. The precise architectural parameters for each model are chosen based on computational efficiency and load-balancing in the layout of models across GPU’s. Previous work [KMH+20] suggests that validation loss is not strongly sensitive to these parameters within a reasonably broad range.

表 2.1 显示了我们的 8 个模型的大小和架构。 这里 n_params 是可训练参数的总数，n_layers 是总层数，d_model 是每个瓶颈层的单元数（我们总是有四倍于瓶颈层大小的前馈层，d_ff = 4 ∗ d_model） ，d_head是每个attention head的维度。 所有模型都使用 n_ctx = 2048 个标记的上下文窗口。 我们沿着深度和宽度维度跨 GPU 划分模型，以最大限度地减少节点之间的数据传输。 每个模型的精确架构参数是根据跨 GPU 的模型布局中的计算效率和负载平衡来选择的。 之前的工作 [KMH+20] 表明，验证损失在相当广泛的范围内对这些参数并不十分敏感。

2.2 Training Dataset
Datasets for language models have rapidly expanded, culminating in the Common Crawl dataset2 [RSR+19] constituting nearly a trillion words. This size of dataset is sufficient to train our largest models without ever updating on the same sequence twice. However, we have found that unfiltered or lightly filtered versions of Common Crawl tend to have lower quality than more curated datasets. Therefore, we took 3 steps to improve the average quality of our datasets: (1) we downloaded and filtered a version of CommonCrawl based on similarity to a range of high-quality reference corpora, (2) we performed fuzzy deduplication at the document level, within and across datasets, to prevent redundancy and preserve the integrity of our held-out validation set as an accurate measure of overfitting, and (3) we also added known high-quality reference corpora to the training mix to augment CommonCrawl and increase its diversity.

语言模型的数据集迅速扩展，最终形成了构成近万亿个单词的 Common Crawl 数据集 [RSR+19]。 这种大小的数据集足以训练我们最大的模型，而无需对同一序列进行两次更新。 但是，我们发现 Common Crawl 的未过滤或轻微过滤版本的质量往往低于经过精心策划的数据集。 因此，我们采取了 3 个步骤来提高数据集的平均质量：(1) 我们根据与一系列高质量参考语料库的相似性下载并过滤了一个版本的 CommonCrawl，(2) 我们在文档级别执行了模糊去重 ，在数据集内和跨数据集，以防止冗余并保持我们保留的验证集的完整性作为过度拟合的准确度量，并且（3）我们还在训练组合中添加了已知的高质量参考语料库以增强 CommonCrawl 并增加其 多样性。

Details of the first two points (processing of Common Crawl) are described in Appendix A. For the third, we added several curated high-quality datasets, including an expanded version of the WebText dataset [RWC+19], collected by scraping links over a longer period of time, and first described in [KMH+20], two internet-based books corpora (Books1 and Books2) and English-language Wikipedia.

附录 A 中描述了前两点（Common Crawl 的处理）的详细信息。对于第三点，我们添加了几个精选的高质量数据集，包括 WebText 数据集 [RWC+19] 的扩展版本，通过抓取链接收集 一段较长的时间，首先在 [KMH+20] 中描述，两个基于互联网的书籍语料库（Books1 和 Books2）和英语维基百科。

Table 2.2 shows the final mixture of datasets that we used in training. The CommonCrawl data was downloaded from 41 shards of monthly CommonCrawl covering 2016 to 2019, constituting 45TB of compressed plaintext before filtering and 570GB after filtering, roughly equivalent to 400 billion byte-pair-encoded tokens. Note that during training, datasets are not sampled in proportion to their size, but rather datasets we view as higher-quality are sampled more frequently, such that CommonCrawl and Books2 datasets are sampled less than once during training, but the other datasets are sampled 2-3 times. This essentially accepts a small amount of overfitting in exchange for higher quality training data.

表 2.2 显示了我们在训练中使用的最终混合数据集。 CommonCrawl 数据从 2016 年到 2019 年的 41 个每月 CommonCrawl 分片下载，构成过滤前 45TB 的压缩明文和过滤后的 570GB，大约相当于 4000 亿字节对编码的令牌。 请注意，在训练期间，数据集不会按其大小的比例进行采样，而是我们认为质量更高的数据集会被更频繁地采样，例如 CommonCrawl 和 Books2 数据集在训练期间被采样的次数少于一次，但其他数据集的采样次数为 2 -3次。 这本质上是接受少量的过拟合来换取更高质量的训练数据。


图 2.2：训练期间使用的总计算量。 基于神经语言模型的缩放定律 [KMH+20] 中的分析，我们用比典型情况少得多的标记训练了更大的模型。 因此，尽管 GPT-3 3B 几乎是 RoBERTa-Large（355M 参数）的 10 倍，但两个模型在预训练期间都花费了大约 50 petaflop/s-days 的计算量。 这些计算的方法可以在附录 D

表 2.2：用于训练 GPT-3 的数据集。 “训练组合中的权重”指的是训练期间从给定数据集中提取的示例部分，我们故意不使其与数据集的大小成比例。 因此，当我们训练 3000 亿个标记时，某些数据集在训练期间最多出现 3.4 次，而其他数据集出现次数不到一次。
A major methodological concern with language models pretrained on a broad swath of internet data, particularly large models with the capacity to memorize vast amounts of content, is potential contamination of downstream tasks by having their test or development sets inadvertently seen during pre-training. To reduce such contamination, we searched for and attempted to remove any overlaps with the development and test sets of all benchmarks studied in this paper. Unfortunately, a bug in the filtering caused us to ignore some overlaps, and due to the cost of training it was not feasible to retrain the model. In Section 4 we characterize the impact of the remaining overlaps, and in future work we will more aggressively remove data contamination.

在广泛的互联网数据上预训练语言模型的一个主要方法论问题，特别是具有记忆大量内容能力的大型模型，是下游任务的潜在污染，因为在预训练期间无意中看到了它们的测试或开发集。 为了减少这种污染，我们搜索并试图消除与本文研究的所有基准的开发和测试集的任何重叠。 不幸的是，过滤中的一个错误导致我们忽略了一些重叠，并且由于训练成本，重新训练模型是不可行的。 在第 4 节中，我们描述了剩余重叠的影响，在未来的工作中，我们将更积极地消除数据污染。

2.3 Training Process
As found in [KMH+20, MKAT18], larger models can typically use a larger batch size, but require a smaller learning rate. We measure the gradient noise scale during training and use it to guide our choice of batch size [MKAT18]. Table 2.1 shows the parameter settings we used. To train the larger models without running out of memory, we use a mixture of model parallelism within each matrix multiply and model parallelism across the layers of the network. All models were trained on V100 GPU’s on part of a high-bandwidth cluster provided by Microsoft. Details of the training process and hyperparameter settings are described in Appendix B.

正如 [KMH+20, MKAT18] 中所见，较大的模型通常可以使用较大的批量大小，但需要较小的学习率。 我们在训练期间测量梯度噪声尺度，并用它来指导我们选择批量大小 [MKAT18]。 表 2.1 显示了我们使用的参数设置。 为了在不耗尽内存的情况下训练更大的模型，我们在每个矩阵乘法中混合使用模型并行性和跨网络层的模型并行性。 所有模型都在 Microsoft 提供的高带宽集群的一部分上的 V100 GPU 上进行了训练。 附录 B 中描述了训练过程和超参数设置的详细信息。

2.4 Evaluation
For few-shot learning, we evaluate each example in the evaluation set by randomly drawing K examples from that task’s training set as conditioning, delimited by 1 or 2 newlines depending on the task. For LAMBADA and Storycloze there is no supervised training set available so we draw conditioning examples from the development set and evaluate on the test set. For Winograd (the original, not SuperGLUE version) there is only one dataset, so we draw conditioning examples directly from it.

对于小样本学习，我们通过从该任务的训练集中随机抽取 K 个示例作为条件来评估评估集中的每个示例，根据任务用 1 或 2 个换行符分隔。 对于 LAMBADA 和 Storycloze，没有可用的监督训练集，因此我们从开发集中抽取条件示例并在测试集上进行评估。 对于 Winograd（原始版本，而非 SuperGLUE 版本）只有一个数据集，因此我们直接从中提取条件示例。



6 Broader Impacts
6.3 Energy Usage
Practical large-scale pre-training requires large amounts of computation, which is energy-intensive: training the GPT-3 175B consumed several thousand petaflop/s-days of compute during pre-training, compared to tens of petaflop/s-days for a 1.5B parameter GPT-2 model (Figure 2.2). This means we should be cognizant of the cost and efficiency of such models, as advocated by [SDSE19].

实际的大规模预训练需要大量的计算，这是能源密集型的：训练 GPT-3 175B 在预训练期间消耗了数千 petaflop/s-days 的计算，相比之下，数十 petaflop/s-days 一个 1.5B 参数的 GPT-2 模型（图 2.2）。 这意味着我们应该认识到 [SDSE19] 所提倡的此类模型的成本和效率。

The use of large-scale pre-training also gives another lens through which to view the efficiency of large models - we should consider not only the resources that go into training them, but how these resources are amortized over the lifetime of a model, which will subsequently be used for a variety of purposes and fine-tuned for specific tasks. Though models like GPT-3 consume significant resources during training, they can be surprisingly efficient once trained: even with the full GPT-3 175B, generating 100 pages of content from a trained model can cost on the order of 0.4 kW-hr, or only a few cents in energy costs. Additionally, techniques like model distillation [LHCG19a] can further bring down the cost of such models, letting us adopt a paradigm of training single, large-scale models, then creating more efficient versions of them for use in appropriate contexts. Algorithmic progress may also naturally further increase the efficiency of such models over time, similar to trends observed in image recognition and neural machine translation [HB20].

大规模预训练的使用也提供了另一个视角来观察大型模型的效率——我们不仅应该考虑用于训练它们的资源，还应该考虑这些资源如何在模型的生命周期内摊销，这 随后将用于各种目的，并针对特定任务进行微调。 尽管像 GPT-3 这样的模型在训练期间会消耗大量资源，但一旦训练它们就会出奇地高效：即使使用完整的 GPT-3 175B，从经过训练的模型生成 100 页内容的成本约为 0.4 kW-hr，或者 能源成本只有几美分。 此外，模型蒸馏 [LHCG19a] 等技术可以进一步降低此类模型的成本，让我们采用训练单一、大规模模型的范例，然后创建它们的更高效版本以在适当的环境中使用。 随着时间的推移，算法的进步也可能自然地进一步提高此类模型的效率，类似于在图像识别和神经机器翻译 [HB20] 中观察到的趋势。

8 Conclusion
We presented a 175 billion parameter language model which shows strong performance on many NLP tasks and benchmarks in the zero-shot, one-shot, and few-shot settings, in some cases nearly matching the performance of state-of-the-art fine-tuned systems, as well as generating high-quality samples and strong qualitative performance at tasks defined on-the-fly. We documented roughly predictable trends of scaling in performance without using fine-tuning. We also discussed the social impacts of this class of model. Despite many limitations and weaknesses, these results suggest that very large language models may be an important ingredient in the development of adaptable, general language systems.

我们展示了一个 1750 亿参数的语言模型，它在零样本、单样本和少样本设置中的许多 NLP 任务和基准测试中表现出强大的性能，在某些情况下几乎与最先进的精细模型的性能相匹配 调优系统，以及在动态定义的任务中生成高质量样本和强大的定性性能。 我们记录了在不使用微调的情况下大致可预测的性能扩展趋势。 我们还讨论了此类模型的社会影响。 尽管有许多限制和弱点，但这些结果表明，非常大的语言模型可能是开发适应性强的通用语言系统的重要组成部分。

B Details of Model Training
To train all versions of GPT-3, we use Adam with β1 = 0.9, β2 = 0.95, and = 10−8 , we clip the global norm of the gradient at 1.0, and we use cosine decay for learning rate down to 10% of its value, over 260 billion tokens (after 260 billion tokens, training continues at 10% of the original learning rate). There is a linear LR warmup over the first 375 million tokens. We also gradually increase the batch size linearly from a small value (32k tokens) to the full value over the first 4-12 billion tokens of training, depending on the model size. Data are sampled without replacement during training (until an epoch boundary is reached) to minimize overfitting. All models use weight decay of 0.1 to provide a small amount of regularization [LH17].

为了训练所有版本的 GPT-3，我们使用 β1 = 0.9、β2 = 0.95 和 ξ= 10e−8 的 Adam，我们将梯度的全局范数剪裁为 1.0，并使用余弦衰减将学习率降至 其值的10%，超过 2600 亿个代币（在 2600 亿个代币之后，继续以原始学习率的 10% 进行训练）。 对前 3.75 亿个代币进行线性 LR 预热。 我们还根据模型大小，在训练的前 4-120 亿个令牌中，逐渐将批量大小从一个小值（32k 令牌）线性增加到完整值。 在训练期间（直到达到纪元边界）对数据进行无替换采样，以最大程度地减少过度拟合。 所有模型都使用 0.1 的权重衰减来提供少量正则化 [LH17]。

During training we always train on sequences of the full nctx = 2048 token context window, packing multiple documents into a single sequence when documents are shorter than 2048, in order to increase computational efficiency. Sequences with multiple documents are not masked in any special way but instead documents within a sequence are delimited with a special end of text token, giving the language model the information necessary to infer that context separated by the end of text token is unrelated. This allows for efficient training without need for any special sequence-specific masking.

在训练期间，我们总是在完整的 n_ctx = 2048 令牌上下文窗口的序列上进行训练，当文档短于 2048 时，将多个文档打包成一个序列，以提高计算效率。 具有多个文档的序列不会以任何特殊方式屏蔽，而是序列中的文档用特殊的文本标记结尾分隔，为语言模型提供必要的信息来推断由文本标记结尾分隔的上下文是不相关的。 这允许进行有效的训练，而无需任何特殊的特定于序列的掩蔽。

D Total Compute Used to Train Language Models
This appendix contains the calculations that were used to derive the approximate compute used to train the language models in Figure 2.2. As a simplifying assumption, we ignore the attention operation, as it typically uses less than 10% of the total compute for the models we are analyzing.

本附录包含用于推导用于训练图 2.2 中的语言模型的近似计算的计算。 作为一个简化的假设，我们忽略了注意力操作，因为对于我们正在分析的模型，它通常使用不到总计算量的 10%。

Calculations can be seen in Table D.1 and are explained within the table caption.

计算结果见表 D.1，并在表标题中进行了解释。


表 D.1：从右手边开始向左移动，我们从训练每个模型的训练标记的数量开始。 接下来我们注意到，由于 T5 使用编码器-解码器模型，因此在向前或向后传递期间，每个标记只有一半的参数是活动的。 然后我们注意到每个标记都涉及前向传递中每个活动参数的单个加法和单个乘法（忽略注意力）。 然后我们添加一
表 D.1：从右手边开始向左移动，我们从训练每个模型的训练标记的数量开始。 接下来我们注意到，由于 T5 使用编码器-解码器模型，因此在向前或向后传递期间，每个标记只有一半的参数是活动的。 然后我们注意到每个标记都涉及前向传递中每个活动参数的单个加法和单个乘法（忽略注意力）。 然后我们添加一个 3x 的乘数来解释向后传递（因为计算 ∂params/∂loss 和 ∂acts/∂loss 使用与前向传递相似的计算量。结合前两个数字，我们得到总失败次数 per parameter per token. 我们将这个值乘以总训练 token 和总参数，得到训练期间使用的总 flops 数。我们报告了 flops 和 petaflop/s-day（每个都是 2.88e+7 flops） .

[2]: https://zhuanlan.zhihu.com/p/606194698

1.思维链定义
背景



在 2017-2019 年之间，随着 Transformer 模型的提出，计算资源与大规模语料库不断出现，自然语言处理领域发生了翻天覆地的变化，传统的全监督学习的范式逐渐达到了瓶颈，很难在传统的训练方式上取得大幅度提升。这时大规模预训练模型的如 Bert、RoBERTa 等模型的出现使得研究方向转向了以预训练模型为基础 + 下游任务 Fine-tune 的范式。



然而随着语言模型规模的不断增大，Fine-tune 的成本变得越来越高，以 GPT-3 为例，其参数量已经达到了惊人的 175B，对于这样大规模的参数，仅依靠传统 Fine-Tune 已经很难对模型起到有效的迁移，且如此大规模的参数量使得梯度的反向传播的代价也急剧增加。在这样的背景下，提示学习应运而生。提示学习通过改造下游任务、增加专家知识等形式，使得目标任务的输入输出更加贴合原始语言模型训练时的数据。



2021 年，提示学习经历了以离散提示学习（提示词的组合）为开始，连续化提示学习（连续空间表示）为复兴的多个阶段，逐步达到高潮。但基于连续空间的提示学习同样存在较多的局限性，比如资源消耗与训练不稳定等多种问题。这一时期，虽然大多数研究者普遍认同提示学习将会带来自然语言处理领域下一代革命，但这一时期大多数研究工作主要还是与模型训练或新的语言模型结构相关。



直到 2022 年，大规模语言模型的效果 “肉眼可见” 的变好，同时随着模型规模的不断增大，模型也变得更好“提示”，尤其是之前一些没有办法做很好的任务不断取得突破。但是大模型在做算术推理、常识推理和符号推理时的表现还不够好。 大模型的 in-context few shot 能力是极强的，但是创建很多的中间步骤用来做监督 finetune 是非常耗时的，而且传统的 prompt 方式在数学计算、常识推理等做的又不好，怎么结合 in-context few shot 和 中间步骤来改善算术推理、常识推理和符号推理等能力是一个问题。思维链的一系列工作就是在这样的大环境下诞生的。



定义



思维链 (Chain-of-thought，CoT) 的概念是在 Google 的论文 "Chain-of-Thought Prompting Elicits Reasoning in Large Language Models" 中被首次提出。思维链（CoT）是一种改进的提示策略，用于提高 LLM 在复杂推理任务中的性能，如算术推理、常识推理和符号推理。CoT 没有像 ICL 那样简单地用输入输出对构建提示，而是结合了中间推理步骤，这些步骤可以将最终输出引入提示。简单来说，思维链是一种离散式提示学习，更具体地，大模型下的上下文学习（即不进行训练，将例子添加到当前样本输入的前面，让模型一次输入这些文本进行输出完成任务），相比于之前传统的上下文学习（即通过x1​,y1​,x2​,y2​,....xtest​作为输入来让大模型补全输出ytest​），思维链多了中间的中间的推导提示，以下图为例：






可以看到，类似的算术题，思维链提示会在给出答案之前，还会自动给出推理步骤：



“罗杰先有 5 个球，2 罐 3 个网球等于 6 个，5 + 6 = 11”



“食堂原来有 23 个苹果，用 20 个做午餐，23-20=3；又买了 6 个苹果，3+6=9”



思维链提示给出了正确答案，而直接给出答案的传统提示学习，结果是错的，连很基本的数学计算都做不好。简单来说，语言模型很难将所有的语义直接转化为一个方程，因为这是一个更加复杂的思考过程，但可以通过中间步骤，来更好地推理问题的每个部分。



一个有效的思维链应该具有以下特点：



逻辑性：思维链中的每个思考步骤都应该是有逻辑关系的，它们应该相互连接，从而形成一个完整的思考过程。

全面性：思维链应该尽可能地全面和细致地考虑问题，以确保不会忽略任何可能的因素和影响。

可行性：思维链中的每个思考步骤都应该是可行的，也就是说，它们应该可以被实际操作和实施。

可验证性：思维链中的每个思考步骤都应该是可以验证的，也就是说，它们应该可以通过实际的数据和事实来验证其正确性和有效性。

2.思维链用于上下文学习的方法(In-context learning)
2.1 Few-shot CoT
Few-shot CoT 是 ICL 的一种特殊情况，它通过融合 CoT 推理步骤，将每个演示〈input，output〉扩充为〈input，CoT，output〉。



【CoT prompt 的设计】

作为一种直接的方法，研究表明，使用不同的 CoT（即每个问题的多个推理路径）可以有效地提高它们的性能。

另一个直观的想法是，具有更复杂推理路径的提示更有可能引发 LLM 的推理能力，这可以导致生成正确答案的准确性更高。然而，这两种方法都依赖于带标注的 CoT 数据集，这限制了在实践中的应用。为了克服这一限制，Auto-CoT 建议利用 Zero-shot-CoT，通过专门提示 LLM 来生成 CoT 推理路径，从而消除了手动操作。为了提高性能，Auto-CoT 进一步将训练集中的问题划分为不同的聚类，然后选择最接近每个聚类中心的问题，这应该很好地代表训练集中的提问。尽管 Few-shot CoT 可以被视为 ICL 的一种特殊提示情况，但与 ICL 中的标准提示相比，演示的顺序似乎影响相对较小：在大多数任务中，重新排序演示只会导致小于 2% 的性能变化。

【增强的 CoT 策略】

除了丰富上下文信息外，CoT 提示还提供更多选项来推断给定问题的答案。现有的研究主要集中在生成多条推理路径，并试图在得出的答案中找到共识。例如，在生成 CoT 和最终答案时，提出了 self-consistency 作为一种新的解码策略。它首先生成几个推理路径，然后对所有答案进行综合（例如，通过在这些路径中投票来选择最一致的答案）。self-consistency 在很大程度上提高了 CoT 推理的性能，甚至可以改进一些 CoT 提示通常比标准提示差的任务。此外，将自一致性策略扩展到更通用的集成框架（扩展到提示上的集成），发现不同的推理路径是提高 CoT 推理性能的关键。

2.2 Zero-shot CoT
与 Few-shot CoT 不同，Zero-shot CoT 在 prompt 中不包括人工标注的任务演示。相反，它直接生成推理步骤，然后使用生成的 CoT 来导出答案。其中 LLM 首先由 “Let's think step by step” 提示生成推理步骤，然后由 “Therefore, the answer is” 提示得出最终答案。他们发现，当模型规模超过一定规模时，这种策略会大大提高性能，但对小规模模型无效，显示出显著的涌现能力模式。



为了在更多的任务上解锁 CoT 能力，Flan-T5 和 Flan-PaLM 进一步在 CoT 标注上执行指令调优，并且改进了在不可见任务上的零样本性能。

3. 结论
CoT 对小模型作用不大，模型参数至少达到 10B 才有效果，达到 100B 效果才明显。并且，从小模型的输出可以看出，它们大部分是输出了流畅但不合逻辑的 CoT，因此得到错误的结果。

CoT 对复杂的问题的性能增益更大，例如 GSM8K（更难，因为基线最低）上 GPT-3 和 PaLM 的性能增加了一倍多。而对于 MAWPS-SingleOp（更简单的任务），性能改进非常小甚至是负面的。

加上 CoT 的 PaLM 540B 超过了任务特定的用监督学习训练的模型的最优结果。不加 CoT 的话 GSM8K 和 MAWPS 任务上 LLM 的结果比不过最优的监督学习模型。



思维链是解决推理任务时人类思维过程遵循的一系列典型步骤。 它可以帮助我们将一个问题分解成一系列的子问题，然后逐个解决这些子问题，从而得出最终的答案。在大型语言模型中，思维链可以用来引出推理。思路链方法带来以下好处：



CoT 允许模型将多步推理问题分解为中间步骤，这意味着额外的计算可以分配到需要推理的复杂问题上；

CoT 使大语言模型更具可解释性，更加可信，并提供了调试推理路径错误的机会；

CoT 推理能够被用于数学应用题、常识推理和符号操作等任务，并且可能适用任何人类需要通过语言解决的问题；

CoT 可以通过将其加入到 few-shot prompting 示例中，从而在足够大的语言模型中引导出推理能力。



当前的思维链也存在着许多局限性：



首先，尽管设计的思维链是在模拟人类的推理过程，但模型是否真正的学会了推理仍需进一步进行验证。

人工设计思维链仍然是代价过大，大规模的人工标注思维链是不可行的。

思维链只在大规模模型上有效（10B 以上）

4.未来对思维链的思考
（1）什么时候 CoT 对 LLMs 有用



由于 CoT 是一种涌现能力，只对足够大的模型（例如，通常包含 10B 或更多的参数）有积极影响，但对小模型没有影响。此外，由于 CoT 通过中间推理步骤增强了标准提示，因此它主要有效地改进了需要逐步推理的任务，如算术推理、常识推理和符号推理。然而，对于不依赖于复杂推理的其他任务，它可能显示出比标准提示更差的性能，例如 GLUE 的 MNLI-m/mm、SST-2 和 QQP。



（2）为什么 LLMs 可以执行 CoT 推理



关于 CoT 能力的来源，人们普遍假设它可以归因于对代码的训练，因为在代码上训练的模型显示出强大的推理能力。从直觉上讲，代码数据通过算法逻辑和编程流程进行了良好的组织，这可能有助于提高 LLM 的推理性能。然而，这一假设仍然缺乏消融实验的公开报道证据。此外，指令调优似乎不是获得 CoT 能力的关键原因，因为经验表明，对非 CoT 数据的指令调优并不能提高保持的 CoT 基准的性能。



总之，CoT 提示为诱导 LLM 的推理能力提供了一种通用而灵活的方法。也有一些初步尝试将该技术扩展到解决多模态任务和多语言任务。除了将 LLM 与 ICL 和 CoT 直接结合使用外，最近的一些研究还探讨了如何将 LLM 的能力专门化到特定任务，这被称为模型专门化。例如，研究人员通过微调 LLM 生成的 CoT 推理路径上的小规模 Flan-T5，专门研究 LLM 的数学推理能力。模型专业化也可用于解决各种任务，如问答、代码合成和信息检索。

5.关键知识点
有效的思维链应具备的特点是：逻辑性、全面性、可行性

思维链只能在大语言模型中起作用。

Few-shot CoT 是 ICL 的一种特殊情况。

Zero-shot CoT 在 prompt 中不包括人工标注的任务演示。

CoT 使大语言模型更具可解释性，更加可信。[1]



[1]: https://xie.infoq.cn/article/736cd66decbb874093b9c1b61
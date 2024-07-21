# Llama

## Llama3

仓库：https://github.com/meta-llama/llama3

今天，我们推出 Meta Llama 3，这是我们最先进的开源大型语言模型的下一代。
Llama 3 models will soon be available on AWS, Databricks, Google Cloud, Hugging Face, Kaggle, IBM WatsonX, Microsoft Azure, NVIDIA NIM, and Snowflake, and with support from hardware platforms offered by AMD, AWS, Dell, Intel, NVIDIA, and Qualcomm.
Llama 3型号将很快在AWS，Databricks，Google Cloud，Hugging Face，Kaggle，IBM WatsonX，Microsoft Azure，NVIDIA NIM和Snowflake上提供，并得到AMD，AWS，戴尔，英特尔，英伟达和高通提供的硬件平台的支持。
We’re dedicated to developing Llama 3 in a responsible way, and we’re offering various resources to help others use it responsibly as well. This includes introducing new trust and safety tools with Llama Guard 2, Code Shield, and CyberSec Eval 2.
我们致力于以负责任的方式开发 Llama 3，并提供各种资源来帮助其他人负责任地使用它。这包括通过 Llama Guard 2、Code Shield 和 CyberSec Eval 2 引入新的信任和安全工具。
In the coming months, we expect to introduce new capabilities, longer context windows, additional model sizes, and enhanced performance, and we’ll share the Llama 3 research paper.
在接下来的几个月里，我们预计将引入新的功能、更长的上下文窗口、额外的模型大小和增强的性能，我们将分享 Llama 3 的研究论文。
Meta AI, built with Llama 3 technology, is now one of the world’s leading AI assistants that can boost your intelligence and lighten your load—helping you learn, get things done, create content, and connect to make the most out of every moment. You can try Meta AI here.
Meta AI 采用 Llama 3 技术构建，现已成为世界领先的 AI 助手之一，可以提高您的智力并减轻您的负担——帮助您学习、完成工作、创建内容和建立联系，以充分利用每一刻。您可以在此处试用 Meta AI。

Today, we’re excited to share the first two models of the next generation of Llama, Meta Llama 3, available for broad use. This release features pretrained and instruction-fine-tuned language models with 8B and 70B parameters that can support a broad range of use cases. This next generation of Llama demonstrates state-of-the-art performance on a wide range of industry benchmarks and offers new capabilities, including improved reasoning. We believe these are the best open source models of their class, period. In support of our longstanding open approach, we’re putting Llama 3 in the hands of the community. We want to kickstart the next wave of innovation in AI across the stack—from applications to developer tools to evals to inference optimizations and more. We can’t wait to see what you build and look forward to your feedback.
今天，我们很高兴与大家分享下一代 Llama 的前两个型号 Meta Llama 3，可供广泛使用。此版本具有预训练和指令微调的语言模型，具有 8B 和 70B 参数，可支持广泛的用例。下一代 Llama 在广泛的行业基准测试中展示了最先进的性能，并提供了新功能，包括改进的推理。我们相信这些是同类产品中最好的开源模型。为了支持我们长期以来的开放方法，我们将 Llama 3 交到社区手中。我们希望在整个堆栈中启动下一波 AI 创新浪潮——从应用程序到开发人员工具，从评估到推理优化等等。我们迫不及待地想看看您构建的内容，并期待您的反馈。

Our goals for Llama 3
我们对 Llama 3 的目标


With Llama 3, we set out to build the best open models that are on par with the best proprietary models available today. We wanted to address developer feedback to increase the overall helpfulness of Llama 3 and are doing so while continuing to play a leading role on responsible use and deployment of LLMs. We are embracing the open source ethos of releasing early and often to enable the community to get access to these models while they are still in development. The text-based models we are releasing today are the first in the Llama 3 collection of models. Our goal in the near future is to make Llama 3 multilingual and multimodal, have longer context, and continue to improve overall performance across core LLM capabilities such as reasoning and coding.
在 Llama 3 中，我们着手构建与当今最好的专有模型相媲美的最佳开放模型。我们希望解决开发人员的反馈，以提高 Llama 3 的整体实用性，并在这样做的同时继续在负责任地使用和部署 LLMs.我们正在接受尽早发布的开源精神，并经常发布，以使社区能够在这些模型仍在开发中时访问它们。我们今天发布的基于文本的模型是 Llama 3 模型集合中的第一个。在不久的将来，我们的目标是使 Llama 3 成为多语言和多模态的，具有更长的上下文，并继续提高推理和编码等核心LLM功能的整体性能。

State-of-the-art performance
最先进的性能

Our new 8B and 70B parameter Llama 3 models are a major leap over Llama 2 and establish a new state-of-the-art for LLM models at those scales. Thanks to improvements in pretraining and post-training, our pretrained and instruction-fine-tuned models are the best models existing today at the 8B and 70B parameter scale. Improvements in our post-training procedures substantially reduced false refusal rates, improved alignment, and increased diversity in model responses. We also saw greatly improved capabilities like reasoning, code generation, and instruction following making Llama 3 more steerable.
我们新的 8B 和 70B 参数 Llama 3 模型是 Llama 2 的重大飞跃，并为LLM这些规模的模型建立了新的最先进的技术。由于预训练和训练后改进，我们的预训练和指令微调模型是当今 8B 和 70B 参数尺度上存在的最佳模型。我们培训后程序的改进大大降低了错误拒绝率，改善了一致性，并增加了模型响应的多样性。我们还看到了推理、代码生成和指令等功能的大幅改进，使 Llama 3 更具可操控性。



*Please see evaluation details for setting and parameters with which these evaluations are calculated.
*有关计算这些评估的设置和参数，请参阅评估详细信息。


In the development of Llama 3, we looked at model performance on standard benchmarks and also sought to optimize for performance for real-world scenarios. To this end, we developed a new high-quality human evaluation set. This evaluation set contains 1,800 prompts that cover 12 key use cases: asking for advice, brainstorming, classification, closed question answering, coding, creative writing, extraction, inhabiting a character/persona, open question answering, reasoning, rewriting, and summarization. To prevent accidental overfitting of our models on this evaluation set, even our own modeling teams do not have access to it. The chart below shows aggregated results of our human evaluations across of these categories and prompts against Claude Sonnet, Mistral Medium, and GPT-3.5.
在 Llama 3 的开发中，我们研究了标准基准测试下的模型性能，并试图针对真实场景的性能进行优化。为此，我们开发了一套新的高质量人体评估集。该评估集包含 1,800 个提示，涵盖 12 个关键用例：寻求建议、头脑风暴、分类、封闭式问答、编码、创意写作、提取、栖息角色/角色、开放式问答、推理、重写和总结。为了防止我们的模型在这个评估集上意外过拟合，即使是我们自己的建模团队也无法访问它。下图显示了我们对这些类别的人工评估的汇总结果，并针对 Claude Sonnet、Mistral Medium 和 GPT-3.5 进行了提示。



Preference rankings by human annotators based on this evaluation set highlight the strong performance of our 70B instruction-following model compared to competing models of comparable size in real-world scenarios.
人类注释者基于此评估集的偏好排名突出了我们的 70B 指令跟踪模型在真实场景中与类似规模的竞争模型相比的强大性能。

Our pretrained model also establishes a new state-of-the-art for LLM models at those scales.
我们的预训练模型还为LLM这些规模的模型建立了新的最先进的技术。


*Please see evaluation details for setting and parameters with which these evaluations are calculated.
*有关计算这些评估的设置和参数，请参阅评估详细信息。


To develop a great language model, we believe it’s important to innovate, scale, and optimize for simplicity. We adopted this design philosophy throughout the Llama 3 project with a focus on four key ingredients: the model architecture, the pretraining data, scaling up pretraining, and instruction fine-tuning.
为了开发一个伟大的语言模型，我们认为创新、扩展和优化以简化是很重要的。我们在整个 Llama 3 项目中采用了这种设计理念，重点关注四个关键要素：模型架构、预训练数据、扩展预训练和指令微调。

Model architecture 模型架构

In line with our design philosophy, we opted for a relatively standard decoder-only transformer architecture in Llama 3. Compared to Llama 2, we made several key improvements. Llama 3 uses a tokenizer with a vocabulary of 128K tokens that encodes language much more efficiently, which leads to substantially improved model performance. To improve the inference efficiency of Llama 3 models, we’ve adopted grouped query attention (GQA) across both the 8B and 70B sizes. We trained the models on sequences of 8,192 tokens, using a mask to ensure self-attention does not cross document boundaries.
根据我们的设计理念，我们在 Llama 3 中选择了相对标准的纯解码器转换器架构。与 Llama 2 相比，我们进行了几项关键改进。Llama 3 使用具有 128K 标记词汇表的分词器，可以更有效地编码语言，从而大大提高模型性能。为了提高 Llama 3 模型的推理效率，我们在 8B 和 70B 大小中都采用了分组查询注意力 （GQA）。我们在 8,192 个令牌的序列上训练模型，使用掩码来确保自我注意力不会跨越文档边界。

Training data 训练数据

To train the best language model, the curation of a large, high-quality training dataset is paramount. In line with our design principles, we invested heavily in pretraining data. Llama 3 is pretrained on over 15T tokens that were all collected from publicly available sources. Our training dataset is seven times larger than that used for Llama 2, and it includes four times more code. To prepare for upcoming multilingual use cases, over 5% of the Llama 3 pretraining dataset consists of high-quality non-English data that covers over 30 languages. However, we do not expect the same level of performance in these languages as in English.
为了训练最佳语言模型，管理大型、高质量的训练数据集至关重要。根据我们的设计原则，我们在预训练数据方面投入了大量资金。Llama 3 在超过 15T 的代币上进行了预训练，这些代币都是从公开来源收集的。我们的训练数据集比 Llama 2 使用的数据集大 7 倍，包含的代码是 Llama 2 的 4 倍。为了应对即将到来的多语言用例，Llama 3 预训练数据集的 5% 以上由涵盖 30 多种语言的高质量非英语数据组成。但是，我们预计这些语言的性能水平与英语不同。

To ensure Llama 3 is trained on data of the highest quality, we developed a series of data-filtering pipelines. These pipelines include using heuristic filters, NSFW filters, semantic deduplication approaches, and text classifiers to predict data quality. We found that previous generations of Llama are surprisingly good at identifying high-quality data, hence we used Llama 2 to generate the training data for the text-quality classifiers that are powering Llama 3.
为了确保 Llama 3 接受最高质量的数据训练，我们开发了一系列数据过滤管道。这些管道包括使用启发式筛选器、NSFW 筛选器、语义重复数据删除方法和文本分类器来预测数据质量。我们发现前几代 Llama 在识别高质量数据方面出奇地好，因此我们使用 Llama 2 为 Llama 3 提供支持的文本质量分类器生成训练数据。

We also performed extensive experiments to evaluate the best ways of mixing data from different sources in our final pretraining dataset. These experiments enabled us to select a data mix that ensures that Llama 3 performs well across use cases including trivia questions, STEM, coding, historical knowledge, etc.
我们还进行了广泛的实验，以评估在最终的预训练数据集中混合来自不同来源的数据的最佳方法。这些实验使我们能够选择一种数据组合，确保 Llama 3 在包括琐事问题、STEM、编码、历史知识等在内的用例中表现良好。

Scaling up pretraining 扩大预训练规模

To effectively leverage our pretraining data in Llama 3 models, we put substantial effort into scaling up pretraining. Specifically, we have developed a series of detailed scaling laws for downstream benchmark evaluations. These scaling laws enable us to select an optimal data mix and to make informed decisions on how to best use our training compute. Importantly, scaling laws allow us to predict the performance of our largest models on key tasks (for example, code generation as evaluated on the HumanEval benchmark—see above) before we actually train the models. This helps us ensure strong performance of our final models across a variety of use cases and capabilities.
为了在 Llama 3 模型中有效地利用我们的预训练数据，我们投入了大量精力来扩大预训练。具体而言，我们为下游基准评估制定了一系列详细的扩展法则。这些缩放定律使我们能够选择最佳的数据组合，并就如何最好地使用我们的训练计算做出明智的决策。重要的是，缩放定律允许我们在实际训练模型之前预测最大模型在关键任务上的性能（例如，在 HumanEval 基准测试中评估的代码生成——见上文）。这有助于我们确保最终模型在各种用例和功能中具有强大的性能。

We made several new observations on scaling behavior during the development of Llama 3. For example, while the Chinchilla-optimal amount of training compute for an 8B parameter model corresponds to ~200B tokens, we found that model performance continues to improve even after the model is trained on two orders of magnitude more data. Both our 8B and 70B parameter models continued to improve log-linearly after we trained them on up to 15T tokens. Larger models can match the performance of these smaller models with less training compute, but smaller models are generally preferred because they are much more efficient during inference.
我们在 Llama 3 的开发过程中对缩放行为进行了一些新的观察。例如，虽然 8B 参数模型的 Chinchilla 最优训练计算量对应于 ~200B 标记，但我们发现，即使在模型使用两个数量级的数据进行训练后，模型性能仍在继续提高。我们的 8B 和 70B 参数模型在我们对高达 15T 的代币进行训练后，继续对数线性改进。较大的模型可以与这些较小模型的性能相匹配，但训练计算较少，但通常首选较小的模型，因为它们在推理过程中效率更高。

To train our largest Llama 3 models, we combined three types of parallelization: data parallelization, model parallelization, and pipeline parallelization. Our most efficient implementation achieves a compute utilization of over 400 TFLOPS per GPU when trained on 16K GPUs simultaneously. We performed training runs on two custom-built 24K GPU clusters. To maximize GPU uptime, we developed an advanced new training stack that automates error detection, handling, and maintenance. We also greatly improved our hardware reliability and detection mechanisms for silent data corruption, and we developed new scalable storage systems that reduce overheads of checkpointing and rollback. Those improvements resulted in an overall effective training time of more than 95%. Combined, these improvements increased the efficiency of Llama 3 training by ~three times compared to Llama 2.
为了训练我们最大的 Llama 3 模型，我们结合了三种类型的并行化：数据并行化、模型并行化和管道并行化。我们最高效的实现是在 16K GPU 上同时训练时，每个 GPU 的计算利用率超过 400 TFLOPS。我们在两个定制的 24K GPU 集群上执行了训练运行。为了最大限度地延长 GPU 正常运行时间，我们开发了一种先进的新训练堆栈，可自动执行错误检测、处理和维护。我们还大大改进了硬件可靠性和静默数据损坏检测机制，并开发了新的可扩展存储系统，以减少检查点和回滚的开销。这些改进使总体有效培训时间超过 95%。总之，这些改进将 Llama 3 的训练效率提高了 ~3 倍，比 Llama 2 提高了 ~3 倍。

Instruction fine-tuning 指令微调

To fully unlock the potential of our pretrained models in chat use cases, we innovated on our approach to instruction-tuning as well. Our approach to post-training is a combination of supervised fine-tuning (SFT), rejection sampling, proximal policy optimization (PPO), and direct preference optimization (DPO). The quality of the prompts that are used in SFT and the preference rankings that are used in PPO and DPO has an outsized influence on the performance of aligned models. Some of our biggest improvements in model quality came from carefully curating this data and performing multiple rounds of quality assurance on annotations provided by human annotators.
为了在聊天用例中充分释放预训练模型的潜力，我们还对指令调整方法进行了创新。我们的后培训方法是监督微调 （SFT）、拒绝抽样、近端策略优化 （PPO） 和直接偏好优化 （DPO） 的组合。SFT 中使用的提示的质量以及 PPO 和 DPO 中使用的偏好排名对对齐模型的性能有很大影响。我们在模型质量方面的一些最大改进来自于仔细管理这些数据，并对人工注释者提供的注释执行多轮质量保证。

Learning from preference rankings via PPO and DPO also greatly improved the performance of Llama 3 on reasoning and coding tasks. We found that if you ask a model a reasoning question that it struggles to answer, the model will sometimes produce the right reasoning trace: The model knows how to produce the right answer, but it does not know how to select it. Training on preference rankings enables the model to learn how to select it.
通过 PPO 和 DPO 从偏好排名中学习也大大提高了 Llama 3 在推理和编码任务上的表现。我们发现，如果你问一个模型一个它难以回答的推理问题，模型有时会产生正确的推理痕迹：模型知道如何产生正确的答案，但它不知道如何选择它。对偏好排名的训练使模型能够学习如何选择它。

Building with Llama 3 与骆驼一起建造 3

Our vision is to enable developers to customize Llama 3 to support relevant use cases and to make it easier to adopt best practices and improve the open ecosystem. With this release, we’re providing new trust and safety tools including updated components with both Llama Guard 2 and Cybersec Eval 2, and the introduction of Code Shield—an inference time guardrail for filtering insecure code produced by LLMs.
我们的愿景是使开发人员能够自定义 Llama 3 以支持相关用例，并使其更容易采用最佳实践和改进开放生态系统。在此版本中，我们将提供新的信任和安全工具，包括 Llama Guard 2 和 Cybersec Eval 2 的更新组件，并引入了 Code Shield，这是一种用于过滤 生成的不安全代码的推理时间护栏LLMs。

We’ve also co-developed Llama 3 with torchtune, the new PyTorch-native library for easily authoring, fine-tuning, and experimenting with LLMs. torchtune provides memory efficient and hackable training recipes written entirely in PyTorch. The library is integrated with popular platforms such as Hugging Face, Weights & Biases, and EleutherAI and even supports Executorch for enabling efficient inference to be run on a wide variety of mobile and edge devices. For everything from prompt engineering to using Llama 3 with LangChain we have a comprehensive getting started guide and takes you from downloading Llama 3 all the way to deployment at scale within your generative AI application.
我们还与 torchtune 共同开发了 Llama 3，torchtune 是新的 PyTorch 原生库，可轻松创作、微调和试验LLMs。torchtune 提供完全用 PyTorch 编写的内存高效且可破解的训练配方。该库与Hugging Face、Weights & Biases和EleutherAI等流行平台集成，甚至支持Executorch，以便在各种移动和边缘设备上运行高效推理。对于从提示工程到将 Llama 3 与 LangChain 一起使用的方方面面，我们都有一份全面的入门指南，带您从下载 Llama 3 一直到在生成式 AI 应用程序中大规模部署。

A system-level approach to responsibility
系统级的责任方法

We have designed Llama 3 models to be maximally helpful while ensuring an industry leading approach to responsibly deploying them. To achieve this, we have adopted a new, system-level approach to the responsible development and deployment of Llama. We envision Llama models as part of a broader system that puts the developer in the driver’s seat. Llama models will serve as a foundational piece of a system that developers design with their unique end goals in mind.
我们设计的 Llama 3 模型具有最大的帮助，同时确保采用行业领先的方法来负责任地部署它们。为了实现这一目标，我们采用了一种新的系统级方法来负责任地开发和部署 Llama。我们将 Llama 模型设想为更广泛系统的一部分，让开发人员坐在驾驶座上。Llama 模型将作为系统的基础部分，开发人员在设计时会考虑到他们独特的最终目标。


Instruction fine-tuning also plays a major role in ensuring the safety of our models. Our instruction-fine-tuned models have been red-teamed (tested) for safety through internal and external efforts. ​​Our red teaming approach leverages human experts and automation methods to generate adversarial prompts that try to elicit problematic responses. For instance, we apply comprehensive testing to assess risks of misuse related to Chemical, Biological, Cyber Security, and other risk areas. All of these efforts are iterative and used to inform safety fine-tuning of the models being released. You can read more about our efforts in the model card.
指令微调在确保模型安全方面也起着重要作用。我们的指令微调模型已通过内部和外部努力进行了安全测试。我们的红队方法利用人类专家和自动化方法来生成对抗性提示，试图引发有问题的响应。例如，我们应用全面的测试来评估与化学、生物、网络安全和其他风险领域相关的滥用风险。所有这些努力都是迭代的，用于为正在发布的模型的安全微调提供信息。您可以在模型卡中阅读有关我们努力的更多信息。

Llama Guard models are meant to be a foundation for prompt and response safety and can easily be fine-tuned to create a new taxonomy depending on application needs. As a starting point, the new Llama Guard 2 uses the recently announced MLCommons taxonomy, in an effort to support the emergence of industry standards in this important area. Additionally, CyberSecEval 2 expands on its predecessor by adding measures of an LLM’s propensity to allow for abuse of its code interpreter, offensive cybersecurity capabilities, and susceptibility to prompt injection attacks (learn more in our technical paper). Finally, we’re introducing Code Shield which adds support for inference-time filtering of insecure code produced by LLMs. This offers mitigation of risks around insecure code suggestions, code interpreter abuse prevention, and secure command execution.
Llama Guard 模型旨在成为及时和响应安全的基础，并且可以根据应用需求轻松进行微调以创建新的分类法。作为起点，新的 Llama Guard 2 使用最近宣布的 MLCommons 分类法，以支持这一重要领域的行业标准的出现。此外，CyberSecEval 2 在其前身的基础上进行了扩展，增加了允许滥用其代码解释器、攻击性网络安全功能以及对提示注入攻击的敏感性的倾向的衡量标准LLM（在我们的技术论文中了解更多信息）。最后，我们引入了 Code Shield，它增加了对 生成的LLMs不安全代码的推理时过滤的支持。这样可以降低不安全的代码建议、代码解释器滥用预防和安全命令执行的风险。

With the speed at which the generative AI space is moving, we believe an open approach is an important way to bring the ecosystem together and mitigate these potential harms. As part of that, we’re updating our Responsible Use Guide (RUG) that provides a comprehensive guide to responsible development with LLMs. As we outlined in the RUG, we recommend that all inputs and outputs be checked and filtered in accordance with content guidelines appropriate to the application. Additionally, many cloud service providers offer content moderation APIs and other tools for responsible deployment, and we encourage developers to also consider using these options.
随着生成式人工智能领域的发展速度，我们相信开放方法是将生态系统整合在一起并减轻这些潜在危害的重要方式。作为其中的一部分，我们正在更新我们的负责任使用指南 （RUG），该指南提供了负责任的LLMs开发综合指南。正如我们在 RUG 中概述的那样，我们建议根据适合应用程序的内容指南检查和过滤所有输入和输出。此外，许多云服务提供商提供内容审核 API 和其他工具，用于负责任的部署，我们鼓励开发人员也考虑使用这些选项。

Deploying Llama 3 at scale
大规模部署 Llama 3

Llama 3 will soon be available on all major platforms including cloud providers, model API providers, and much more. Llama 3 will be everywhere.
Llama 3 将很快在所有主要平台上推出，包括云提供商、模型 API 提供商等。骆驼 3 将无处不在。

Our benchmarks show the tokenizer offers improved token efficiency, yielding up to 15% fewer tokens compared to Llama 2. Also, Group Query Attention (GQA) now has been added to Llama 3 8B as well. As a result, we observed that despite the model having 1B more parameters compared to Llama 2 7B, the improved tokenizer efficiency and GQA contribute to maintaining the inference efficiency on par with Llama 2 7B.
我们的基准测试显示，代币化器提供了更高的代币效率，与 Llama 2 相比，代币产量减少了 15%。此外，Group Query Attention （GQA） 现在也已添加到 Llama 3 8B 中。因此，我们观察到，尽管该模型的参数比 Llama 2 7B 多了 1B，但分词器效率和 GQA 的提高有助于保持推理效率与 Llama 2 7B 相当。

For examples of how to leverage all of these capabilities, check out Llama Recipes which contains all of our open source code that can be leveraged for everything from fine-tuning to deployment to model evaluation.
有关如何利用所有这些功能的示例，请查看 Llama Recipes，其中包含我们所有的开源代码，这些代码可用于从微调到部署再到模型评估的所有工作。

What’s next for Llama 3?
Llama 3 的下一步是什么？

The Llama 3 8B and 70B models mark the beginning of what we plan to release for Llama 3. And there’s a lot more to come.
Llama 3、8B 和 70B 型号标志着我们计划为 Llama 3 发布的产品的开始。还有很多事情要做。

Our largest models are over 400B parameters and, while these models are still training, our team is excited about how they’re trending. Over the coming months, we’ll release multiple models with new capabilities including multimodality, the ability to converse in multiple languages, a much longer context window, and stronger overall capabilities. We will also publish a detailed research paper once we are done training Llama 3.
我们最大的模型参数超过 400B，虽然这些模型仍在训练中，但我们的团队对它们的趋势感到兴奋。在接下来的几个月里，我们将发布多个具有新功能的模型，包括多模态、使用多种语言交谈的能力、更长的上下文窗口和更强大的整体功能。一旦我们完成了Llama 3的训练，我们还将发表一篇详细的研究论文。

To give you a sneak preview for where these models are today as they continue training, we thought we could share some snapshots of how our largest LLM model is trending. Please note that this data is based on an early checkpoint of Llama 3 that is still training and these capabilities are not supported as part of the models released today.
为了让您先睹为快，了解这些模型在继续训练时所处的位置，我们认为我们可以分享一些我们最大的LLM模型趋势的快照。请注意，此数据基于仍在训练的 Llama 3 的早期检查点，今天发布的模型不支持这些功能。


*Please see evaluation details for setting and parameters with which these evaluations are calculated.
*有关计算这些评估的设置和参数，请参阅评估详细信息。


We’re committed to the continued growth and development of an open AI ecosystem for releasing our models responsibly. We have long believed that openness leads to better, safer products, faster innovation, and a healthier overall market. This is good for Meta, and it is good for society. We’re taking a community-first approach with Llama 3, and starting today, these models are available on the leading cloud, hosting, and hardware platforms with many more to come.
我们致力于开放 AI 生态系统的持续增长和发展，以负责任地发布我们的模型。我们一直认为，开放会带来更好、更安全的产品、更快的创新和更健康的整体市场。这对 Meta 有好处，对社会也有好处。我们在 Llama 3 中采用社区优先的方法，从今天开始，这些模型可以在领先的云、托管和硬件平台上使用，未来还会有更多模型。

Try Meta Llama 3 today
立即试用 Meta Llama 3

We’ve integrated our latest models into Meta AI, which we believe is the world’s leading AI assistant. It’s now built with Llama 3 technology and it’s available in more countries across our apps.
我们已将最新模型集成到 Meta AI 中，我们相信 Meta AI 是世界领先的 AI 助手。它现在采用 Llama 3 技术构建，并通过我们的应用程序在更多国家/地区推出。

You can use Meta AI on Facebook, Instagram, WhatsApp, Messenger, and the web to get things done, learn, create, and connect with the things that matter to you. You can read more about the Meta AI experience here.
您可以在 Facebook、Instagram、WhatsApp、Messenger 和网络上使用 Meta AI 来完成工作、学习、创造和联系对你来说重要的事情。您可以在此处阅读有关 Meta AI 体验的更多信息。

Visit the Llama 3 website to download the models and reference the Getting Started Guide for the latest list of all available platforms.
访问 Llama 3 网站下载模型并参考入门指南以获取所有可用平台的最新列表。

You’ll also soon be able to test multimodal Meta AI on our Ray-Ban Meta smart glasses.
您很快将能够在我们的 Ray-Ban Meta 智能眼镜上测试多模态 Meta AI。

As always, we look forward to seeing all the amazing products and experiences you will build with Meta Llama 3.
一如既往，我们期待看到您将使用 Meta Llama 3 构建的所有令人惊叹的产品和体验。

[1]: https://ai.meta.com/blog/meta-llama-3/

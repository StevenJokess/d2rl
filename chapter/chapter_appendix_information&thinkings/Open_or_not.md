

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2024-06-21 10:49:50
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2024-06-21 10:50:04
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->

在生成式AI中寻求平衡：开源模型与专有模型之争
2023.10.19
作者：腾讯企业发展事业群总经理兼欧洲首席代表葛凌博士

今年三月，OpenAI开创性的GPT-4基础模型标志着生成式人工智能（AI）历史迈向全新里程碑。而仅仅两周后，旧金山市区又举办了另一场被誉为“AI届的伍德斯托克”的活动，引起科技界关注。

这场充满活力的聚会旨在庆祝开源型生成AI的快速成长，以及相关社群的涌现。在此后的几个月里，开源生态系统中出现了大量新兴参与者、模型和使用案例。未来当我们回顾过去时，很可能会将这段时期定位为两种AI类型——专有模型和开源模型——竞争公开化的决定性节点。

在GPT-4发布和“AI届的伍德斯托克”聚会后的六个月中，这两种模型的竞争渐趋白热化。这里先明确一下定义：生成式AI被归类为“闭源”，其中专有的基础模型通常由大型科技公司拥有，用户调用API需要付费。相比之下，开源生态系统支持免费分享和调整AI模型参数（参与的公司通过分享云服务供应商提供的模型等方式间接获得收入）。

我们正在见证两种模型的对弈。开源的支持者声称开源模型的发展势头是强大而不可阻挡的。与此同时，OpenAI这个月刚刚推出另一个强大的专有模型——GPT-Vision，旨在将视觉与文本结合起来。在新书《The Coming Wave》中，DeepMind的联合创始人Mustafa Suleyman认为，出于安全考虑，应禁止AI模型中使用开源。

全球企业和消费者主要采用闭源的生成式AI还是开源的生成式AI，或者均衡使用两种模型，将是问题的关键。结果至关重要，因为讨论的出发点是确保AI以有利于人类的方式发展。不仅如此，这场竞争还将塑造商业和社会中最具变革性的AI用例，并决定生成式AI的受益者。



不过首先，所谓的“AI届的伍德斯托克”是什么，又有哪些人参与了呢？这场“开源AI聚会”于三月末在旧金山的探索馆举行，参与者超过5000人。正如它的名字来源“伍德斯托克摇滚音乐节”，活动洋溢着派对氛围，而开源运动的合作精神和创新动力更为其锦上添花。

人潮涌动中，活动的组织者——AI公司Hugging Face的首席执行官Clement Delangue，打扮成公司的吉祥物，一个看起来像“拥抱脸”的开心的黄色表情符号，非常应景。羊驼漫步会场，致敬Meta大语言模型“LLaMA”。“释放羊驼”的标语在空中飘扬，各种AI名人，如吴恩达以及大语言模型（LLM）初创公司Anthropic的高管悉数到场。许多来宾被《时代》杂志列为当前AI领域“最具影响力的100人”。

尽管这个场景与任何科技会议都不同，但参与者分享的想法足以改变整个行业，对生成式AI巨大潜力的期待清晰又殷切——麦肯锡近期估计，在63个用例中，生成式AI的潜力每年或将额外产生2.6至4.4万亿美元的价值。

全球的科技领导者都对此兴奋不已。例如，今年五月，腾讯创始人兼首席执行官马化腾在公司的股东大会上表示：“我们最开始以为AI是互联网十年不遇的机会，但是越想越觉得，这是几百年不遇的、类似发明电的工业革命一样的机遇。”

那么，哪种类型的生成式AI模型正在引领新的工业革命？现在，专有模型处于领先地位。这有两个明显的原因：专有模型在能力方面领先，并且目前人们认为专有模型更安全。

首先是性能。根据领先基准，如大规模多任务语言理解评测，OpenAI的GPT-4目前脱颖而出，成为最强大和最有能力的LLM。尽管开源模型的质量正在迅速提升，但它们仍然落后于先进的闭源同类产品。

这背后的原因是训练先进基础模型的严酷商业现实。前期成本巨大，从购买专业硬件，如高达3万美元的Nvidia最新H100 GPU芯片，到巨额云计算费用都包含在内。此外，部署先进的训练技术，如人类反馈的强化学习，需要专业知识。像Cohere、Anthropic、Adept、Mistral、Aleph Alpha、AI21 Labs和Imbue这样的初创公司，大部分预算都投入到了芯片上，这一支出模式便能说明这一点。

总的来说，专有模型投入资源最多。以OpenAI为例，所涉及的成本之高似乎促使它从开源转向封闭。OpenAI由首席执行官Sam Altman以及Elon Musk等知名人士在2015年创立，最初致力于研发开源模型。然而，在发布迄今为止最强大的大语言模型时，它却放弃了最初的开源承诺。这种转变部分可以归因于OpenAI需要保护其巨额投入。

安全性目前被视为闭源的另一优势。OpenAI声称，选择封闭的另一个原因是LLM相关的道德风险。这些模型有可能被不良行为者滥用，随着模型能力越来越强，公开可访问的风险也在增加。OpenAI的首席科学家Ilya Sutskever表示：“如果你像我们一样相信，某个时候，AI或者AGI将变得极其强大，那么开源它根本没有意义。这是一个糟糕的主意。”

那么，鉴于Sutskever的上述论点，以及专有模型的强大性能优势，为什么开源生成式AI发展会引起如此广泛的关注呢？全球最大的科技公司以及初创公司和大量开发者都加入了这股潮流。

其中一个原因是，随着时间的推移，开源在科技界中慢慢取得了切实成功。现代云基础设施主要在Linux上运行，机器学习由Python等开源编程语言所驱动，开源渗透到了科技领域的许多方面。

“AI届的伍德斯托克”的激动人心之处在于开源创新。开源LLM将其权重和参数公开，使全球的开发者社群能够对其进行微调并改进，激发出比最新的专有模型更大的创新。

对于寻求采用生成式AI的企业来说，轻松微调开源模型的能力也非常吸引人。他们可以根据自己公司特定的数据来定制这些模型，以实现需要这种知识的特定用例。

“AI届的伍德斯托克”的组织者Hugging Face是开源AI运动的早期先驱之一。该公司成立于2016年，其开源产品之一是Transformers库。它是LLM的开放存储库，客户可以访问以进一步自行调整模型，或者通过API调用典型的LLM功能，如补全句子、分类或文本生成。这个“模型即服务”平台使各种规模的企业都可以从实验过渡到部署，而无需占用过多内部资源。用户可以使用托管的基础设施将任何模型转换为自己的API，彰显出开源的民主化AI精神。

Microsoft、Google、Meta、Intel和eBay等巨头都是Hugging Face一万多名客户中的一员。其“模型即服务”概念已经演变为托管超过一百万个模型、数据集和应用程序。这个多样化的生态系统强调了其开源工具的广泛适用性，范围包含从辉瑞和罗氏等制药巨头的数据安全升级，到专门的AI应用，如彭博的财务语言模型BloombergGPT。

随着AI领域的不断发展，领导者和关键参与者越来越主张将生成式AI开源。图灵奖得主、Meta的首席AI科学家Yann LeCun认为世界需要开源LLM：“AI基础模型将成为基础设施，人们和行业会要求它开源。就像互联网的软件基础设施一样。”

Meta首席执行官Mark Zuckerberg对开源的热衷则是出于不同的目的。“它每天都在变得更加高效，”他评论道，“我只是觉得，整个社群，包括学生、黑客、初创公司以及其他人使用开源模型，我们也能从中学到很多。”

在这种理念的指导下，Meta于7月发布的LLaMa-2可以说是目前公众可以获取的最强大、性能最高的开源LLM。它提供了预训练和微调的版本，参数分别为70亿、130亿和700亿。

除了LLaMa-2这样的主流项目外，其他一些值得关注的项目也正在为开源AI生态做出贡献。例如，Runway公司于2018年开始专注于为电影制作人提供AI工具，但现在已经转向生成式AI。其旗舰产品Gen-2开拓了根据文本提示创建视频这一细分领域，该公司还推出了Runway Studios和AI电影节以扩大其影响力。

另一方面，LangChain作为一个Python库，旨在增强LLM的可用性、可访问性和多功能性，使开发人员更容易将这些强大的工具集成到各种应用程序中。这些项目都展示了开源AI模型在不同行业日渐增长的适用性和多样性。

开源模型也挑战了关于模型参数的一个观念，即“更大总是更好”。较小的模型可以提供成本效益、更高的灵活性，而且在针对特定应用程序进行微调时，甚至可能超越大模型的表现。

涉及到确保AI安全和负责任的关键问题时，开源一方也有好的论据。专有模型的支持者认为，让所有人都可以访问模型很危险。然而，开源AI的支持者反驳说，开源LLM提供了透明度，并吸引多元社群的审查。这有助于识别和减少偏见，使它们更加公正。此外，开源模型与一些闭源模型不同，它们在如何使用用户数据方面提供了透明度。

未来将会如何，哪种模型将会胜出呢？总的来说，两种模型各有千秋。以GPT-4为代表的专有模型具有独特的优势，包括自定义、专用支持和强大的安全功能。另一方面，效率、透明度和公平性等特征为开源AI提供了有力的论据。

当然，理性的策略是让公司提供并利用两者的优点。因此，腾讯采取双重策略。我们已经推出了专有基础AI模型“混元”，可用于多种应用程序，同时也在腾讯云上提供了一个“模型即服务”解决方案。这项服务旨在协助多个行业高效部署开源模型。我们预计，在未来格局中，少数几种专有基础模型将占据主导地位，但针对特定行业和企业应用的专门开源模型也将蓬勃发展。基于非常小的模型（能够在智能手机和笔记本电脑的即时通讯软件内运行）的个人AI助手将成为我们的伙伴。

Meta的LLaMa-2由美国云服务提供商，如Microsoft Azure和Amazon的AWS托管，这进一步证明这些科技巨头同样看到支持开源模型和专有模型的价值所在。

我们应该欢迎开源和专有模型之间的良性竞争。幸运的是，目前似乎还没有一种模型能占据主导地位。过去六个月中，两种模型之间的质量差距已经缩小。开源模型在激发创新、AI民主化以及促进责任和安全方面的潜力愈加明显。

牛津大学计算机科学教授、图灵研究所基础AI研究主任、AI先驱Michael Wooldridge教授将在2023年皇家研究院圣诞讲座上发表演讲“AI的真相”。他希望看到两种模型都能繁荣发展。他表示：“这一年，像ChatGPT这样的大众市场、通用AI工具已经出现，我们处于一个关键节点。开源和专有模型各有优劣。在向前发展的过程中，保持平衡至关重要，我们需要确保AI继续成为造福广大社会的工具。”正如1969年的伍德斯托克音乐节一样，2023年春天的旧金山已经在AI史册上占有一席之地。


[1]: https://www.tencent.com/zh-cn/articles/2201720.html
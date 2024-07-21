# BosonAI

## Announcing the Higgs Family of LLMs
宣布希格斯家族LLMs
June 5, 2024 6月 5， 2024

Since founding Boson AI in 2023, we have 【dedicated ourselves to】 empower enterprises with AI technologies, with a mission to transform how stories are told, knowledge is learned, and insights are gathered. We helped customers build intelligent agents to interact with their users by playing various roles, including game characters, language tutors, insurance agents and financial advisors.

自 2023 年创立 Boson AI 以来，我们一直【致力】于用 AI 技术赋能企业，以改变讲故事、学习知识和收集见解的方式为使命。我们通过扮演各种角色，包括游戏角色、语言导师、保险代理人和财务顾问，帮助客户构建智能代理，以便与用户互动。

Today, we are excited to share our open-source series of Higgs family of large language models with the community to 【foster innovation】. The first model, 【Higgs-Llama-3-70B】, is a powerful chat model based on Meta’s LLaMA-3-base. It is specially tuned for role-playing while being competitive in general-domain instruction-following and reasoning.

今天，我们很高兴与社区分享我们的开源系列希格斯系列大型语言模型，以【促进创新】。第一个模型 Higgs-Llama-3-70B 是基于 Meta 的 LLaMA-3-base 的强大聊天模型。它专门针对角色扮演进行了调整，同时在一般领域的指令遵循和推理方面具有竞争力。

### Role Playing 角色扮演

Our customers want models with personality and character. Beyond the general abilities of a “helpful AI assistant”, models need to be able to act as a patient teacher, competent financial adviser, sympathetic companion, an evil villain, or an ambiguous protagonist 【straddling the line between】 good and evil.

我们的客户想要具有个性和特色的模型。除了“有用的人工智能助手”的一般能力之外，模型还需要能够扮演耐心的老师、称职的财务顾问、富有同情心的同伴、邪恶的恶棍或【跨越善恶界限】的模棱两可的主角。

In order to achieve this, models need the ability to follow and adapt to story and scene context, rather than just emulate a known character. A hero may be tempted in a particular situation, while a villain may be perfectly willing to provide sound advice in poetry, given the right context. To accomplish specific missions, goals and instructions, agents require strong general-domain reasoning ability.

为了实现这一点，模型需要能够遵循和适应故事和场景背景，而不仅仅是模仿已知角色。英雄可能会在特定情况下受到诱惑，而恶棍可能完全愿意在适当的背景下在诗歌中提供合理的建议。为了完成特定的任务、目标和指令，智能体需要强大的通用领域推理能力。

We built both pretraining and post-training pipelines to generate models that excel in role playing. The current release showcases the effectiveness of our post-training pipeline. We choose Meta’s LLama-3 base model as a strong starting point. This is followed by our in-house teacher models and toolings to guide the alignment, making the fine-tuned model strong in both general-domain tasks and role playing.
我们构建了训练前和训练后管道，以生成在角色扮演方面表现出色的模型。当前版本展示了我们培训后管道的有效性。我们选择 Meta 的 LLama-3 基本模型作为强有力的起点。接下来是我们的内部教师模型和工具来指导对齐，使微调的模型在一般领域任务和角色扮演中都很强大。

Benchmarks 基准
All benchmarks lead to eventual overfitting, including those for LLMs. Training on data, particularly beneficial for benchmarks typically does not improve (or even worsen) role playing performance. We worked to exclude benchmark data, including their training examples, from our fine-tuning data.
所有基准都会导致最终的过拟合，包括 的LLMs过拟合。对数据进行训练，对基准测试特别有益，通常不会提高（甚至恶化）角色扮演性能。我们努力从微调数据中排除基准数据，包括它们的训练示例。

We highlight our results on two new and challenging benchmarks: MMLU-Pro and Arena-hard. MMLU-Pro extends the popular MMLU benchmark. We believe that it suffers from less overfitting by other released models as well, as it was released only recently (it was released after our models finished training).
我们重点介绍了两个具有挑战性的新基准测试的结果：MMLU-Pro 和 Arena-hard。MMLU-Pro 扩展了流行的 MMLU 基准测试。我们认为，其他已发布的模型也较少受到过拟合的影响，因为它是最近才发布的（它是在我们的模型完成训练后发布的）。

Model	MMLU-Pro MMLU-Pro型
GPT-4o GPT-4o的	72.6
Gemini-1.5-Pro 双子座-1.5-Pro	69.0
Claude-3-Opus 克劳德-3-Opus	68.5
GPT-4-Turbo GPT-4-涡轮增压	63.7
Higgs-Llama-3-70B 希格斯-骆驼-3-70B	63.2
Gemini-1.5-Flash Gemini-1.5-闪存	59.1
Claude-3-Sonnet 克劳德-3-十四行诗	56.8
Llama-3-70B-Instruct 骆驼-3-70B-指示	56.2
Arena-hard contains 500 challenging real user queries from the popular Chatbot Arena.
Arena-hard 包含来自流行的聊天机器人 Arena 的 500 个具有挑战性的真实用户查询。

Model	Arena-Hard 竞技场-硬
GPT-4o GPT-4o的	79.5
Gemini-1.5-Pro 双子座-1.5-Pro	72.0
Claude-3-Opus 克劳德-3-Opus	60.4
Higgs-Llama-3-70B 希格斯-骆驼-3-70B	49.6
Gemini-1.5-Flash Gemini-1.5-闪存	49.6
Claude-3-Sonnet 克劳德-3-十四行诗	46.8
Claude-3-Haiku 克劳德-3-俳句	41.5
Llama-3-70B-Instruct 骆驼-3-70B-指示	41.1
GPT-4-0613	37.9
Mistral-Large	37.7
With the same base model, Higgs-Llama-3-70B outperforms Meta’s LLama-3-70B-Instruct on 6 widely-used benchmarks.
使用相同的基本模型，Higgs-Llama-3-70B 在 6 个广泛使用的基准测试中优于 Meta 的 LLama-3-70B-Instruct。

MMLU-Pro MMLU-Pro型	Arena-Hard 竞技场-硬	AlpacaEval  羊驼Eval
2.0 LC 2.0 液相色谱	MMLU	GPQA	DROP  落
(F1,3-shot) （F1,3-射击）
GPT-4o GPT-4o的	72.6	82.6	57.5	87.2	49.9	83.7
Higgs-Llama-3-70B 希格斯-骆驼-3-70B	63.2	49.6	38.6	80.8	42.1	81.6
LLama-3-70B-Instruct LLama-3-70B-指示	56.2	41.1	34.4	80.2	41.3	81.4
What’s Next? 下一步是什么？
Higgs-Llama-3-70B is but an appetizer of what Boson AI offers. We will dive into the role playing performance, the post-training pipeline, our experience in building a datacenter from scratch, using GPUs in the cloud, straddling multiple providers in the future. This includes releasing more models from the Higgs family.
Higgs-Llama-3-70B 只是 Boson AI 提供的开胃菜。我们将深入探讨角色扮演性能、训练后管道、我们从头开始构建数据中心的经验、在云中使用 GPU、未来跨越多个提供商的经验。这包括发布希格斯家族的更多模型。

We would like to thank our customers for their constructive feedback and the excellent technical support from our friends at NVIDIA, Arc Compute, eStruxture, Crusoe, AWS and Scaleway. This wouldn’t have been possible without them. There are more stories to be told in the future.
我们要感谢客户的建设性反馈，以及 NVIDIA、Arc Compute、eStruxture、Crusoe、AWS 和 Scaleway 等朋友的出色技术支持。没有他们，这是不可能的。未来还有更多故事要讲。

##

Announcing Higgs Llama V2
Higgs Llama V2 发布
At Boson AI, we are working on intelligent agents that can serve as human companions and helpers. Today, we are excited to share Higgs-Llama-3-70B-v2, a new model that significantly improves upon its predecessor. It narrows the gap to the very best proprietary models on benchmarks relevant for dialog, interaction and understanding. Arena-Hard and AlpacaEval 2.0 measure the general intelligence of LLMs and correlate well with human preference. MMLU-Pro is a recent benchmark that measures LLM’s knowledge and reasoning capability.
在Boson AI，我们正在研究可以作为人类伴侣和助手的智能代理。今天，我们很高兴与大家分享 Higgs-Llama-3-70B-v2 ，一种在其前身的基础上显着改进的新模型。它缩小了与对话、交互和理解相关的基准上与最佳专有模型的差距。Arena-Hard 和 AlpacaEval 2.0 测量人类的一般智力LLMs并与人类的偏好密切相关。MMLU-Pro是衡量LLM知识和推理能力的最新基准。

Higgs-v2

Partnering with the roleplay community we collected 6.2M dialogues in a 2-week A/B test. This allowed us to evaluate Higgs v2 directly against other models. Compared to Claude 3.5 Sonnet, Higgs v2 reduces the response regeneration rate1 by 21.6%. This rate matters as it directly relates to the cases where users are unhappy with the generated result. Moreover, it increases the day 1 retention rate2 by 5.3%.
我们与角色扮演社区合作，在为期 2 周的 A/B 测试中收集了 6.2M 对话。这使我们能够直接针对其他模型评估 Higgs v2。与克劳德 3.5 十四行诗相比，希格斯 v2 将响应再生率 1 降低了 21.6%。这个比率很重要，因为它与用户对生成的结果不满意的情况直接相关。此外，它还将第 1 天的保留率 2 提高了 5.3%。

Higgs Judger 希格斯评判者
Much of the performance boost of Higgs v2 comes from an improved judging system, which guides the model alignment through synthetic feedback signals. We built an in-house LLM reward model, named Higgs Judger, to evaluate model outputs. On Reward Bench, Higgs Judger ties with the best generative judger, Google’s Gemini 1.5 Pro, in the leaderboard.
Higgs v2 的大部分性能提升来自改进的判断系统，该系统通过合成反馈信号指导模型对齐。我们建立了一个内部LLM奖励模型，名为 Higgs Judger，用于评估模型输出。在Reward Bench上，Higgs Judger在排行榜上与最佳生成式评委Google的Gemini 1.5 Pro并列。

Higgs Judger

In addition, this judger model learns the preference of players during roleplays, using the the feedback that the user provides.
此外，该评判者模型使用用户提供的反馈，在角色扮演过程中学习玩家的偏好。

What’s Next? 下一步是什么？
We are conducting more evaluations before the final release. If you would like to access Higgs v2 early or do customization, please contact us at api@boson.ai.
在最终版本发布之前，我们正在进行更多评估。如果您想尽早访问 Higgs v2 或进行自定义，请通过以下方式 api@boson.ai 与我们联系。

Acknowledgments 确认
Model: Xingjian Shi, Rand Xie, Weisu Yin
模特： Xingjian Shi， Rand Xie， Weisu Yin

Serving: Yizhi Liu, Zach Zheng
服务：刘一志、郑淑娴

Data / Evaluation: Yi Zhu, Jaewon Lee, Weisu Yin, Canwen Xu
数据/评估：Yi Zhu， Jaewon Lee， Weisu Yin， Canwen Xu

Training Infrastructure: Shuai Zheng, Rand Xie
培训基础设施：郑帅、谢彦宏

Hardware: Sergii Tiugaiev, Kells Kearney, Alex Shylo
硬件： Sergii Tiugaiev， Kells Kearney， Alex Shylo

We would like to thank our customers for their constructive feedback and the excellent technical support from our friends at NVIDIA, Arc Compute, eStruxture, Crusoe, AWS and Scaleway.
我们要感谢客户的建设性反馈，以及 NVIDIA、Arc Compute、eStruxture、Crusoe、AWS 和 Scaleway 等朋友的出色技术支持。

Performance on Reward Bench
在奖励台上的表现
Model	Reward Bench score Reward Bench 分数
Higgs Judger 希格斯评判者	88.1
Gemini 1.5 Pro (05/14)
双子座 1.5 Pro （05/14）	88.1
GPT-4 Turbo (04/09) GPT-4 涡轮增压 （04/09）	85.1
GPT-4o GPT-4o的	84.7
Claude 3.5 Sonnet 克劳德 3.5 十四行诗	83.8
Claude 3 Opus 克劳德 3 作品	80.7
Performance on Arena-Hard
在竞技场上的表现 - 硬
Model	Arena-Hard 竞技场-硬
Claude 3.5 Sonnet 克劳德 3.5 十四行诗	79.3
GPT-4o GPT-4o的	79.2
Higgs Llama 3 70B v2
希格斯骆驼 3 70B v2	78.6
GPT-4 Turbo (01/25) GPT-4 涡轮增压 （01/25）	78.0
Gemini 1.5 Pro 双子座 1.5 Pro	72.0
Claude 3 Opus 克劳德 3 作品	60.4
Higgs Llama 3 70B 3
希格斯骆驼 3 70B 3	49.6
Claude 3 Sonnet 克劳德 3 十四行诗	46.8
Llama 3 70B Instruct
骆驼 3 70B 指导	41.1
Mistral Large 米斯特拉尔大号	37.7
Performance on AlpacaEval 2.0
AlpacaEval 2.0 的性能
Model	AlpacaEval 2.0 羊驼Eval 2.0
GPT-4o GPT-4o的	57.5
Higgs Llama 3 70B v2
希格斯骆驼 3 70B v2	56.7
GPT-4 Turbo (04/09) GPT-4 涡轮增压 （04/09）	55.0
Claude 3.5 Sonnet 克劳德 3.5 十四行诗	52.4
Claude 3 Opus 克劳德 3 作品	40.5
Higgs Llama 3 70B
希格斯骆驼 3 70B	38.6
Claude 3 Sonnet 克劳德 3 十四行诗	34.9
Llama 3 70B Instruct
骆驼 3 70B 指导	34.4
Mistral Large 米斯特拉尔大号	32.7
Performance on MMLU Pro MMLU Pro 的性能
Model	MMLU-Pro MMLU-Pro型
GPT-4o GPT-4o的	72.6
Gemini 1.5 Pro 双子座 1.5 Pro	69.0
Claude 3 Opus 克劳德 3 作品	68.5
GPT-4 Turbo GPT-4 涡轮增压	63.7
Higgs Llama 3 70B
希格斯骆驼 3 70B	63.2
Higgs Llama 3 70B v2
希格斯骆驼 3 70B v2	62.8
Gemini 1.5 Flash Gemini 1.5 闪存	59.1
Claude 3 Sonnet 克劳德 3 十四行诗	56.8
Llama 3 70B Instruct
骆驼 3 70B 指导	56.2
Footnotes 脚注
The rate a user regenerates the response from the model. ↩
用户从模型重新生成响应的速率。↩

The percentage of new users who returns back next day. ↩
第二天返回的新用户所占的百分比。↩

[1]: https://boson.ai/higgs-opensource/
[2]: https://boson.ai/higgs-v2/



<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-11-06 06:21:57
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-11-06 06:36:53
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# moreGPT

## AutoGPT

Andrej Karpathy 在 2017 年提出的 Software 2.0：基于神经网络的软件设计。真的很有前瞻性了。这进一步引出了当前正在迅速发展的 Agent Ecosystem。AutoGPT ，BabyAGI 和 HuggingGPT 这些项目形象生动地为我们展示了 LLM 的潜力除了在生成内容、故事、论文等方面，它还具有强大的通用问题解决能力。如果说 ChatGPT 本身的突破体现在人们意识到语言可以成为一种服务，成为人和机器之间最自然的沟通接口，这一轮新发展的关键在于人们意识到语言（不一定是自然语言，也包括命令、代码、错误信息）也是模型和自身、模型和模型以及模型和外部世界之间最自然的接口，让 AI agent 在思考和表达之外增加了调度、结果反馈和自我修正这些新的功能模块。于是在人类用自然语言给 AI 定义任务目标（只有这一步在实质上需要人类参与）之后可以形成一个自动运行的循环：

1. agent 通过语言思考，分解目标为子任务
1. agent 检讨自己的计划
1. agent 通过代码语言把子任务分配给别的模型，或者分配给第三方服务，或者分配给自己来执行
1. agent 观察执行的结果，根据结果思考下一步计划，回到循环开始

原生的 ChatGPT 让人和 AI 交流成为可能，相当于数学归纳法里 n=0 那一步。而新的 agent ecosystem 实现的是 AI 和自己或者其他 AI 或者外部世界交流，相当于数学归纳法里从 n 推出 n+1 那一步，于是新的维度被展开了。比如将机器人强大的机械控制能力和目前 GPT-4 的推理与多模态能力结合，也许科幻小说中的机器人将在不久成为现实。

与Chains依赖人脑思考并固化推理过程的做法不同，AutoGPT是一个基于GPT-4语言模型的、实验性的开源应用程序，可以根据用户给定的目标，自动生成所需的提示，并执行多步骤的项目，无需人类的干预和指导（自己给自己提示）。AutoGPT的本质是一个自主的AI代理，可以利用互联网、记忆、文件等资源，来实现各种类型和领域的任务。这意味着它可以扫描互联网或执行用户计算机能够执行的任何命令，然后将其返回给GPT-4，以判断它是否正确以及接下来要做什么。下面举一个简单的例子，来说明AutoGPT的运行流程。假设我们想让AutoGPT帮我们写一篇关于太空的文章，我们可以给它这样的一个目标：“写一篇关于太空的文章”。然后AutoGPT会开始运行，它会这样做：

1. AutoGPT会先在PINECONE里面查找有没有已经写好的关于太空的文章，如果有，它就会直接把文章展示给我们，如果没有，它就会继续下一步。
1. AutoGPT会用GPT-4来生成一个提示，比如说：“太空是什么？”，然后用GPT-4来回答这个提示，比如说：“太空是指地球大气层之外的空间，它包含了许多星球，卫星，彗星，小行星等天体。”
1. AutoGPT会把生成的提示和回答都存储在PINECONE里面，并且用它们来作为文章的第一段。
1. AutoGPT会继续用GPT-4来生成新的提示，比如说：“太空有什么特点？”，然后用GPT-4来回答这个提示，比如说：“太空有很多特点，比如说，太空没有空气，没有重力，没有声音，温度变化很大等等。”
1. AutoGPT会把生成的提示和回答都存储在PINECONE里面，并且用它们来作为文章的第二段。
1. AutoGPT会重复这个过程，直到它觉得文章已经足够长或者足够完整了，或者达到了一定的字数限制或者时间限制。
1. AutoGPT会把最终生成的文章展示给我们，并且询问我们是否满意。如果我们满意，它就会结束运行；如果我们不满意，它就会根据我们的反馈来修改或者补充文章。[1]

## MetaGPT

https://xie.infoq.cn/article/4dfd90ad9ebc0bdfb4ea1b01f

## ChatGPT for Robotics

Sai Vemprala, ChatGPT for Robotics: Design Principles and Model Abilities, Microsoft, 2023

[1]: https://qiankunli.github.io/2023/10/30/llm_agent.html

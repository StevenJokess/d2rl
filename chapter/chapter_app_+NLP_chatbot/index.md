

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-25 23:19:44
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2024-06-22 10:21:04
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 应用：聊天机器人（ChatBot）

本章介绍强化学习的应用之一——聊天机器人，包括：

![基于神经网络的语言模型演进历程](../../../img/NN_based_language_model_developing.png)

- NLP：自然语言处理的历史发展
- LM(RNN_LSTM_GRU)：语言模型、RNN、LSTM、GRU
- Attention_Mechanism：注意力机制。自注意力机制，是之后Transformer 模型强大的秘诀
- Encoder-Decoder&Seq2Seq：Encoder-Decoder 模型
- Pretrain_LM(NNLM_Word2Vec_Glove)：预训练
- Pretrain_LLM(Transformer)：大语言模型、经典大语言模型 Transformer（其后续影响了ELMo、GPT、BERT）
- ELMo：
- GPT-1：2018年6月提出的模型，在9个NLP任务上取得了 SOTA 的效果。[3]
- BERT：
- GPT-2：2019年被提出，也就是该年OpenAI转为有限营利公司。[3]
- Prompt_Learning(Prompt_Tuning)：预训练语言模型加持下的Prompt Learning成为了NLP的第四范式
- GPT-3：GPT-3论文、API、[2]思维链（COT）、提示词（Prompt）；
- Codex&Github_CoPilot：GPT应用于代码生成。1月，GPT-3.5 API (text-davinci-002)发布，该模型经过Github代码的训练加持，推理能力显著提升（该假设的因果关系待学术界论证），经过Alignment技术的加持，Follow人类指令的能力显著提升，输出结果有用性和无害性显著提升。3月，GPT-3.5论文发布，公开Alignment算法。5月，OpenAI Codex已经被70个应用使用，包括微软收购的Github的Copilot.
- RL_in_NLP：回顾强化学习在自然语言处理中的应用
- GPT-3.5(+RLHF=InstructGPT)：从人类反馈强化学习（RLHF），并将其应用到GPT。
- ChatGPT&Newbing：12月1日，ChatGPT发布。Musk等名流开始谈论ChatGPT，引爆英文互联网。12月初，中国互联网的自媒体逐渐开始讨论ChatGPT，主要以翻译twitter的方式。知乎上有学者开始反思。一周后，关注指数下降，两个月来只剩下AI自媒体把ChatGPT作为自己的主要关注内容。2023年1月，微软宣布投资OpenAI数十亿美元，并将GPT加入全家桶。2月，中国春节结束，微软和Google你方唱罢我登场，纳斯达克财报季，AI被反复提起。中国互联网是认识微软的，ChatGPT引爆中国互联网，关注指数飙升。GPT应用于聊天机器人、搜索。
- GPT-4：介绍 ChatGPT Plugins、GPT4V、GPTstore、GPT4-Turbo
- more_about_ChatGPT：介绍AutoGPT、MetaGPT、ChatGPT for Robotics
- Safety_AGI:
- other_LLM：介绍外国的大语言模型，开源有LLaMA、Alpaca；不开源的有Claude、Gemini等；介绍中国的大语言模型，开源ChatRWKV等；不开源的
- ChatGPT_PUA_Ilya：ilya离职后我的PUA。

？ NLG(by_NN_for_Dia)

```toc
:maxdepth: 2

NLP

LM(RNN_LSTM_GRU)
LLM(Transformer)
Bert_VS_GPT
LLM(Transformer)



```

TODO: https://blog.csdn.net/qq_56591814/article/details/130542583
[2]: https://chinadigitaltimes.net/chinese/692793.html
[3]: https://www.zhihu.com/people/wen-liang-85-30/posts

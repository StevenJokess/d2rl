

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-22 21:27:29
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-13 03:37:20
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 强化学习可被攻击

强化学习也可以被攻击，根据UC伯克利大学，OpenAI和宾夕法尼亚大学的研究人员发表的论文《Adversarial Attacks on Neural Network Policies》[插图]以及内华达大学的论文《Vulnerability of Deep Reinforcement Learning to Policy Induction Attacks》[插图]显示，广泛使用的强化学习算法，比如DQN、TRPO和A3C，在这种攻击面前都十分脆弱。即便是人类难以观察出来的微妙的干扰因素，也能导致系统性能减弱。比如引发一个智能体让乒乓球拍在本该下降时反而上升[插图]。Dawn Song在文献[插图]中进行了定量的描述，场景为使用强化学习玩Pong游戏，如图14-14所示。通常玩Pong这类型游戏，需要使用强化学习里面的DQN，如图14-15所示，本质上也是处理图像然后进行神经网络计算得出各个动作对应的概率值，这点非常类似于图像分类的过程，所以可以使用FGSM算法对强化学习模型进行攻击。通过实验表明，使用FGSM可以显著影响其得分，如图14-16所示。[1]

[1]: https://weread.qq.com/web/reader/4b732a405e2f7e4b71870b4k43e327b025143ec517d680b
TODO: http://www.aas.net.cn/cn/article/doi/10.16383/j.aas.c200166?viewType=HTML

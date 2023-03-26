# 结合RNN

对于时间序列信息, 深度Q网络的处理方法是加入经验回放机制. 但是经验回放的记忆能力有限, 每个决策点需要获取整个输入画面进行感知记忆。

Hausknecht等将长短时记忆网络与深度Q网络结合，提出深度递归Q 网络 (deep recurrent Q network, DRQN), 在部分可观测马尔科夫决策过程(partially observable Markov decision process, POMDP)中表现出了更好的鲁棒性, 同时在缺失若干帧画面的情况下也能获得很好的实验结果[1]

[1]: http://pg.jrj.com.cn/acc/Res/CN_RES/INDUS/2023/2/9/27c20431-8ed3-4562-83b5-5c82706f28a5.pdf



<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-17 05:15:29
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-03-17 05:27:57
 * @Description:
 * @Help me: 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 资格迹

https://chengfeng96.com/blog/2020/02/24/%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%EF%BC%88%E4%B8%83%EF%BC%89-%E8%B5%84%E6%A0%BC%E8%BF%B9/


### TD($\lambda$)

TD($\lambda$)为了避免n-step TD不知道怎么选n的问题，引入了参数λ以便于调超参，sampling时G(t)变成

$$G_{t}^{\lambda}=(1-\lambda) \sum_{n=1}^{\infty} \lambda^{n-1} G_{t}^{(n)}$$

### SARSA ($\lambda$)

SARSA($\lambda$)算法是在SARSA算法的基础上引入了「资格迹（eligibility trace）」，直观上解释就是让算法对于经历过的状态有一定的记忆性，如sutton书中所述，资格迹对所获取的轨迹起到了短期记忆的效果。从下图可以直观看出，经历过的状态不再是经历过之后直接删除，而是存在一个「平滑的衰减过渡」，保存了一定程度上的信息。也可以说，该算法增加了距离目标点最近的状态的权重，从而加快算法的收敛性（直观意义上的说法）。[8]

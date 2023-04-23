

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-04-16 21:41:00
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-16 21:41:45
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 生成对抗模仿学习

[2]

## GAIL与GAN

- 生成对抗模仿学习 (GAIL)

$$
\min _\pi \max _D \mathbb{E}_{(s, a) \sim \pi_E}[\log D(s, a)]+\mathbb{E}_{(s, a) \sim \pi}[\log (1-D(s, a))]-\lambda H(\pi)
$$

- 生成对抗网络 (GAN)

$$
\min _G \max _D \mathbb{E}_{x \in p_{\text {data }}(x)}[\log D(x)]+\mathbb{E}_{z \sim p_z(z)}[\log (1-D(G(z)))]
$$

判别器 $D$ 判别数据分布是由生成器 $G$ (即生成对抗模仿学习中的 $\pi$ ) 还是 真实数据分布 (即生成对抗学习中的 $\pi_E$ ) 产生

[1]: https://papers.nips.cc/paper_files/paper/2016/hash/cc7e2b878868cbae992d1fb743995d8f-Abstract.html
[2]: https://boyuai.oss-cn-shanghai.aliyuncs.com/disk/%E5%8A%A8%E6%89%8B%E5%AD%A6%E7%B3%BB%E5%88%97/%E5%8A%A8%E6%89%8B%E5%AD%A6%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0/%E8%AF%BE%E4%BB%B6pdf/10-%E6%A8%A1%E4%BB%BF%E5%AD%A6%E4%B9%A0.pdf

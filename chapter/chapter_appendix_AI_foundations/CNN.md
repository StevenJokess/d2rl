# 卷积神经网络

## 卷积神经网络的输出形状

若

- 输入形状为 $n_h \times n_w$
- 卷积核形状为 $k_h \times k_w$
- 上下各填充 $p_h$ 行，左右各填充 $p_w$ 列
- 垂直步幅 $s_h$ ，水平步幅 $s_w$

则输出形状为
$$
\left\lfloor\frac{n_h+2 \times p_h-k_h}{s_h}+1\right\rfloor \times\left\lfloor\frac{n_w+2 \times p_w-k_w}{s_w}+1\right\rfloor
$$
torch.nn.Conv2d

[1]: https://kibazen.cn/li-jie-qiang-hua-xue-xi-zhong-de-ji-ben-gai-nian/#toc-heading-10

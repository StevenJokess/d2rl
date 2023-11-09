# 微积分

- 极限
- 连续
- 微分
- 向量求导
- 梯度

## 导数

导数的定义: 设函数 $y=f(x)$ 在点 $x_0$ 的某个邻域内有定义，当自变量 $x$ 在 $x_0$ 处取得增量 $\triangle x$ (点 $x+\triangle x$ 仍在该邻域内) 时，若存在极限
$$
f^{\prime}\left(x_0\right)=\lim _{\triangle x \rightarrow 0} \frac{f\left(x_0+\triangle x\right)-f\left(x_0\right)}{\triangle x}
$$

我们称函数 $f(x)$ 在 $x_0$ 处可导，且导数为 $f^{\prime}\left(x_0\right)$ 。

## 方向导数


方向导数的定义: 如果函数 $f(x, y)$ 在点 $\left(x_0, y_0\right)$ 可微，那么函数在该点沿任一方向 $\vec{\ell}$ 方向的方向导数存在，且有

$$
\left.\frac{\partial f}{\partial \vec{\ell}}\right|_{\left(x_0, y_0\right)}=f_x\left(x_0, y_0\right) \cos \alpha+f_y\left(x_0, y_0\right) \cos \beta
$$

其中 $\cos \alpha$ 和 $\cos \beta$ 是方向 $\vec{\ell}$ 的方向余弦。

## 梯度

梯度的定义: 二元函数 $f(x, y)$ 在点 $\left(x_0, y_0\right)$ 处的梯度定义为
$$
\nabla f\left(x_0, y_0\right)=f_x\left(x_0, y_0\right) \vec{i}+f_y\left(x_0, y_0\right) \vec{j}
$$

## 总结

- 导数是一元函数所特有的概念。对于多元函数，有偏导数概念。
- 方向导数是数值概念，没有方向。
- 梯度的模值等于最大的方向导数。
- 梯度有大小也有方向。

[1]: https://www.boyuai.com/elites/
[2]: https://www.qiuyun-blog.cn/2018/11/27/%E5%AF%BC%E6%95%B0%E3%80%81%E6%96%B9%E5%90%91%E5%AF%BC%E6%95%B0%E5%92%8C%E6%A2%AF%E5%BA%A6/

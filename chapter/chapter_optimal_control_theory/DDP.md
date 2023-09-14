

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-09-14 03:51:38
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-09-14 03:52:28
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# DDP

- DDP (Differential Dynamics Programming) 为了完整性，把iLQR中的dynamics model加了个二阶近似项，此处并不提及DDP的算法流程。
$$
\begin{aligned}
& f\left(\mathbf{x}_t, \mathbf{u}_t\right) \approx f\left(\hat{\mathbf{x}}_t, \hat{\mathbf{u}}_t\right)+\nabla_{\mathbf{x}_t, \mathbf{u}_t} f\left(\hat{\mathbf{x}}_t, \hat{\mathbf{u}}_t\right)\left[\begin{array}{l}
\delta \mathbf{x}_t \\
\delta \mathbf{u}_t
\end{array}\right] \\
& f\left(\mathbf{x}_t, \mathbf{u}_t\right) \approx f\left(\hat{\mathbf{x}}_t, \hat{\mathbf{u}}_t\right)+\nabla_{\mathbf{x}_t, \mathbf{u}_t} f\left(\hat{\mathbf{x}}_t, \hat{\mathbf{u}}_t\right)\left[\begin{array}{l}
\delta \mathbf{x}_t \\
\delta \mathbf{u}_t
\end{array}\right]+\frac{1}{2}\left(\nabla_{\mathbf{x}_t, \mathbf{u}_t}^2 f\left(\hat{\mathbf{x}}_t, \hat{\mathbf{u}}_t\right) \cdot\left[\begin{array}{l}
\delta \mathbf{x}_t \\
\delta \mathbf{u}_t
\end{array}\right]\right)\left[\begin{array}{l}
\delta \mathbf{x}_t \\
\delta \mathbf{u}_t
\end{array}\right]
\end{aligned}
$$



<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-03 04:19:29
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-03 04:20:11
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->


3. 把计算出的梯度 $\tilde{\boldsymbol{g}}^k$ 发送给服务器。
服务器端的计算：服务器上储存一份模型参数, 并且用 Worker 发来的梯度更新参 数。每当收到一个Worker (比如第 $k$ 号 Worker) 发送来的梯度 (记作 $\tilde{\boldsymbol{g}}^k$ ), 服务器就立 刻做梯度下降更新参数：
$$
\boldsymbol{w}_{\text {new }} \leftarrow \boldsymbol{w}_{\text {now }}-\eta \cdot \tilde{\boldsymbol{g}}^k .
$$
服务器还需要监听 Worker 发送的请求。如果有 Worker 索要参数, 就把当前的参数 $\boldsymbol{w}_{\text {new }}$ 发送给这个 Worker。

## 同步与异步梯度下降的对比

上一节介绍的同步并行梯度下降完全等价于标准的梯度下降, 只是把计算分配到了 多个 Worker 节点上而已。然而异步梯度下降算法与标准的梯度下降是不等价的。同步与 异步梯度下降不只是编程实现有区别, 更是在算法上有本质区别。
1. 不难证明，同步并行梯度下降更新参数的方式为：
$$
\boldsymbol{w}_{\text {new }} \leftarrow \boldsymbol{w}_{\text {now }}-\eta \cdot \nabla_{\boldsymbol{w}} L\left(\boldsymbol{w}_{\text {now }}\right),
$$
即标准的梯度下降。在同一时刻, 所有 Worker 节点上的参数是相同的, 都是 $\boldsymbol{w}_{\text {now }}$ 。 所有 Worker 节点都基于相同的 $\boldsymbol{w}_{\text {now }}$ 计算梯度。
2. 对于异步并行梯度下降, 在同一时刻, 不同 Worker 节点上的参数 $\boldsymbol{w}$ 通常是不同的。 比如两个 Worker 分别在 $t_1$ 和 $t_2$ 时刻向服务器索要参数。在两个时刻之间, 服务器 可能已经对参数做了多次更新, 导致在 $t_1$ 和 $t_2$ 时刻取回的参数不同。两个 Worker 节点会基于不同的参数计算梯度。
在理论上, 异步梯度下降的收玫速度慢于同步算法, 即需要更多的计算量才能达到 相同的精度。但是实践中异步梯度下降远比同步算法快 (指的是钟表时间), 这是因为异 步算法无需等待, Worker 节点几乎不会空闲, 利用率很高。

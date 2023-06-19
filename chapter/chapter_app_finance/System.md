

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-06-17 22:51:14
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-06-17 22:51:36
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 交易系统

## 什么是策略回测

策略回测，就是对交易策略进行历史数据的回溯测试。比如，一个交易策略写好后，我们可以跑下过去几年的历史数据，看看策略在过去几年的表现，并可以进行参数的优化。策略回测是建立在历史会重演的基础上进行的。

回测系统一般是用Python来写的。

## 回测系统的构成

回测，是对历史数据的测试，所以，首先要有历史数据。历史数据可以保存在数据库里面，比如MySQL，也可以保存在文件里面，比如CSV文件。

回测系统的第一个构成就是读取数据模块，用来读取数据库或文件里面的历史行情数据。除了读取行情数据，策略的参数和交易品种的参数，都是要读取进来的。

回测系统的第二个构成是策略计算模块，这是一个核心的模块。策略计算模块，主要功能是进行策略逻辑的计算，算出策略在历史数据中的回报。如果需要优化，可以在这里更改策略逻辑。

回测系统的第三个构成是信息输出模块，包括策略的收益、收益率、参数组等信息，需要输出到外部文件或数据库。

以上是回测系统的三大模块，每个模块，还可以细分成几个小的功能模块。

[1]: http://www.quantceo.com/?cat=18
[2]: http://www.quantceo.com/?cat=289

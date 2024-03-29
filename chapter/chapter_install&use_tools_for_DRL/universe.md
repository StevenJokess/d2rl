

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-10-03 02:41:50
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-10-08 01:37:12
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请资助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# Universe

2016年年底，继4月发布Gym之后，OpenAI又推出
一个新平台——Universe（见图14.18）。Universe的目标是评估和训练通用AI。同Gym上的定制游戏不同，Universe瞄准的环境是世界范围的各种游戏、网页及其他应用，与人类一样面对相同复杂和实时程度的环境，至少在信息世界这个层面345
上，物理世界还有待传感器和硬件的进步。具体地讲，游戏程序被打包到一个Docker容器里，提供给外部的接口，人与机器一样的，谁都不能访问游戏程序的内部，只能接收屏幕上的画面，和发送键盘和鼠标指令。

图14.18 OpenAI 开发的通用AI平台Universe示意图
Universe的目标是让设计者开发单一的智能体，去完成Universe中的各类游戏和任务。当一个陌生游戏和任务出现时，智能体可以借助过往经验，快速地适应并执行新的游戏和任务。我们都知道，虽然AlphaGo  击败了人类世界围棋冠军，
但是它仍然属于狭义AI，即可以在特定领域实现超人的表现，但缺乏领域外执行任务的能力，就像AlphaGo不能陪你一起玩其他游戏。为了实现具有解决一般问题能力的系统，就要让AI拥有人类常识，这样才能够快速解决新的任务。因此，智能体需要携带经验到新任务中，而不能采用传统的训练步骤，初始化为全随机数，然后不断试错，重新学习参数。这或许是迈向通用  AI的重要一步，所以我们必须让智能体去经历一系列不同的任务，以便它能发展出关于世界的认知以及解决问题的通用策略，并在新任务中得到使用。

最典型的任务就是基于浏览器窗口的各项任务。互联网是一个蕴藏丰富信息的大宝藏。Universe提供了一个浏览器环境，要求AI能浏览网页并在网页间导航，像人类一样使用显示器、键盘和鼠标。当前的主要任务是学习与各类网页元素交互，如点击按钮、下拉菜单等。将来，AI可以完成更复杂的任务，如搜索、购物、预定航班等。

## OpenAI Universe安装

```bash
git clone https://github.com/openai/universe.git
cd universe
pip install -e .
```

[1]:[ ](https://blog.csdn.net/QFire/article/details/91490383)

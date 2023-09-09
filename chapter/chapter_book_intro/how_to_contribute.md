

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-03-22 02:22:18
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-04-12 20:55:28
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# 如何提交 Pull Request

**准备工作**

1. 在原始代码库上点 Fork ，在自己的账户下开一个分支代码库
2. 将自己的分支克隆到本地
   * `git clone https://github.com/(YOUR_GIT_NAME)/d2rl.git`
3. 将本机自己的 fork 的代码库和 GitHub 上原始作者的代码库 ，即上游（ upstream ）连接起来
   * `git remote add upstream https://github.com/StevenJokess/d2rl.git`

**提交代码**

1. 每次修改之前，先将自己的本地分支同步到上游分支的最新状态
   * `git pull upstream master`
2. 作出修改后 push 到自己名下的代码库
3. 在 GitHub 网页端自己的账户下看到最新修改后点击 New pull request 即可

[1]: https://raw.githubusercontent.com/jindongwang/transferlearning-tutorial/master/README.md

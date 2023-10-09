

<!--
 * @version:
 * @Author:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @Date: 2023-05-12 01:44:22
 * @LastEditors:  StevenJokess（蔡舒起） https://github.com/StevenJokess
 * @LastEditTime: 2023-05-12 02:20:49
 * @Description:
 * @Help me: make friends by a867907127@gmail.com and help me get some “foreign” things or service I need in life; 如有帮助，请赞助，失业3年了。![支付宝收款码](https://github.com/StevenJokess/d2rl/blob/master/img/%E6%94%B6.jpg)
 * @TODO::
 * @Reference:
-->
# tic-tac-toe

就是三子棋。3 * 3棋盘，交替下，谁先连成3子，谁胜利。[3]

## 复杂度估算

以tic-tac-toe（三子连珠棋）为例，估算了此博弈问题的状态复杂度和博弈树复杂度。tic-tac-toe 共有9 个位置可以落子，能够形成的局面较少，因此其复杂度的估算相对容易。

### 具体估算过程如下：

具体估算过程如下：

（1）对于其状态复杂度，由于棋盘上每个位置有三种状态（双方的棋子和空白），因此，状态复杂度可估算为3^9，根据此博弈问题的走棋规则，在棋盘上形成连3则游戏结束，出现两个以上的连3 的局面属于非法局面。而对称相同的多个局面应该只算作一个局面。将这些考虑在内，则更精确的状态复杂度为5478；
（2）对于其博弈树复杂度，平均深度约为9，第i（1≤ i ≤9）层时，走棋方可能的走法有9-i 个，因此，此博弈树的叶子节点数（即博弈树复杂度）为9！。[1]

## 第一款成功下棋的软件

第一款成功下棋的软件诞生于1952年，记录在道格拉斯的博士论文中，玩的正是最简单的Tic-Tac-Toe游戏。[2]

##


作者：111辄
链接：https://zhuanlan.zhihu.com/p/655405874
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

代码概览  类的定义：定义了State、Judger、Player、HumanPlayer四个类，分别代表棋局状态、下棋(裁判)、AI棋手、人类棋手  状态s：每个棋局是一种状态，使用hash标识唯一的状态。共3^9种状态  训练过程：首先让2个AI棋手对战，以逐渐完善策略(价值状态函数)。AI棋手训练完后，让AI棋手和人类棋手对战  训练AI棋手时：初始时，设置胜局状态value为1，输局状态value为0，其余为0.5。然后backup更新，即利用 V(s) ⬅ V(s) + α[V(s')-V(s)] 不断修正value，直到逐渐收敛。α是步长代码讲解main主函数先来看下主函数：if __name__ == '__main__':
    train(int(1e5))   # 1e5是浮点型。是epoch  # 2AI对战，完善value function
    compete(int(1e3))   # 2AI对战训练完后，再对战自测胜率
    play()   # 人类和AI对战
总之就是先AI对战，再人机大战State状态类引入包并定义3*3棋局：import numpy as np
import pickle

BOARD_ROWS = 3
BOARD_COLS = 3
BOARD_SIZE = BOARD_ROWS * BOARD_COLS
再进行State状态类的定义。State类包括函数：  __init__()初始化、  hash()计算每个状态的哈希值以索引、  is_end()检查棋局是否结束、  next_station()函数将棋手标志放至下一个下棋位置上、  print()打印当前3*3棋局 详细注释见代码：# 每一个state是棋盘的整个状态，共3^9个状态
class State:
    def __init__(self):
        # 1 symbol： 先行player
        # -1 symbol：后行player
        # 0 symbol：empty position
        self.data = np.zeros((BOARD_ROWS, BOARD_COLS))  # 代表board
        self.winner = None
        self.hash_val = None    # 使用hash标识每个状态
        self.end = None

    # 计算每个状态的哈希值（规则随机）
    def hash(self):
        if self.hash_val is None:
            self.hash_val = 0
            for i in self.data.reshape(BOARD_COLS * BOARD_ROWS):
                if i == -1:
                    i = 2
                self.hash_val = self.hash_val * 3 + i
        return int(self.hash_val)

    # 检查游戏是否分出胜负，或是平局
    def is_end(self):
        if self.end is not None:
            return self.end

        results = []
        # check row
        for i in range(0, BOARD_ROWS):
            results.append(np.sum(self.data[i, :]))
        # check columns
        for i in range(0, BOARD_COLS):
            results.append(np.sum(self.data[i, :]))
        # check diagnoals
        results.append(0)
        for i in range(0, BOARD_ROWS):
            results[-1] += self.data[i, i]
        results.append(0)
        for i in range(0, BOARD_COLS):
            results[-1] += self.data[i, BOARD_ROWS -1 - i]

        for result in results:
            if result == 3:
                self.end = True
                self.winner = 1
                return self.end
            if result == -3:
                self.end = True
                self.winner = -1
                return self.end

        # check tie
        sum = np.sum(np.abs(self.data))
        if sum == BOARD_COLS * BOARD_ROWS:
            self.end = True
            self.winner = 0  # 平局
            return self.end

        # 非胜负/平局，继续游戏
        self.end = False
        return self.end

    # 下一个状态
    # 将棋手标志放置board位置(i, j)
    def next_station(self, i, j, symbol):
        new_state = State()
        new_state.data = np.copy(self.data)
        new_state.data[i, j] = symbol
        return new_state

    # 打印棋局
    def print(self):
        for i in range(0, BOARD_ROWS):
            print('-----------------')
            out = '| '
            for j in range(0, BOARD_COLS):
                if self.data[i, j] == 1:
                    token = '*'
                if self.data[i, j] == -1:
                    token = 'x'
                if self.data[i, j] == 0:
                    token = '0'
                out += token + ' | '
            print(out)
        print('-----------------')
# 检索当前状态下，所有下一个可能动作带来的状态变换
def get_all_states_impl(current_state, current_symbol, all_states):
    for i in range(0, BOARD_ROWS):
        for j in range(0, BOARD_COLS):
            if current_state.data[i][j] == 0:   # 检索目前所有空格子
                newState = current_state.next_state(i, j, current_symbol)
                newHash = newState.hash()
                if newHash not in all_states.keys():
                    isEnd = newState.is_end()
                    all_states[newHash] = (newState, isEnd)
                    if not isEnd:   # 如果棋手1下完还没结束棋局，棋手2下
                        get_all_states_impl(newState, -current_symbol, all_states)

def get_all_states():
    current_symbol = 1
    current_state = State()
    all_states = dict()
    all_states[current_state.hash()] = (current_state, current_state.is_end())
    get_all_states_impl(current_state, current_symbol, all_states)
    return all_states

# all_states字典：key是某状态对应的唯一哈希值，value是(state,isEnd)
all_states = get_all_states()
Judger裁判类Judger类是裁判，其实就是两个棋手轮流下棋。包括函数：  __init__() 初始化  reset() 重置  alternate() 轮流选择下棋手  play() 双方下棋详细注释见代码：class Judger:
    def __init__(self, player1, player2):
        self.p1 = player1
        self.p2 = player2
        self.current_player = None
        self.p1_symbol = 1
        self.p2_symbol = -1
        self.p1.set_symbol(self.p1_symbol)
        self.p2.set_symbol(self.p2_symbol)

    def reset(self):
        self.p1.reset()
        self.p2.reset()

    def alternate(self):
        while True:
            yield self.p1
            yield self.p2

    # play函数用于双方轮流下棋（这个play函数是两个AI棋手下），
    # act函数用于为当前player选择value最高的下棋位置
    # @print:if True, print each board during the game
    def play(self, print = False):
        alternator = self.alternate()
        self.reset()
        current_state = State()
        self.p1.set_state(current_state)
        self.p1.set_state(current_state)
        while True: # 一直到棋局结束，return了才结束循环
            player = next(alternator)   # 双方轮流下棋
            if print:
                current_state.print()
                [i, j, symbol] = player.act()    # 为棋手选择下一步最佳落子
            next_state_hash = current_state.next_state(i, j, symbol)
            current_state, is_end = all_states[next_state_hash]
            self.p1.set_state(current_state)
            self.p2.set_state(current_state)
            if is_end:
                if print:
                    current_state.print()
                return current_state.winner
Player棋手(AI)类Player是AI棋手类。包括函数：  __init__() 初始化  reset() 重置  set_state() 设置状态及是否explore  set_symbol() 状态价值初始化赋值  backup() 反向更新状态价值：V(s) ⬅ V(s) + α[V(s')-V(s)]  act() 当前state下，选择最优action  save_policy() 保存策略(就是estimations价值)  load_policy() 加载策略详细注释见代码：# AI player
# 关于value function(state的函数)解释：https://face2ai.com/RL-RSAB-1-5-An-Extended-Example/
class Player:
    # @step_size: the step size to update estimation
    # (好像就是value function)，back up更新里的α，详见上方链接解释
    # @epsilon: the probability to explore
    def __init__(self, step_size = 0.1, epsilon = 0.1):
        self.estimations = dict()
        self.step_size = step_size
        self.epsilon = epsilon
        self.states = []
        self.greedy = []

    def reset(self):
        self.states = []
        self.greedy = []

    def set_state(self, state):
        self.states.append(state)
        self.greedy.append(True)    # 应该是exploit而完全不explore

    # 这个函数就是给状态state赋value的(estimation字典，key为hash(对应某个状态)，值为value)，
    # 在棋局结束状态下，如果这个状态赢，赋1；若平局，赋0.5，输则赋0
    # 正在进行时的棋局状态一律赋0.5
    def set_symbol(self, symbol):
        self.symbol = symbol
        for hash_val in all_states.keys():
            (state, is_end) = all_states[hash_val]
            if is_end:
                if state.winner == self.symbol:
                    self.estimations[hash_val] = 1.0
                elif state.winner == 0:
                    self.estimations[hash_val] = 0.5
                else:
                    self.estimations[hash_val] = 0
            else:
                self.estimations[hash_val] = 0.5

    # 反向更新价值状态函数（value estimation）
    def backup(self):
        self.states = [state.hash() for state in self.states]

        # 反向更新：V(s) ⬅ V(s) + α[V(s')-V(s)]
        # 可参考链接：https://face2ai.com/RL-RSAB-1-5-An-Extended-Example/
        for i in reversed(range(len(self.states) - 1)):
            state = self.states[i]
            td_error = self.greedy[i] * (self.estimations[self.states[i + 1]] - self.estimations[state])
            self.estimations[state] += self.step_size * td_error

    # 当前state下，选择下一步的最优action
    # act函数的返回结果为下棋位置和棋手标志(i, j, symbol)
    def act(self):
        state = self.states[-1]
        next_states = []
        next_positions = []

        # 找出目前state下所有空位
        for i in range(BOARD_ROWS):
            for j in range(BOARD_COLS):
                if state.data[i, j] == 0:
                    next_positions.append([i,j])
                    next_states.append(state.next_state(i, j, self.symbol).hash())

        # explore
        if np.random.rand() < self.epsilon:    # 随机生成(0,1)之间数
            action = next_positions[np.random.randint(len(next_positions))]
            action.append(self.symbol)
            self.greedy[-1] = False
            return action

        # 否则exploit
        values = []
        for hash, pos in zip(next_states, next_positions):
            values.append((self.estimations[hash], pos))
        np.random.shuffle(values)
        values.sort(key = lambda x:x[0], reverse = True)    # 按照state的value值大小倒序排
        action = values[0][1]    # 选择value值最大的action的位置
        action.append(self.symbol)  # 为这个动作加上棋手标志
        return action

    def save_policy(self):
        # bin是二进制格式的文件
        with open('policy_%s.bin' % ('first' if self.symbol == 1 else 'second'), 'wb') as f:
            pickle.dump(self.estimations, f)    # 对象存储

    def load_policy(self):
        with open('policy_%s.bin' % ('first' if self.symbol == 1 else 'second'), 'rb') as f:
            self.estimations = pickle.load(f)
HumanPlayer人类棋手类HumanPlayer类就是人类棋手。包括函数：  __init__() 初始化  set_state() 设置状态  set_symbol() 设置棋手标志  act() 人类棋手通过键盘下棋详细注释见代码：# human interface
# input a number to put a chessman
# | q | w | e |
# | a | s | d |
# | z | x | c |
class HumanPlayer:
    def __init__(self, **kwargs):
        self.symbol = None
        self.keys = ['q', 'w', 'e', 'a', 's', 'd', 'z', 'x', 'c']
        self.state = None
        return

    def reset(self):
        return

    def set_state(self, state):
        self.state = state

    def set_symbol(self, symbol):
        self.symbol = symbol
        return

    def backup(self, _):
        return

    def act(self):
        self.state.print()
        key = input("Input your position:")  # 将这句话显示在屏幕上，接收用户输入的值，赋给key
        # 默认用户的输入是键盘最左边三行三列字母
        data = self.keys.index(key)  # 索引
        i = data // int(BOARD_COLS)
        j = data % BOARD_COLS
        return (i, j, self.symbol)
train函数train()其实就是让两个AI对战，不断完善value function，直至逐渐收敛。是在训练AI棋手# 2个AI间训练
# 两个AI player打，不断完善value function(即estimations)
# 训练结束后, Epoch 10000, player 1 win 0.08, player 2 win 0.03
def train(epochs):
    player1 = Player(epsilon = 0.01)
    player2 = Player(epsilon = 0.01)
    judger = Judger(player1, player2)
    player1_win = 0.0
    player2_win = 0.0
    for i in range(1, epochs + 1):
        winner = judger.play(print = False)   # 开始下棋
        if winner == 1:
            player1_win += 1
        if winner == -1:
            player2_win += 1
        print('Epoch %d, player1 win %.02f, player2 win %.02f' % (i, player1_win / i, player2_win / i))
        player1.backup()
        player2.backup()
        judger.reset()
    player1.save_policy()
    player2.save_policy()
compete函数相当于train后的test。这个游戏规则太简单了，测试时两个AI都是平局# 2个AI间测试
# 训练结束后的测试，AI之间就没有输赢了，全是平局：
# 1000 turns, player 1 win 0.00, player 2 win 0.00
# 计算下turns次棋，两个AI棋手的分别胜率
def compete(turns):
    player1 = Player(epsilon = 0)
    player2 = Player(epsilon = 0)
    judger = Judger(player1, player2)
    player1.load_policy()
    player2.load_policy()
    player1_win = 0.0
    player2_win = 0.0
    for i in range(0, turns):
        winner  = judger.play()
        if winner == 1:
            player1_vin += 1
        if winner == -1:
            player2_win += 1
        judger.reset()
    print('%d turns, player1 win %.02f, player1 win %.02f' % (turns, player1_win / turns, player2_win / turns))
play函数人类和AI下棋# 人类和AI下棋
def play():
    while True:
        player1 = HumanPlayer()
        player2 = Player(epsilon = 0)
        judger = Judger(player1, player2)
        player2.load_policy()   # AI棋手使用之前两个AI对战储存的policy（即value function，因为上面设置的epsilon为0，直接贪婪地选造成value最大状态的action）
        winner = judger.play()
        if winner == player2.symbol:
            print("You lose!")
        elif winner == player1.symbol:
            print("You win!")
        else:
            print("It is a tie!")




[1]: https://www.ambchina.com/data/upload/image/20220226/2017%E4%B8%AD%E5%9B%BD%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD%E7%B3%BB%E5%88%97%E7%99%BD%E7%9A%AE%E4%B9%A6--%E6%99%BA%E8%83%BD%E5%8D%9A%E5%BC%88-2017.pdf
[2]: https://pdf-1307664364.cos.ap-chengdu.myqcloud.com/%E6%95%99%E6%9D%90/%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0/%E3%80%8A%E7%99%BE%E9%9D%A2%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0%E7%AE%97%E6%B3%95%E5%B7%A5%E7%A8%8B%E5%B8%88%E5%B8%A6%E4%BD%A0%E5%8E%BB%E9%9D%A2%E8%AF%95%E3%80%8B%E4%B8%AD%E6%96%87PDF.pdf
[3]: https://zhuanlan.zhihu.com/p/655405874

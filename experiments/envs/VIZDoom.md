# VIZDoom

## pip 安装

Installation of Ubuntu

```
sudo apt install cmake libboost-all-dev libsdl2-dev \
libfreetype6-dev libgl1-mesa-dev libglu1-mesa-dev \
libpng-dev libjpeg-dev libbz2-dev \
libfluidsynth-dev libgme-dev libopenal-dev \
zlib1g-dev timidity tar nasm wget
#("pip3" for python3)
pip install vizdoom
```

Installation of MacOS

```
brew install cmake boost sdl2 wget
#("pip3" for python3)
pip install vizdoom
```

## 安装测试

安装成功后，测试文件如下：

```
from vizdoom import *
import random
import time
​
game = DoomGame()
game.load_config("vizdoom/scenarios/basic.cfg")
game.init()
​
shoot = [0, 0, 1]
left = [1, 0, 0]
right = [0, 1, 0]
actions = [shoot, left, right]
​
episodes = 10
for i in range(episodes):
    game.new_episode()
    while not game.is_episode_finished():
        state = game.get_state()
        img = state.screen_buffer
        misc = state.game_variables
        reward = game.make_action(random.choice(actions))
        print "\treward:", reward
        time.sleep(0.02)
    print "Result:", game.get_total_reward()
    time.sleep(2)
```

不过我遇到了 .cfg 配置文件读取不到的问题，如果遇到了可以尝试将其改成绝对路径（非常粗暴）

```
#game.load_config("vizdoom/scenarios/basic.cfg")
doom_path="/usr/local/lib/python3.6/dist/packages/vizdoom/scenarios/basic.cfg"
game.load_config(doom_path)
```


## VIZDoom 配置

VIZDoom 有丰富的配置选项，需要提前设定好

DoomGame类

该类的实体就是交互的环境，可通过如下代码制定wad文件以及相应的地图。

```
game = DoomGame()
game.set_doom_scenario_path("../../scenarios/basic.wad")
game.set_doom_map("map01")
```

wad包含了多个构建的虚拟环境，因此在指定wad文件后还要通过game.set_doom_map(“map01”)来指定具体需要哪张地图，若不指定，则默认使用第一张地图。

还可以设置显示参数：

是否显示弹夹

```
game.set_render_hud(False)
```
是否显示瞄准线

```
game.set_render_crosshair(False)
```

是否显示武器
```
game.set_render_weapon(True)
```

是否显示贴花纸

```
game.set_render_decals(False)
```

是否显示微粒（粒子特效）

```
game.set_render_particles(False)
```

此外，还可以设置整个环境的迭代回合数，以及奖励。

game.set_episode_timeout(200)
game.set_episode_start_time(10)
game.set_window_visible(True)
game.set_living_reward(-1)

ScreenFormat 与 ScreenResolution 类

设置图像分辨率

```
game.set_screen_resolution(ScreenResolution.RES_640X480)
```

通道数，即彩色GRB24，或灰度图GRAY

```
game.set_screen_format(ScreenFormat.RGB24) # GRAY8
```
Button类
特别注意：这一步不能省略，否则智能体将没有可以动的 action

```
game.add_available_button(Button.MOVE_LEFT)  # 左移
game.add_available_button(Button.MOVE_RIGHT)  # 右移
game.add_available_button(Button.ATTACK)  # 攻击
```

GameVariable类

此外，还可以通过game_variable获得玩家的health, ammo, weapon availability以及location.

```
game.add_available_game_variable(GameVariable.AMMO2)
```

获取X坐标

```
game.add_available_game_variable(GameVariable.POSITION_X)
```

获取Y坐标

```
game.add_available_game_variable(GameVariable.POSITION_Y)
```

初始化

在完成上述环境配置后，初始化环境。初始化仅需一次即可，之后每次重新开始新的一个回合，通过 game.new_episode() 即可。
```
game.init()
`````
state类
当需要获取环境的状态是，可通过如下操作：

# Gets the state

```
state = game.get_state()
1
2
包含了

n           = state.number
misc        = state.game_variables  # 音乐
screen_buf  = state.screen_buffer   # 主视角图
depth_buf   = state.depth_buffer    # 主视角深度图
labels_buf  = state.labels_buffer   # 主视角的标签图(目标分割图)
automap_buf = state.automap_buffer  # 俯视地图
labels      = state.labels
```

终止环境
```
game.close()
```
自定义环境配置文件
上面介绍了在程序中通过 http://game.xxx 来配置环境，从而定义在环境中显示或获取什么信息。其实也可以通过修改和加载cfg配置文件，实现一样的功能。

这里同样需要注意路径问题。

```
game.load_config("vizdoom/scenarios/basic.cfg")
```

配置文件的修改必须满足以下规则：

- 一行一项：one entry per line (except for list parameters),
- 大小写无关：case insensitive
- 开头为注释：lines starting with # are ignored,
- 忽视下划线：underscores in keys are ignored (episode_timeout is the same as episodetimeout),
- 字符串无需引号：string values should not be surrounded with apostrophes or quotation marks.

示例：


```
#doom_game_path = ../../scenarios/doom2.wad
doom_scenario_path = ../../scenarios/basic.wad
doom_map = map01

# Rewards
living_reward = -1

# Rendering options, RES_160X120
screen_resolution = RES_320X240
screen_format = CRCGCB
render_hud = True
render_crosshair = false
render_weapon = true
render_decals = false
render_particles = false
window_visible = true

# make episodes start after 20 tics (after unholstering the gun)
episode_start_time = 14

# make episodes finish after 300 actions (tics)
episode_timeout = 300

# Available buttons
available_buttons =
    {
        MOVE_LEFT
        MOVE_RIGHT
    }
available_buttons += { ATTACK }
```

# Game variables that will be in the state

available_game_variables = { AMMO2}

mode = PLAYER
doom_skill = 5

#auto_new_episode = false
#new_episode_on_timeout = false
#new_episode_on_player_death = false
#new_episode_on_map_end = false

[1]: https://blog.csdn.net/wzduang/article/details/109426913

# Gym

* Gym is a toolkit for developing and comparing reinforcement learning algorithms.
  * 簡單來說 Gym 提供了一些 RL 的環境，也可以讓使用者自訂環境
  * [https://github.com/openai/gym](https://github.com/openai/gym)

## 環境 Environments
* 環境中最重要的就是 `step()`
  * 傳回 `observation`, `reward`, `done`, `info` 四個值
    * `observation` (object): 當前環境下的狀態
    * `reward` (float): 剛剛的 action 所產生的 reward，目標是要讓最後的 total reward 增加
    * `done` (boolean): 如果是 True 表示 episode 是結束了，結束的話就要重設環境
    * `info` (dict): 用來 debug 的
* Gym 提供的環境見 [https://gym.openai.com/envs/#classic_control](https://gym.openai.com/envs/#classic_control)
* 所有的 Gym 環境都繼承自 `Env`
  * 最重要的函數有 `step()`, `reset()`, `render()`, `close()`, `seed()`
  * 在環境中要設定 action_space`, `observation_space`, `reward_range`
    * `reward_range` 預設是 [-inf,+inf]
 
        
        


## Agent

## Observations
* 每個 episode 開始的時候 observation 要先 `env.reset()`
* 環境的 `step()` 就是定義每一個步驟要做些什麼事情
  * 每一個步驟，agent 會選擇一個 action，然後環境根據這個 action 傳回 observation 和 reward
  
## Spaces
* 每個環境都有 `action_space` 和 `observation_space` 用來表明可用的 action 和 observation
  * `print(env.action_space)`
  * `print(env.observation_space)`
* 最常使用的是 `Box` 和 `Discrete` spaces
  * 例如:
``
from gym import spaces
space = spaces.Discrete(8) # Set with 8 elements {0, 1, 2, ..., 7}
``

* Example
```
import gym
env = gym.make("環境的名字")
observation = env.reset() # 開始 RL 之前要把 observation 恢復到初始值
for _ in range(要跑幾步):
  env.render()
  action = env.action_space.sample() # your agent here (this takes random actions)
  observation, reward, done, info = env.step(action)

  if done:
    observation = env.reset()
env.close()
```

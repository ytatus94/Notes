* Reinforcement learning 就是 agent 在某個環境的某個狀態下，要做什麼動作才能獲得最大的報酬。
    * agent
    * Environment
    * Policy: 在某個狀態下，該採取什麼樣的動作 actions。注意動作是**複數**，當前的某個狀態可能執行的動作有很多個。Policy 可以決定 agent 的行為
    * Reward: 每走一步 (執行一個動作) 環境就會傳給 agent 一個數值，這個數值就是做這個動作所獲得的報酬。agent 的目標就是要在每一步選擇最好的動作，讓整個過程的總報酬最大
    * Value functioin: agent 在現在這個時間點現在這個狀態之下，未來的每一個步驟**預計**所能獲得的總報酬。
        * 報酬是每執行一個步驟之後就立刻會獲得的結果，但是 value function 是未來的總報酬，因為還沒執行未來的動作，所以未來的報酬其實也不知道，因此 value function 其實是個未來總報酬的期望值
* Temporal difference: $V(S_{t+1}) - V(S_{t})$, 就是某個狀態 s 在某一時刻 t 和下一時刻 t+1 之間的 value function 的差值 
### Q-learning
* Q(s, a) 叫做 action-value function, 就是在某個狀態 s 時執行某個動作 a 所得到的 Q 值
    * 可把 Q 值想像成某種報酬，但 Q 值不是 reward
* Q-learning 其實就是把每個狀態的所有可能動作的 Q 值通通找出來後，形成一個很大的表格，之後只要照這個表格去找在該狀態 s 時要執行哪個動作才能獲得最大的 Q 值
    * 現在的狀態是 s 執行了動作 a 之後會得到報酬 R 並且狀態更新成 s'，在 s' 的狀態要執行哪個動作，就是去查 Q 值的表格，選 Q(s', a') 最大的那個動作去執行
    
    
DQN (Deep Q-Network):

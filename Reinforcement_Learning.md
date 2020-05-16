* Reinforcement learning 就是 agent 在某個環境的某個狀態下，要做什麼動作才能獲得最大的報酬。
* Environment
* Policy: 在某個狀態下，該採取什麼樣的動作 actions。注意動作是**複數**，當前的某個狀態可能執行的動作有很多個。Policy 可以決定 agent 的行為
* Reward: 每走一步 (執行一個動作) 環境就會傳給 agent 一個數值，這個數值就是做這個動作所獲得的報酬。agent 的目標就是要在每一步選擇最好的動作，讓整個過程的總報酬最大
* Value functioin: agent 在現在這個時間點現在這個狀態之下，未來的每一個步驟**預計**所能獲得的總報酬。
    * 報酬是每執行一個步驟之後就立刻會獲得的結果，但是 value function 是未來的總報酬，因為還沒執行未來的動作，所以未來的報酬其實也不知道，因此 value function 其實是個未來總報酬的期望值
* Temporal difference:

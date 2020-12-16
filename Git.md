# Git commands
```bash
# 在命令列顯示樹狀圖 
git log --oneline --graph --decorate --all

# 只看一個檔案的修改歷史紀錄
git log -p filename

# 顯示本地和遠端的 branches
git branch -a 

# 只顯示遠端的 branches
git branch -r 

# 把本地的 branch 改名字
git branch -m <old_name> <new_name>

# 把經過修改但是還沒有 commit 的部分存起來，staged 和 unstaged 的都會存起來，但是 untracked 的不會存起來
git stash

# 把 untracked 的也存起來
git stash --include-untracked

# 把 stagsh 的東西拿出來
git stash pop

# 可以 checkout 遠端的某個 branch
git checkout --track origin/newsletter
git checkout -t <name of remote>/test
git checkout -b test <name of remote>/test

# 可以 checkout 某個 commit-number 的檔案
git checkout commit-number file

# 可以從別的 branch 中 checkout 某個檔案
git checkout <branch_name> -- <paths>

# 把 staged 的部分 reset 成原本的 (就是 git add 的相反) 已經更改過的檔案仍維持改過的樣子，但是尚未放入 staged.
git reset

# 把 B 合併到 A (以 A 為主，要 在 A branch 執行)
git merge B

# 可以比較兩個 commits 的所有檔案的差別
git diff commit-numbar1 commit-number2

# 只要列出兩個 commits 中不同的檔案的名字，有兩種方法
git diff --name-only SHA1 SHA2
git diff --name-only HEAD~10 HEAD~5

# 兩個點是比較兩個 branch 的 HEAD
git diff branch1..branch2

# 三個點是比較 branch2 的 HEAD 和與 branch1 的共同祖先
git diff branch1...branch2

# 範例，可以指定只比較哪個檔案
git diff master..feature -- <file>
git diff master...feature -- <file>

# 把已經刪除的遠端 branch 從 git branch -r 中移除
git fetch -p

# 刪除遠端的 branch
git push <remote> --delete <old_name>
git push origin --delete origin/debug 
git push <remote> :<old_name>

# 把本地端的新的 branch 推到 github repository 上
git push <remote> <new_name>

# Reset the upstream branch for the new_name local branch
git push <remote> -u <new_name>

# 修改 commit 的作者資訊
git commit --amend --author "New Author Name <email@address.com>"

# Rebase, if you are on the branch you want to rebase
git rebase <new_base>

# Rebase, if you are not on the branch you want to rebase
git rebase <new_base> <other_branch>

# Solve conflict when rebase
git add <file-name>
git rebase --continue

# Rebase, move the base of branch from old_base to new_base
git rebase --onto <new_base> <old_base> <branch>
```

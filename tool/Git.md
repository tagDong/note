#Git

### 将远程git仓库里的指定分支拉取到本地（本地不存在的分支）

1. `git fetch origin dev(远端分支名)：dev(本地新分支名)`
2. 本地已经创建了dev的分支，需要手动切换


### 将本地Git分支develop推送到远程的master分支

`git push origin develop:master`
`git push <remote> <local branch name>:<remote branch to push into>`


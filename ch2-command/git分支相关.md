# git 分支相关

## 本地分支
### git rebase

`git rebase master topic`

`git rebase --onto test release^ dev` 把本来基于 release^1 的 dev 分支变基到 test 分支上，用于将一条分支上的开发线整个移动到完全不同的分支  
A -> B -> C -> D -> E  test  
A -> B -> W -> X -> Y -> Z  release (release^1 = Y)    
A -> B -> W -> X -> Y -> P -> Q  dev

变基之后 dev 分支变成
A -> B -> C -> D -> E -> P -> Q

`git rebase --preserve-merges master dev` 如果需要被变基的分支出现了环型结构，变基过程中可能会丢失掉某些合并点，是因为 git 在变基过程中会将该环形结构解析成一个线性序列，如果你不想 git 这么做，而是要保留合并点的话，可以使用 `--preserve-merges` 参数



-------------

## 远程分支
### 推送本地分支到远程仓库
基本语法：  
`git push origin local_branch:remote_branch`

示例：   
`$ git checkout -b examples-spring` # 创建一个新的分支
`$ git push origin examples-spring:examples-spring` # 推送本地 examples-spring 到 origin/examples-spring  


将本地分支和远程分支进行关联  
`$ git branch --set-upstream-to examples-spring origin/examples-spring`  
> 也可以直接使用 `git push -u origin examples-spring` 命令来替代上面两个命令，-u 参数相当于 --set-upstream-to  
> 也可以先 checkout 到本地分支，然后使用 `$ git branch --set-upstream-to=origin/examples-spring` 直接搞定


### 将远程分支拉取到本地分支
远程有个 origin/examples-git 分支，现需要将该远程分支拉倒本地 examples-git 分支，使用命令：  
`$ git checkout --track origin/examples-git`  # --track 参数表示将本地分支和远程分支直接建立了关联


### 删除远程分支
`git push origin :remote_branch`

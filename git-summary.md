# Git 常用命令速查表

### 前提说明

#### 写在最前面的话
**使用命令行一定要善用 tab 键的命令补全功能！**

#### 关于命令行中参数前面的 - 和 -- 的区别
- `-` 通常后面接的是参数的缩写
- `--` 通常后面接的是参数的全拼  

> e.g. `git status --short` 这个命令等价于 `git status -s`


#### 关于指定文件路径的命令
某些命令需要查看指定文件或者指定路径可以使用 `--` 后接文件名的形式



### 创建版本库
`git init`  # 本地初始化 git 仓库  
`git clone <url>`  # 克隆远程版本库  

`git config --list`  # 查看本地所有配置项  
`git config core.autocrlf`  # 查看某个配置的详细配置值  
`git config --global core.autocrlf false`  # 配置参数的值，`--global` 是对本地所有 git 项目生效，如果只想对本项目生效，则不需要该参数

### 提交操作
`git add .`  # 将所有的变更添加到索引，等同于 `git add -A` 或者 `git add --all`，如果只想添加某个指定的文件可以使用 `git add -- filename`  

`git commit -m "message"`  # 将索引中的变更提交到本地仓库  
`git commit -a -m "message"`  # 将 `git add .` 与 `git commit -m "message"` 这两个命令合成一个命令提交  

`git status`  # 查看当前工作目录下的变更内容  
`git status -s`  # 查看变更文件和变更状态，那些标识为<span style="color:red">UU</span>的文件就是需要解决的冲突文件

`git pull`  # 拉取远程分支的更新，并且将远程分支的更新合并到本地分支  
`git pull --rebase`  # 拉取远程分支的更新，并使用 rebase 的方式将远程分支的更新应用到本地分支，相对于 `git pull` 操作，这种方式不会产生一个远程分支合并点，该操作等下于下面两个命令的合并*（推荐采用这种方式来拉取远程分支的更新，这样整个分支的提交历史记录会赶紧很多）*  
`git fetch`  # 拉取远程仓库的更新，但不进行合并  
`git rebase origin/master master`  # 将远程 master 分支上的更新 rebase 到本地 master 分支上
`git push origin master`  # 将本地提交记录推送到远程仓库
`git push origin :test`  # 删除远程仓库的 test 分支，冒号前面的是本地分支名，冒号后面的是远程分支名，这里冒号前面的为空，表示将本地的一个空分支推送到远程的 test 分支，意思就是删掉远程的 test 分支


### 日志&差异
`git log -5`  # 显示最近5次提交历史，-n 表示最近的n次提交历史   
`git log --graph`  # 以图形结构展示提交历史点  
`git log --oneline -5`  # 显示最近5次提交，每个提交显示在一行  
`git log -p -1`  # -p 查看提交的详细内容，-p 会列出每个文件改动的地方  
`git log --oneline --name-status -1`  # --name-status 显示提交文件名以及文件的修改状态，如果只关注提交涉及的文件名也可以使用 --name-only  
`git log --oneline --no-merges -5`  # --no-merges 忽略掉那些 merge 的提交点  
`git log --oneline --author=walle -5`  # --author=walle 只关注作者是 walle 的提交记录  
`git log --oneline --after='2017-09-05'`  # --after='2017-09-05'显示从2017-09-05号之后的所有提交，类似的查看某个日期之前的提交可以使用 --before=''  
`git log --oneline --grep='oracle'`  # --grep='oracle' 查看 commit message 中包含 oracle 字符的所有提交（可以帮你快速找到某个历史的提交点）  
`git log -S "readme"`  # -S 基于提交内容的搜索，搜索哪次提交内容中包含了 readme 字符（可以用来快速查找文件中的某一行或者某段内容是在哪个提交中添加进来的）  
`git rev-list --oneline master -- README.md`  # 查看 README.md 文件在 master 分支上所有的提交历史


`git diff`  # 比较工作目录和索引之间的差异  
`git diff --cached`  # 比较索引和HEAD之间的差异  
`git diff --cached SHA1`  # 比较索引和某个提交点之间的差异
`git diff SHA1` # 比较工作目录和给定 commit 直接的差异，e.g. `git diff HEAD` 比较工作目录和最后一次 commit 之间的差异  
`git diff --name-only SHA1..SHA2`  # 列出两个提交点之间的所有差异文件名  
`git diff SHA1..SHA2 > test.patch`  # 将两个提交点之间的所有差异打成补丁文件 test.patch（这样可以很方便地将一个分支上面的所有修改内容应用到另外一个分支，应用补丁的命令为 `git apply test.patch`）
`git diff --name-only branch1 branch2`  # 查看两个分支之间的差异文件



`git reflog`





### 分支相关
`git branch -avv`  # 显示所有的本地分支和远程分支  
`git checkout test`  # 切换到 test 分支，test 分支必须是本地存在的分支，否则会报错  
`git checkout -b test`  # 基于当前所在的分支，检出一个新的命名为 test 的分支  
`git branch -D test`  # 删除本地的 test 分支，-D 为强制删除，如果当前分支的变更没有完全合并到 master 分支时使用 -d 无法删除，必须使用 -D 来强制删除  
`git branch -r -d origin/test`  # 删除本地远程追踪分支，并不会删除远程仓库里面的分支，推荐使用下面的命令删除本地远程追踪分支  
`git fetch -p`  # 拉取远程更新，并且将本地无用的远程追踪分支删除，例如，远程仓库中的某个分支被其他人员删除，而你使用 `git fetch` 时你本地对应的远程追踪分支(origin/test)并不会删除  
`git push origin :test`  # 删除远程仓库 test 分支，并且删除本地的 origin/test 分支，但是不会删除本地的 test 分支  
`git branch -m dev develop`  # 重命名本地分支 dev 为 develop
`git checkout --track origin/test`  # 基于远程的 test 分支，检出为本地的 test 分支，--track 参数表示将本地新检出的 test 分支关联到远程的 origin/test 分支上，一般在第一次拉取远程新的分支是使用  
`git branch --set-upstream-to=origin/test`  # 将当前分支关联到远程的 origin/test 分支，这样之后对当前分支的 pull/push 操作都不需要指定远程分支名  
`git show-branch`  # 查看本地所有分支之间的差异点
`git show-branch test origin/test`  # 查看本地 test 分支与远程追踪分支 origin/test 分支之间的提交差异  




参考
http://www.jb51.net/article/55442.htm

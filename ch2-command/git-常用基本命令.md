# git 常用基本命令

### 写在最前面的话
**使用命令行一定要善用 tab 键的命令补全功能！**

#### 关于命令行中参数前面的 - 和 -- 的区别
- `-` 通常后面接的是参数的缩写
- `--` 通常后面接的是参数的全拼  

> e.g. `git status --short` 这个命令等价于 `git status -s`

#### 关于 `^` 和 `~` 的区别
- `^` 指向前一个父提交
- `~` 指向前一个提交

```
e.g.
A <- B <- C <- D(HEAD)
         /
  E <- F

C: HEAD~ HEAD 的前一个提交
B: HEAD~~ / HEAD~2 / HEAD~1^1
F: HEAD~^2  HEAD 的前一个提交的第二个父提交
E: HEAD~1^2~1
```

#### 关于指定文件路径的命令
某些命令需要查看指定文件或者指定路径可以使用 `-- filename` 的形式，避免文件名和命令名称重名情况下，git 可以明确地知道是个文件名（两个短线一个空格加文件名/路径，和参数不同的是参数中间没有空格）
> e.g. `git log --oneline -- qfang-examples-parents/pom.xml` -- 用于明确指定文件名


### 创建版本库
`git init`  # 本地初始化 git 仓库  
`git clone <url>`  # 克隆远程版本库  

`git config --list`  # 查看本地所有配置项  
`git config core.autocrlf`  # 查看某个配置的详细配置值  
`git config --global core.autocrlf false`  # 配置参数的值，`--global` 是对本地所有 git 项目生效，如果只想对本项目生效，则不需要该参数


### 提交&更新
`git add .`  # 将所有的变更添加到索引，等同于 `git add -A` 或者 `git add --all`，如果只想添加某个指定的文件可以使用 `git add -- filename`  
`git rm --cached -- filename`  # 从索引中删除某个文件，但不会从工作目录删除

`git commit -m "message"`  # 将索引中的变更提交到本地仓库  
`git commit -a -m "message"`  # 将 `git add .` 与 `git commit -m "message"` 这两个命令合成一个命令提交  
`git commit --amend`  # 修改最后一次提交的 message（只能修改最后一次的提交）  
`git rebase -i`  # 与交互的方式合并提交/更改提交的顺序等  
`git merge test`  # 将 test 分支合并到当前分支  

`git status`  # 查看当前工作目录下的变更内容  
`git status -s`  # 查看变更文件和变更状态，那些标识为<span style="color:red">UU</span>的文件就是需要解决的冲突文件  
`git status -vv`  # 查看变更的详细内容

`git pull`  # 拉取远程分支的更新，并且将远程分支的更新合并到本地分支  
`git pull --rebase`  # 拉取远程分支的更新，并使用 rebase 的方式将远程分支的更新应用到本地分支，相对于 `git pull` 操作，这种方式不会产生一个远程分支合并点，该操作等下于下面两个命令的合并*（推荐采用这种方式来拉取远程分支的更新，这样整个分支的提交历史记录会赶紧很多）*  
`git fetch`  # 拉取远程仓库的更新，但不进行合并  
`git rebase origin/master master`  # 将远程 master 分支上的更新 rebase 到本地 master 分支上
`git push origin master`  # 将本地提交记录推送到远程仓库
`git push origin :test`  # 删除远程仓库的 test 分支，冒号前面的是本地分支名，冒号后面的是远程分支名，这里冒号前面的为空，表示将本地的一个空分支推送到远程的 test 分支，意思就是删掉远程的 test 分支

`git cherry-pick SHA1`  # 将某个提交应用到其他分支，会产生一个新的提交


#### 重置
`git reset`  # 重置更改，将 HEAD 指向新的提交，对应的有三个选项 --soft：工作目录和索引都不会更好，只是将 HEAD 指向新的提交，之前的所有提交都还会在索引中, --mixed：只重置索引，不重置工作目录，你所有的更改还会在工作目录中（默认的选项）, --hard：重置工作目录和索引，之前所有的变更都会被丢弃掉  
`git checkout -- filename`  # 将某个指定的文件还原到 HEAD 中的内容， reset 是用来还原整个工作目录的，如果需要还原某个文件可以使用 git checkout  
`git revert SHA1`  # 用来还原某个提交的更改，与 reset 不同的是 revert 会创建一个新的提交，只不过这个新的提交是之前指定提交（SHA1）的反提交
`git clean -df` # 清除所有的更改，并将工作目录，索引，包括删除 git 未追踪的文件（慎用！慎用！）  


#### 暂存
`git stash`  # 将工作目录和索引中的更改暂存起来，并且将工作目录和索引恢复到当前分支的 HEAD，效果类似于 `git reset --hard`，但是并不会像 reset 那样丢弃当前的更改（在开发一半的过程中需求切换到其他分支修改问题时，该命令非常有用）  
`git stash list`  # 查看当前所有的 stash 暂存点
`git stash save <desc>`  # 创建一个 stash 暂存点，并且指定名称，还原时可以根据该名称来还原  
`git stash pop [<stash>]`  # 弹出指定暂存点，并且该暂存点的更改应用在当前分支上，如果不指定 <stash> 默认是弹出最近的一个暂存点，完成之后会自动删除该暂存点  
`git stash apply <stash>`  # 应用某个暂存点，与 pop 效果类似，但是 apply 并不会删除该暂存点  
`git stash show stash{0}`  # 显示修改的统计  
`git stash show –p stash{0}`  # 显示暂存点更改的内容  
`git stash drop <stash>`  # 删除一个 stash
`git stash clear`  # 清除所有的 stash 暂存点


### 日志&搜索
`git log -5`  # 显示最近5次提交历史，-n 表示最近的n次提交历史   
`git log --graph`  # 以图形结构展示提交历史点  
`git log --oneline -5`  # 显示最近5次提交，每个提交显示在一行  
`git log -p -1`  # -p 查看提交的详细内容，-p 会列出每个文件改动的地方  
`git log --oneline --name-status -1`  # --name-status 显示提交文件名以及文件的修改状态，如果只关注提交涉及的文件名也可以使用 --name-only  
`git log --oneline --no-merges -5`  # --no-merges 忽略掉那些 merge 的提交点  
`git log --oneline --author=walle -5`  # --author=walle 只关注作者是 walle 的提交记录  
`git log --oneline --after='2017-09-05'`  # --after='2017-09-05'显示从2017-09-05号之后的所有提交，类似的查看某个日期之前的提交可以使用 --before=''  
`git log --oneline --grep='oracle'`  # --grep='oracle' 查看 commit message 中包含 oracle 字符串的所有提交（更加提交 message 快速找到某个历史的提交点）  
`git log -S "readme"`  # -S 基于提交内容的搜索，搜索哪次提交内容中包含了 readme 字符（可以用来快速查找文件中的某一行或者某段内容是在哪个提交中添加进来的）  
`git log --format="%h %an %s %ad" --date=short -5 --no-merges`  # --format参数 %h：显示 SHA 值，%an：显示提交作者，%s：显示提交的 message，%ad：显示提交的日期；--date=shore 用来格式化日期的显示形式（2017-12-09）这种格式  
`git rev-list --oneline master -- README.md`  # 查看 README.md 文件在 master 分支上所有的提交历史  
`git bisect`  # 一个基于二分查找的方式来定位修改提交点（自行研究使用方法...）  
`git blame -- README.md`  # 查看指定文件每行的作者，可以指定需要查看哪些行的作者（相当于 idea 中的注释查看功能）  

`git ls-files *Tests.java`  # 搜索某个文件在当前工作目录下的完整路径名（在某些情况下需要对某个文件做指定操作时，需要先获取这个文件的完整路径）  
`git ls-files *.java | wc -l`  # 统计某种类型文件数量

`git grep "message" [<branchname>]`  # 基于内容搜索，查看指定的内容在哪个文件中存在，后面参数还可以指定分组名

`git show SHA1`  # git show 命令几乎可以用来查看 git 中所有的对象
`git show master:README.md > README.md.master`  # 快速获取 master 分支上 README.md 文件的内容（这个命令非常有用，当你需要查看不同分支/不同提交点/不同tag之间某个文件的内容时，使用该命令可以避免不停的切换分支/tag来获取文件的内容）


### 差异
`git diff`  # 比较工作目录和索引之间的差异  
`git diff --cached`  # 比较索引和HEAD之间的差异  
`git diff --cached SHA1`  # 比较索引和某个提交点之间的差异
`git diff SHA1` # 比较工作目录和给定 commit 直接的差异，e.g. `git diff HEAD` 比较工作目录和最后一次 commit 之间的差异  
`git diff --name-only SHA1..SHA2`  # 列出两个提交点之间的所有差异文件名  
`git diff SHA1..SHA2 > test.patch`  # 将两个提交点之间的所有差异打成补丁文件 test.patch（这样可以很方便地将一个分支上面的所有修改内容应用到另外一个分支，应用补丁的命令为 `git apply test.patch`）
`git diff --name-only branch1 branch2`  # 查看两个分支之间的差异文件


### 分支&标签
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

`git tag`  # 查看本地所有的标签  
`git tag <tagname>`  # 基于当前分支 HEAD 创建标签  
`git tag -d <tagname>`  # 删除标签  


#### 其他
`git reflog`  # *让你永远有后悔药吃* 列出所有 HEAD 变更还原点，当前分支 HEAD 发生变化时（新的提交/撤销提交/切换分支等），git 都会自动记录一个还原点，你可以使用这个还原点来恢复你的错误操作，例如：当你使用 `git reset HADE^` 还原了某个提交点后，你又发现那个被重置的提交点是有用的，你又想恢复回来，这时你就可以使用 reflog 还原点来还原

`git filter-branch --tree-filter "find * -type f -name '*.log' -delete" master test test2`  # *核弹级命令*，在 master&test&test2 分支上删除所有 log 文件，通常情况下如果我们修改了大量文件提交时会使用 `git add .` 命令提交所有的更改，如果不小心包含了一些不必要的 log 文件，这些文件将会提交到版本库中，时间长了之后这些文件可能会散落到不同的分支上，如果你想全局地删掉这些文件 `git filter-branch` 命令就非常有用了，它可以帮你在指定的所有分支上所有的提交中删除指定的文件，而且这些删掉的文件你也无法使用 `git reset` 命令来恢复，并且查看提交历史中也看不到这些文件的记录（慎用！慎用！）

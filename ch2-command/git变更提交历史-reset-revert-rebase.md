# git 变更提交历史 reset / revert / rebase


### git reset
`git reset` 命令主要有三个选项: --soft，--mixed，--hard

- `git reset --soft SHA1` 命令  
**只改变 HEAD 引用**，索引和工作目录保持不变。原 HEAD 保持在 ORIG_HEAD 中，SHA1 的默认值为 HEAD  
一个典型的应用 `git reset --soft HEAD~1`，将当前 HEAD 指向上一个提交历史，因为 --soft 不会改变工作目录和索引的内容，所以该命令可以用来修改提交时的 message 信息

- `git reset --mixed SHA1` 命令  
**改变 HEAD 引用和索引内容**，工作目录内容保持不变。--mixed 是默认的选项，即如果你执行 `git reset` 其实等同于 `git reset --mixed HEAD`，其效果是将当前索引中的所有内容清空

- `git reset --hard SHA1` 命令  
**改变 HEAD 引用、索引即工作目录的内容**。默认情况下如果你执行 `git reset --hard` 其效果就是将当前分支重置到 HEAD 的状态，删除 HEAD 之后的所有变更  
*注：如果是新增文件并且没有被 git 追踪的文件，不会被删除掉，因为这个新增文件在 git 索引和版本库中都没有这个文件，`git reset --hard` 不会自动删除这种类型的文件，但是如果新增加的文件已经被索引，则会被删除*

git reset 选项影响  

|选项|HEAD|索引|工作目录|
|-|:-:|:-:|:-:|
|--soft|是|否|否|
|--mixed|是|是|否|
|--hard|是|是|是|

**git reset 示例**
``` shell
$ git status
On branch client
nothing to commit, working tree clean

$ git log --oneline -2
fedccfc (HEAD -> client) c9 commit
7361d87 c8 commit
```
可以看到当前工作目录和索引都是干净的，当前分支最近的两个提交为 c9，c8

``` shell
$ git reset --soft HEAD~1

$ git status
On branch client
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   readme.txt


$ git commit -m "c9 commit other"
[client f5cab36] c9 commit other
 1 file changed, 1 insertion(+)

$ git log --oneline -2
f5cab36 (HEAD -> client) c9 commit other
7361d87 c8 commit
```
`git reset --soft HEAD~1` 重置 HEAD 修改文件还在暂存区，直接使用 `git commit` 命令提交修改，可以修改提交的信息

### git clean


### git revert


### git revert -i


### git commit -m



### cherry-pick

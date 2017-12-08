# git 协作开发规范

### git 分支介绍

- `dev-xxx` 分支是开发分支，命名规范：以`dev-`为前缀，后面接具体的需求描述简称
- `test` 分支是用来测试使用的，一般是研发完成某个需求之后需要提测时，将 `dev-xxx` 分支上的代码合并到 `test` 分支上
- `release` 分支是测试通过之后，准备打预发布时将 `test` 分支的代码合并进 `release` 分支
- `master` 分支是上线成功之后，再将 `release` 分支上的内容合并到 `master` 分支

``` shell
$ git branch -avv
  dev-default            8a2d097 优化人事消息监听器启动异常提示消息
* dev-taCommission       8a2d097 优化人事消息监听器启动异常提示消息
  master                 ec8c39c [origin/master] 修改 ignore 配置
  release                d4378a3 [origin/release] 师徒关系新增了enable字段
  test                   8a2d097 [origin/test] 优化人事消息监听器启动异常提示消息
  remotes/origin/HEAD    -> origin/master
  remotes/origin/master  ec8c39c 修改 ignore 配置
  remotes/origin/release d4378a3 师徒关系新增了enable字段
  remotes/origin/test    8a2d097 优化人事消息监听器启动异常提示消息
  remotes/origin/xfang   e5963ff 第二次git提交
```



### 测试失败需要还原提交
试想下在测试过程中假设 test 分支上已经存在如下提交图（commit1 由用户 A 提交， commit2 由用户 B 提交，...），用户 A 为了修复某个小 bug 做了两个提交 C1 和 C4，此时已经需要打预发不了，而这个 bug 的测试一直无法正常通过，项目组临时决定取消该 bug 的修复提交，不再本次上线，用户 A 则需要撤销 C1 和 C4 两个提交  

C1(A) -> C2(B) -> C3(C) -> C4(A) -> C5(B) ...





### 紧急修改的小 bug 需要快速推到 release 分支上

# git 远程仓库


### git 远程仓库

`git remote -v`  # 查看远程仓库配置
> $ git remote -v  
> origin  https://github.com/walle-liao/git-book.git (fetch)  
> origin  https://github.com/walle-liao/git-book.git (push)

`$ git remote remove origin`  # 移出远程仓库


`$ git remote add origin git@github.com:walle-liao/git-book.git`  # 重新关联一个新的远程仓库  

将之前的 https 的仓库改成使用 ssh 协议的仓库地址，使用 https 仓库地址时，每次提交代码都需要输入用户名和密码，非常麻烦，改成 ssh 协议后，就不需要每次输入用户名和密码了（前提是需要将本地 ssh key 公钥上传到 github 配置中）。

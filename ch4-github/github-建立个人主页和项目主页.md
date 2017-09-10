# github 建立主页


### 创建个人主页
GitHub 为每一个用户分配了一个二级域名 <user-id>.github.io，用户为自己的二级域名创建主页很容易，只要在托管空间下创建一个名为 <user-id>.github.io 的版本库，向其 master 分支提交网站静态页面即可，其中网站首页为 index.html。  

示例：
- 登录 github 创建 `walle-liao.github.io` 的项目
- clone 到本地，并且往 master 分支提交 index.html 文件
- push 到 github 上，即可以访问 [个人主页](http://walle-liao.github.io)

### 创建项目主页
GitHub 会为每个账号分配一个二级域名 <user-id>.github.io 作为用户的首页地址。实际上还可以为每个项目设置主页，项目主页也通过此二级域名进行访问。  
例如 walle-liao 用户创建的 githubdemo 项目如果启用了项目主页，则可通过网址 http://walle-liao.github.io/githubdemo/ 访问。  
为项目启用项目主页很简单，**只需要在项目版本库中创建一个名为 `gh-pages` 的分支**，并向其中添加静态网页即可。也就是说如果项目的 Git 版本库中包含了名为 `gh-pages` 分支的话，则表明该项目提供静态网页构成的主页，可以通过网址 http://<user-id>.github.io/<project-name> 访问到。

示例：
- 登录 github 创建 `githubdemo` 项目
- clone 到本地，并且在本地创建 `gh-pages` 分支，并且在该分支提交 index.html 文件
``` bash
$ git symbolic-ref HEAD ref/heads/gh-pages
$ rm .git/index
vim index.html
$ git commit -m "branch gh-pages init." -a
$ git push -u origin gh-pages:gh-pages
```
- push 到 github 上，即可访问项目主页 https://walle-liao.github.io/githubdemo/

# git 常见问题总结-配置相关

### git 配置文件说明

- /etc/gitconfig 文件：包含了适用于系统所有用户和所有库的值，可以使用命令 `git config --system ` 进行配置。
- ~/.gitconfig 文件 ：包含指定用户的配置，可以使用命令 `git config --global` 进行配置。
- 位于项目 .git 目录的 config 文件 (即 .git/config 文件) ：当前项目指定的配置，可以使用命令 `git config` 进行配置，该配置参数具有最高的优先级。  

当同一个参数在三个配置文件中都有设值时，项目文件下的配置参数值具有最高的优先级，其次是用户配置参数，最后是系统配置参数值。  

**配置相关命令**  
`git config --list`  查看所有的配置信息  
`git config --global --edit`  编辑 global 配置文件，当然在 window 环境下可以直接使用文本工具进行编辑（其实编辑的就是 ~/.gitconfig 文件）  

--------------------
### git bash 对于中文路径显示为数字问题解决办法  
默认情况下 git bash 对于中文的路径会进行编码，这样导致在使用`git status, git diff`等命令时无法直接看到改动的文件名，如下：  
![git bash 中文路径显示乱码](./images/1001.png)  
**解决办法：**  
`$ git config --global core.quotepath false`  
之后再使用`git status`查看可以正常显示中文文件名了
![git bash 正常显示中文路径](./images/1002.png)  

------------------
### GitLab 拉取代码是提示找不到仓库
公司使用 gitlab 之前使用一直正常，某次修改了配置之后再使用 `git fetch` 代码是提示错误：`fatal: Could not read from remote repository.`

**解决办法：**
找到相应项目的 .git/config 文件，修改里面对应的 url 配置：  
`url = http://liaozhicheng:liaozhicheng@git.qfang.com/erp/trade.git`  
这个 url http:// 后面的分别问用户名:密码，这样在使用 `git fetch` 就不会有问题了

-----------------
### 使用 github 时提示需要输入用户名和密码
如题，github 配置中有导入本地电脑的 ssh key 公钥，但是每次提交代码时还是需要输入用户名和密码

解决办法：
找到相应项目 .git/config 文件，修改对应 url 的配置，让其采用 ssh 协议，而不要采用 https 协议，https 协议需要输入用户名和密码校验

修改之前：  
`url = https://github.com/qfang-erp/qfang-examples.git`

修改之后：  
`url = git@github.com:qfang-erp/qfang-examples.git`

PS：要使用 ssh 协议并且不用每次输入用户名和密码，需要先在本地生成对应公钥，并且将公钥添加到 github 配置中。

--------------------
### git diff 基于第三方比较工具配置
通常情况下我们要比较某个文件不同版本之间的差异时，我们可以使用 `git diff` 命令来进行比较，但是如果文件比较大，差异行数比较多时，基于命令窗口的 `git diff` 比较就显得有些无力了，幸好 git 支持我们自定义配置第三方专业的差异比较工具来进行比较。  
下面演示了通过配置 BCompare 工具来进行差异比较，我们使用直接编辑配置文件的办法来配置，在配置文件中加入如下配置，保存文件，重启 gitbash  
*（如果是 linux 环境可以使用 `git config --global --edit` 来打开 config vi 编辑界面）*  
``` bash
# difftool 配置
[diff]
	tool = bc4
[difftool "bc4"]
	cmd = "\"C:/Program Files/Beyond Compare 4/bcomp.exe\" \"$LOCAL\" \"$REMOTE\""
[difftool]
	prompt = false

# mergetool 配置
[merge]
	tool = bc4
[mergetool "bc4"]
	cmd = "\"C:/Program Files/Beyond Compare 4/bcomp.exe\" \"$LOCAL\" \"$REMOTE\" \"$BASE\" \"$MERGED\""
	trustExitCode = true
```
如果配置成功的话，使用命令 `git difftool` 可以弹出 BCompare 的比较窗口  

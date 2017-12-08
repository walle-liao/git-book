# Git 常用命令速查表

### 前提说明
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

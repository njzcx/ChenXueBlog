# 一、git官网教程

1. 版本控制发展历史，经历从本地版本控制(RCS) -> 集中式版本控制(SVN) -> 分布式版本控制(git)，逐步从单人维护到多人+多副本维护。

2. git作者是Linux开发组于2005年开发。
3. git的区别于其他版本控制器的特性有：直接记录快照而非差异；近乎所有操作都是本地执行；git保证完整性；git一般只添加数据；git有三种状态和阶段
4. git使用前的初始化命令:`git config`，设置控制 Git 外观和行为的配置变量，这些变量存储在三个不同的位置
   1. `/etc/gitconfig`
   2. `~/.gitconfig`
   3. 当前使用仓库的 Git 目录中的 `config` 文件（即 `.git/config`）
   4. 每一个级别会覆盖上一级别的配置，所以 `.git/config` 的配置变量会覆盖 `/etc/gitconfig` 中的配置变量。
   5. 你可以通过以下命令查看所有的配置以及它们所在的文件：`git config --list --show-origin`
   6. 可以使用 `git config --list`命令来列出所有 Git 当时能找到的配置
5. 通过git help <verb>或 git <verb> -h获取命令帮助

各类文件和用途

工作原理

# 二、git命令列表
## 2.1 pull
## 2.2 push
## 2.3 fetch
## 2.4 commit
## 2.5 rebase
## 2.6 stash
## 2.7 remote
# 三、常见困惑
# 四、经典使用场景
## 4.1初始化git
```shell
#创建SSH key；t密钥类型，C注释文字；f路径文件名，默认为~/.ssh/id_rsa
$ ssh-keygen -t rsa -C "your_email@example.com" -f njzcx_rsa
#输入密码：该密码是使用此密钥时在push文件的时候要输入的密码，可为空
Enter passphrase (empty for no passphrase): 
Enter same passphrase again:

# 默认SSH只会读取id_rsa，如果密钥改了名字，需要执行如下命令。
# -v输出debug
$ ssh -vT git@github.com -i ~/.ssh/njzcx_rsa
# 在创建~/.ssh/config文件，并写入内容如下
$ cat ~/.ssh/config
Host github.com gist.github.com api.github.com
IdentityFile ~/.ssh/njzcx_rsa
```
## 4.2git切换ssh和http协议
1. 查看当前remote: <br>
git remote -v

2. 切换到http: <br>
git remote set-url https://github.com/username/repository.git

3. 切换到ssh: <br>
git remote set-url git@github.com:username/repository.git
## 4.3 切换分支
## 4.4 解决冲突


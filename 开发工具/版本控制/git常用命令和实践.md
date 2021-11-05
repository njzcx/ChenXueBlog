# git官网教程

# git命令列表
## pull
## push
## fetch
## commit
## rebase
## stash
## remote
# 常见困惑
# 经典使用场景
## 初始化git
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
## git切换ssh和http协议
1. 查看当前remote: <br>
git remote -v

2. 切换到http: <br>
git remote set-url https://github.com/username/repository.git

3. 切换到ssh: <br>
git remote set-url git@github.com:username/repository.git
## 切换分支
## 解决冲突


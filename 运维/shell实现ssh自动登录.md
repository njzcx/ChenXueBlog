# 前言

公司开发使用docker，每次登陆自己开发机总要输入 `ssh user_name@ip_string`，然后再确认输入`password`，手快了还经常会输错。作为一个懒人，肯定要找一个取巧的方式，查看了下ssh命令，由于它要进行一次跟服务器的加密交互，所以没有直接附带密码登陆的选项，只好作罢。

前些天在同事进行技术分享时，看到他竟然只输入了一行命令`./test.sh`就成功登陆了开发机，甚是惊异，于是回来搜索研究了一下，遂成此文。

------

# shell脚本基础

在编写ssh自动登陆脚本之前，先说一下shell脚本的基础，此基础不是一些语法什么的，网上到处都是，这里总结了一下shell脚本的运行机制~

## shell脚本的运行方式

首先要说一下shell的几种启动方式，正是踩了脚本启动的坑，才使用原来十分钟就搞定的脚本，花了两个小时才搞定。同时也使得我们运行shell，知其所以然。

##### 通过文件名执行

shell脚本可以直接通过文件名执行，需要注意的是文件需要执行权限。通过 `sudo chmod +x ./file_name.sh` 来给文件添加执行权限；

##### 指定脚本解释器来执行文件

我们常用的 `sh file_name.sh` 就是指定了脚本解释器 `/bin/sh`来解释执行脚本；常见的脚本解释器还有：`/bin/bash`等，我们可以使用`ls -l /bin/*sh`命令来查看当前可用的脚本解释器；

##### 使用. ./file_name或source命令执行脚本

这种方式不会像前两种方式一样fork一个子进程去执行脚本，而是使用当前shell环境执行，用于 .bashrc或者.bash_profile被修改的时候，我们不必重启shell或者重新登录系统，就能使当前的更改生效。

## shebang

我们写一个shell脚本时，总是习惯在最前面加上一行 `#!/binbash`,它就是脚本的`shebang`,至于为什么叫这么个奇怪的名字，C语言和Unix的开发者丹尼斯·里奇称它为`可能是类似于"hash-bang"的英国风描述性文字`；

贴一段wiki上的解释:

> 在计算机科学中，Shebang是一个由井号和叹号构成的字符串行，其出现在文本文件的第一行的前两个字符。 在文件中存在Shebang的情况下，类Unix操作系统的程序载入器会分析Shebang后的内容，将这些内容作为解释器指令，并调用该指令，并将载有Shebang的文件路径作为该解释器的参数。

简单的说，它指示了此脚本运行时的解释器，所以，使用文件名直接执行shell脚本时，必须带上shebang; 此外，我们还可以在shebang后面直接附加选项，执行时我们默认使用选项执行；

如 `test.sh`的`shebang`为 `#!/bin/sh -x`，那我们执行脚本时:

```
./test.sh hello
```

相当于：

`bin/sh -x ./test.sh hello`;

而编写一个ssh自动登陆脚本，需要用到的shebang(解释器)为 `/usr/bin/expect`;

需要注意的是：在指定脚本解释器来执行脚本时，shebang会被指定的脚本解释器覆盖，即优先使用指定的脚本解释器来执行脚本（习惯性地用sh ./test.sh却提示command not found）

------

# expect解释器

expect是一个能实现自动和交互式任务的解释器，它也能解释常见的shell语法命令，其特色在以下几个命令：

### spawn命令：

`spawn command`命令会fork一个子进程去执行command命令，然后在此子进程中执行后面的命令；

在ssh自动登陆脚本中，我们使用 `spawn ssh user_name@ip_str`，fork一个子进程执行ssh登陆命令；

### expect命令：

expect命令是expect解释器的关键命令，它的一般用法为 `expect "string"`,即期望获取到string字符串,可在在string字符串里使用 * 等通配符;

string与命令行返回的信息匹配后，expect会立刻向下执行脚本；

### set timeout命令：

`set timeout n`命令将expect命令的等待超时时间设置为n秒，在n秒内还没有获取到其期待的命令，expect 为false,脚本会继续向下执行；

### send命令：

send命令的一般用法为 `send "string"`,它们会我们平常输入命令一样向命令行输入一条信息，当然不要忘了在`string`后面添加上 `\r` 表示输入回车；

### interact命令：

interact命令很简单，执行到此命令时，脚本fork的子进程会将操作权交给用户，允许用户与当前shell进行交互；

------

# 完成脚本

以下是一个完成版的脚本 `test.sh`：

```none
#!/usr/bin/expect                   // 指定shebang

set timeout 3                       // 设定超时时间为3秒
spawn ssh user_name@172.***.***.*** // fork一个子进程执行ssh命令
expect "*password*"                 // 期待匹配到 'user_name@ip_string's password:' 
send "my_password\r"                // 向命令行输入密码并回车
send "sudo -s\r" 
send "cd /data/logs\r"              // 帮我切换到常用的工作目录
interact                            // 允许用户与命令行交互
```

执行 `sudo chmod +x ./test.sh`命令给shell脚本添加执行权限；

运行 `./test.sh`命令，一键登陆成功！

简单的几个命令，，搭配起来解决了与命令行的交互问题后，很多复杂的功能也不在话下了~

------

# alias别名

脚本完成了，可是还是有些小瑕疵：

- 输入`./file_name.sh`命令太长。。。
- 只能在脚本目录中才能执行，不然使用绝对路径输出的命令更长。

这里我们想到了linux的alias命令：

### alias命令：

alias命令使用方式为 `alias alias_name="ori_command"`,将alias_name设置为ori_command的别名，这样我们输入执行alias_name，就相当于执行了ori_command;

可是，我们会发现，当你关闭当前shell后，再打开一个shell窗口，再使用alias_name,系统提示`command not found`;

有没有能保持命令的方式呢？编辑bash_profile文件。

### bash_profile文件

我们编辑bash_profile文件，此文件会在终端窗口创建的时候首先执行一次，所以可以帮我们再设置一次别名；

执行命令`vim ~./bash_profile`,在文件内部添加：

```
alias alias_name="/root_dir/../file_name.sh
```

保存后，再使用 `. ~./bash_profile`或`source ~./bash_profile` 在当前脚本执行一遍设置别名命令,完成设置；

这样，我们无论在哪个目录，只要输入`alias_name`命令，回车，真正的一键登录！
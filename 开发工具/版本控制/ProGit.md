书籍地址：https://git-scm.com/book/zh/v2

## Git基础

### 获取Git仓库

1. 将目录转化为Git仓库

```vbnet
git init
```

1. 克隆已有Git仓库

```vbnet
git clone <remote-url>
```

### 记录每次更新到仓库

### 查看提交记录

```vbnet
git log -p
```

| **选项**              | **说明**                                                     |
| --------------------- | ------------------------------------------------------------ |
| `-p`                  | 按补丁格式显示每个提交引入的差异。                           |
| `--stat`              | 显示每次提交的文件修改统计信息。                             |
| `--shortstat`         | 只显示 --stat 中最后的行数修改添加移除统计。                 |
| `--name-only`         | 仅在提交信息后显示已修改的文件清单。                         |
| `--name-status`       | 显示新增、修改、删除的文件清单。                             |
| `--abbrev-commit`     | 仅显示 SHA-1 校验和所有 40 个字符中的前几个字符。            |
| `--relative-date`     | 使用较短的相对时间而不是完整格式显示日期（比如“2 weeks ago”）。 |
| `--graph`             | 在日志旁以 ASCII 图形显示分支与合并历史。                    |
| `--pretty`            | 使用其他格式显示历史提交信息。可用的选项包括 oneline、short、full、fuller 和 format（用来定义自己的格式）。 |
| `--oneline`           | `--pretty=oneline --abbrev-commit` 合用的简写。              |
| `-<n>`                | 仅显示最近的 n 条提交。                                      |
| `--since`, `--after`  | 仅显示指定时间之后的提交。                                   |
| `--until`, `--before` | 仅显示指定时间之前的提交。                                   |
| `--author`            | 仅显示作者匹配指定字符串的提交。                             |
| `--committer`         | 仅显示提交者匹配指定字符串的提交。                           |
| `--grep`              | 仅显示提交说明中包含指定字符串的提交。                       |
| `-S`                  | 仅显示添加或删除内容匹配指定字符串的提交。                   |
| --no-merges           | 不显示合并提交                                               |



```vbnet
## pretty命令
git log --pretty=format:"%h - %an, %ar : %s"
```

| **选项** | **说明**                                      |
| -------- | --------------------------------------------- |
| `%H`     | 提交的完整哈希值                              |
| `%h`     | 提交的简写哈希值                              |
| `%T`     | 树的完整哈希值                                |
| `%t`     | 树的简写哈希值                                |
| `%P`     | 父提交的完整哈希值                            |
| `%p`     | 父提交的简写哈希值                            |
| `%an`    | 作者名字                                      |
| `%ae`    | 作者的电子邮件地址                            |
| `%ad`    | 作者修订日期（可以用 --date=选项 来定制格式） |
| `%ar`    | 作者修订日期，按多久以前的方式显示            |
| `%cn`    | 提交者的名字                                  |
| `%ce`    | 提交者的电子邮件地址                          |
| `%cd`    | 提交日期                                      |
| `%cr`    | 提交日期（距今多长时间）                      |
| `%s`     | 提交说明                                      |

### 撤销操作

撤销commit区域

```vbnet
## 用一个 新的提交 替换旧的提交
git commit --amend

## 以下举例
git commit -m 'initial commit'
git add forgotten_file
git commit --amend
```

撤销stage区域

```vbnet
git reset HEAD <file>
```

撤销workdir区域

```vbnet
git checkout -- <file>
```

### 远程仓库的使用

查看远程仓库

```plain
git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
```

添加远程仓库

```plain
git remote add <shortname> <url>
## 示例: git remote add chenxue https://github.com/paulboone/ticgit
## 现在你可以在命令行中使用字符串 chenxue 来代替整个 URL
```

远程仓库拉取和推送

```plain
## 拉取，将数据下载到你的本地仓库
git fetch <remote>
## 示例: git fetch chenxue

## 推送，
git push <remote> <branch>
## 示例: git push chenxue master

1edee6b..fbff5bc  master -> master
## 推送后的消息的基本格式是 <oldref>..<newref> fromref → toref
## oldref 的含义是推送前所指向的引用， newref 的含义是推送后所指向的引用， fromref 是将要被推送的本地引用的名字， toref 是将要被更新的远程引用的名字。
```

查看指定远程仓库

```plain
git remote show <remote>
## 示例: git remote show origin
```

重命名远程仓库

```plain
git remote rename <old_name> <new_name>
## 示例: git remote rename chenxue chenxue_new
```

移除远程仓库

```plain
git remote rm <remote>
## 示例: git remote rm chenxue
```

### 打标签

列出、查询标签

```plain
git tag
git tag -l <过滤条件>
## 示例: git tag -l "v3.18.*"
git show <标签名称>
## git show v1.4
```

创建标签

```plain
# 附注标签
git tag -a <标签名称> -m "<标签信息>"
# 轻量标签
git tag <标签名称>
# 后期打标
git tag -a <标签名称> <commitId>

## 示例: git tag -a v3.18 -m "测试"
```

推送标签

```plain
git push origin <标签名称>
## 示例: git push origin v1.5
```

删除标签

```plain
# 删除掉你本地仓库上的标签
git tag -d <标签名称>
# 删除远程服务器上的标签
git push origin --delete <标签名称>
```

检出标签

```plain
git checkout <标签名称>
## 示例: git checkout v3.18.3
```

### 别名

可以通过config命令，自定义git其他命令的别名

```plain
# 将git checkout 增加别名，等同于git co
git config --global alias.co checkout

# 将git reset HEAD -- fileA 改为 git unstage fileA
git config --global alias.unstage 'reset HEAD --'
```

## 分支模型

### 分支构成

1. Git 保存的不是文件的变化或者差异，而是一系列不同时刻的 快照 
2. 如下图，Git 仓库中有五个对象：三个 **blob** 对象（保存着文件快照）、一个 **树** 对象 （记录着目录结构和 blob 对象索引）以及一个 **提交** 对象（包含着指向前述树对象的指针和所有提交信息）

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739238617324-afcf1acf-10cf-45e4-9b63-b73c441c698b.png)

1. Git 的分支，其实本质上仅仅是指向提交对象的可变指针。Git 的默认分支名字是 master。 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 master 分支。 master 分支会在每次提交时自动向前移动。
2. HEAD 是特殊指针，指向当前所在的本地分支

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739238737124-168d1024-2086-456f-8bc3-7ebdb680cf79.png)

### 分支创建

```plain
# 创建testing分支，会在当前所在的提交对象上创建一个testing指针
git branch testing
```

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739238948739-5e92bc6c-5723-43a3-a537-761c3e79a3e8.png)

### 分支切换

```plain
# HEAD 指向 testing 分支了, 分支切换会改变工作目录中的文件
git checkout testing

# 创建新分支的同时切换过去
git checkout -b <newbranchname>
```

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739238965310-035b258a-7d7f-453a-a328-b054a61557c5.png)

### 分支合并

```plain
# 使用git merge <frombranch> 将其他分支合并到当前分支
git merge hotfix
```

#### fast-forward

在合并的时候，你应该注意到了“快进（fast-forward）”这个词。 由于你想要合并的分支 hotfix 所指向的提交 C4 是你所在的提交 C2 的直接后继， **没有提交分叉**，因此 Git 会直接将指针向前移动。换句话说，当你试图合并两个分支时， 如果顺着一个分支走下去能够到达另一个分支，那么 Git 在合并两者的时候， 只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有需要解决的分歧——这就叫做 “快进（fast-forward）”。

![合并前](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739239306195-31a79c99-4b52-404d-b144-b86559f714d2.png)

![合并后](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739239337850-e9dc9eb7-45ee-40de-a6c1-4a59c92aa4dd.png)

#### 合并提交

你的开发历史从一个更早的地方开始分叉开来（diverged）。 因为，master 分支所在提交并不是 iss53 分支所在提交的直接祖先，Git 不得不做一些额外的工作。 出现这种情况的时候，Git 会使用两个分支的末端所指的快照（C4 和 C5）以及这两个分支的公共祖先（C2），做一个简单的三方合并。

Git 将此次三方合并的结果做了一个新的快照并且自动创建一个新的提交指向它。 这个被称作一次合并提交，它的特别之处在于他有不止一个父提交。

![合并前](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739239509503-470c2518-8f50-4b61-b3ac-6d3f4a8c100c.png)

![合并后](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739239523391-528ba2da-2428-49a1-b5fc-c8e590cde1b4.png)

#### 冲突合并

当合并存在冲突（两个不同的分支中，对同一个文件的同一个部分进行了不同的修改），Git 做了合并，但是没有自动地创建一个新的合并提交。 Git 会暂停下来，等待你去解决合并产生的冲突。 你可以在合并冲突后的任意时刻使用 `git status `命令来查看那些因包含合并冲突而处于未合并（unmerged）状态的文件。

出现冲突的文件会包含一些特殊区段，看起来像下面这个样子：

```git
<<<<<<< HEAD:index.html
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> iss53:index.html
```

这表示 HEAD 所指示的版本（也就是你的 master 分支所在的位置，因为你在运行 merge 命令的时候已经检出到了这个分支）在这个区段的上半部分（======= 的上半部分），而 iss53 分支所指示的版本在 ======= 的下半部分。 为了解决冲突，你必须选择使用由 ======= 分割的两部分中的一个，或者你也可以自行合并这些内容。

在你解决了所有文件里的冲突之后，对每个文件使用 `git add` 命令来将其标记为冲突已解决。 一旦暂存这些原本有冲突的文件，Git 就会将它们标记为冲突已解决。

如果你想使用图形化工具来解决冲突，你可以运行 `git mergetool`，该命令会为你启动一个合适的可视化合并工具

### 删除分支

```plain
git branch -d hotfix
```

### 分支查看

当前所有分支的一个列表

\* 字符：代表现在检出的那一个分支，即当前 HEAD 指针所指向的分支。 

```git
$ git branch
  iss53
* master
  testing
```

查看所有分支的最后一次提交

```git
$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
```

查看设置的所有跟踪分支

```plain
$ git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
  master    1ae2a45 [origin/master] deploying index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
  testing   5ea463a trying something new
```

过滤这个列表中已经合并或尚未合并到当前分支的分支

```git
# 已合并到当前分支的分支
$ git branch --merged
  iss53
* master

# 未合并到当前分支的分支
$ git branch --no-merged
  testing
```

### 分支开发工作流

长期分支：保留完全稳定的代码，已经发布或即将发布的代码。如master、develop

主题分支：短期分支，它被用来实现单一特性或其相关工作。如hotfix、issue、feature

### 远程分支

远程引用是对远程仓库的引用（指针），包括分支、标签等等。通过 `git remote show <remote>` 获得远程分支的更多信息。

远程跟踪分支是远程分支状态的引用。它们是你无法移动的本地引用。一旦你进行了网络通信， Git 就会为你移动它们以精确反映远程仓库的状态。

它们以 <remote>/<branch> 的形式命名。 例如，如果你想要看你最后一次与远程仓库 origin 通信时 master 分支的状态，你可以查看 origin/master 分支。

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739240784187-5668a5c5-d923-4ecf-aed5-0a230f95b278.png)

如果你在本地的 master 分支做了一些工作，在同一段时间内有其他人推送提交到 git.ourcompany.com 并且更新了它的 master 分支，这就是说你们的提交历史已走向不同的方向。 即便这样，只要你保持不与 origin 服务器连接（并拉取数据），你的 origin/master 指针就不会移动。

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739240829986-fbc9543c-dae1-416b-98fa-cb0ad473bf67.png)

如果要与给定的远程仓库同步数据，运行 `git fetch <remote>` 命令。从中抓取本地没有的数据，并且更新本地数据库，移动 origin/master 指针到更新之后的位置。

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739240879306-39b7d663-1d10-456d-a8c4-54fe1825f2fb.png)

跟踪分支

从一个远程跟踪分支检出一个本地分支会自动创建所谓的“跟踪分支”（它跟踪的分支叫做“上游分支”）。当克隆一个仓库时，它通常会自动地创建一个跟踪 origin/master 的 master 分支。

我理解，与远程分支有关联的本地分支，就是跟踪分支

如果想要将本地分支与远程分支设置为不同的名字，你可以轻松地使用上一个命令增加一个不同名字的本地分支

```plain
$ git checkout -b sf origin/serverfix
Branch sf set up to track remote branch serverfix from origin.
Switched to a new branch 'sf'
```

设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支， 你可以在任意时间使用 -u 或 --set-upstream-to 选项运行 git branch 来显式地设置

```plain
$ git branch -u origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
```

删除远程分支

```plain
$ git push origin --delete serverfix
To https://github.com/schacon/simplegit
 - [deleted]         serverfix
```

### Rebase

Git 中整合来自不同分支的修改主要有两种方法：merge 以及 rebase

rebase是将提交到某一分支上的所有修改都移至另一分支上，就好像“重新播放”一样。

```plain
$ git checkout experiment
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: added staged command
```

原理是首先找到这两个分支（即当前分支 experiment、变基操作的目标基底分支 master） 的最近共同祖先 C2，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件， 然后将当前分支指向目标基底 C3, 最后以此将之前另存为临时文件的修改依序应用。

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1739325318840-38bc2bb6-0121-4184-a173-48aa3edffd18.png)

rebase和merge的相同点和不同点，最终结果所指向的快照始终是一样的，只不过提交历史不同罢了，使用哪个取决于项目情况和成员的观点，并没有哪种方式更好。rebase的提交历史更简洁。使用rebase后，其他人的merge只需要fast-forward便可。

使用rebase注意事项：如果提交存在于你的仓库之外，而别人可能基于这些提交进行开发，那么不要执行变基。

白话讲，只对不会离开你电脑的提交执行变基，那就不会有事。 如果你对已经推送过的提交执行变基，但别人没有基于它的提交，那么也不会有事。 如果你对已经推送至共用仓库的提交上执行变基命令，并因此丢失了一些别人的开发所基于的提交， 那你就有大麻烦了

变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交。 如果你已经将提交推送至某个仓库，而其他人也已经从该仓库拉取提交并进行了后续工作，此时，如果你用 git rebase 命令重新整理了提交并再次推送，你的同伴因此将不得不再次将他们手头的工作与你的提交进行整合。会发现有两个提交的作者、日期、日志是一样的，这会令人感到混乱。 此外，如果同伴整合后将这一堆又推送到服务器上，你实际上是将那些已经被变基抛弃的提交又找了回来，这会令人感到更加混乱。 

如果你 真的 遭遇了类似的处境，那么可以用用变基解决变基。不是执行merge，而是执行 git rebase <远程仓库>

其他rebase常用命令

1. 下面的命令的意思是：“取出 `client` 分支，找出它从 `server` 分支分歧之后的补丁， 然后把这些补丁在 `master` 分支上重放一遍，让 `client` 看起来像直接基于 `master` 修改一样”。

```plain
$ git rebase --onto master server client
```

1. 下面的命令意思是： 将 server 中的修改变基到 master 上 所示，server 中的代码被“续”到了 master 后面

```plain
$ git rebase master server
```

## 服务器上的Git

### 协议

分为本地协议、HTTP、SSH、Git协议

**本地协议**：优点是简单，适合小团队；缺点是配置不方便，速度不够快

```plain
$ git clone file:///srv/git/project.git
```

**HTTP**：智能 HTTP 协议或许已经是最流行的使用 Git 的方式了，它既支持像 git:// 协议一样设置匿名服务， 也可以像 SSH 协议一样提供传输时的授权和加密。

**SSH**：优点是搭建方便；缺点是不支持匿名访问 Git 仓库

**Git**：优点是网络传输协议里最快的；缺点是缺乏授权机制。

### 服务器上搭建Git

操作说明按照文档实施即可

**简单概括SSH协议搭建过程**：

1. 服务器上生成bare仓库。此时用户就可通过`$ git clone user@git.example.com:/srv/git/my_project.git`访问了
2. 服务器上建立git用户，将用户的SSH 公钥，加入 git 账户的` ~/.ssh/authorized_keys`文件。此时用户就可通过`git clone git@gitserver:/srv/git/project.git`访问了
3. 将服务器上的shell替换为git-shell。此步骤实 git 只能利用 SSH 连接对 Git 仓库进行推送和拉取操作，而不能登录机器并取得普通 shell。
4. 在authorized_keys中增加`no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty`。限制 SSH 端口转发来访问任何可达的 git 服务器。



**简单概括Git协议搭建过程**：

1. 运行Git守护进程`$ git daemon --reuseaddr --base-path=/srv/git/ /srv/git/`
2. 将上述命令放在systemd中，随操作系统启动
3. 需要告诉 Git 哪些仓库允许基于服务器的无授权访问。 你可以在每个仓库下创建一个名为 git-daemon-export-ok，该文件将允许 Git 提供无需授权的项目访问服务

**简单概括HTTP协议搭建**：

1. 在服务器上启用一个 Git 自带的名为 git-http-backend 的 CGI 脚本
2. 在服务中间件上增加授权，以支持账号、密码访问。例如Apache的.htpasswd 文件



**白屏化Git服务器搭建**：

方式一：Git 提供了一个叫做 GitWeb 的 CGI 脚本，服务器上安装了轻量级 Web 服务器比如 lighttpd 或 webrick， Git 提供了一个命令来让你启动一个临时的服务器，可以访问 http://gitserver/ 在线查看你的版本库

```bash
$ git instaweb --httpd=webrick
```

方式二：使用Gitlab，基于数据库的 web 应用，是一个成熟的git管理系统， 它提供了丰富的功能和美观的界面。

方式三：使用第三方托管，如Github或Gitee

## 分布式Git

### 常见工作流程

以下是书中提到的常见的工作流模式，在实际的工作中，也可能会遇到许多更适合自己的特定工作流程的变种。

**集中式工作流**：两个开发者从中心仓库克隆代码下来，同时作了一些修改，那么只有第一个开发者可以顺利地把数据推送回共享服务器。 第二个开发者在推送修改之前，必须先将第一个人的工作合并进来，这样才不会覆盖第一个人的修改。这种模式与SVN概念一样，比较简单，适合于中小型团队。

![集中式工作流](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1740016821474-f420b89e-a0fc-48cb-acf3-0281b178389d.png)

**集成管理者工作流**：开发者从项目克隆出一个自己的仓库或分支，然后将自己的修改推送上去。 接着请求项目仓库的管理者拉取更新合并到项目中。 管理者可以将开发者的仓库或分支作为远程仓库添加进来，在本地评审和测试变更，将其合并入主分支并推送回项目仓库。这是 GitHub 和 GitLab 等集线器式（hub-based）工具最常用的工作流程。

![集成管理者工作流](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1740017072440-debdc6c2-42d6-4f80-9d94-ea05cd3bdde0.png)

**主管与副主管工作流**：副主管（lieutenant） 的各个集成管理者分别负责集成项目中的特定部分。 所有这些副主管头上还有一位称为 主管（dictator） 的总集成管理者负责统筹。这种工作流程并不常用，一般拥有数百位协作开发者的超大型项目才会用到这样的工作方式，如 Linux 内核项目。

1. 普通开发者在自己的主题分支上工作，并根据 master 分支进行变基。 这里是主管推送的参考仓库的 master 分支。
2. 副主管将普通开发者的主题分支合并到自己的 master 分支中。
3. 主管将所有副主管的 master 分支并入自己的 master 分支中。
4. 最后，主管将集成后的 master 分支推送到参考仓库中，以便所有其他开发者以此为基础进行变基。

![主管与副主管工作流](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1740017181623-0bdab16f-2ab4-472c-8719-ecdb2322f13a.png)

### 向项目贡献

**提交准则**

1. 提交不应该包含任何空白错误。在提交前，运行 git diff --check，它将会找到可能的空白错误并将它们为你列出来。
2. 让每一个提交成为一个逻辑上的独立变更集。将工作拆分为每个问题一个提交，并且为每一个提交附带一个有用的信息，不要提交为一个巨大的提交
3. 创建优质提交信息。以少于 50 个字符（25个汉字）的单行开始且简要地描述变更，再接着换行是一个更详细的解释。可通过`git log --no-merges` 来看看漂亮的格式化的项目提交历史是否符合预期。



**私有小团队**：一个master主干，开发人员本地仓库merge和push。类似SVN

**私有管理团队**：多个主干分支，开发人员并行开发，远端合并

**派生公开项目**：fork仓库到本地，修改提交后，发起PR

**邮件公开项目**：使用 `format-patch` 生成的一封电子邮件应用的提交正确地保留了所有的提交信息。配置`~/.gitconfig` 文件的 sendmail 区块，使用`git send-email`发送补丁

```plain
git format-patch -M origin/master
git send-email *.patch
```

### 维护项目

**应用邮件补丁**：有两种应用该种补丁的方法：使用 git apply，或者使用 git am。

- **Apply**：收到了一个使用 `git diff` 或 `Unix diff` 命令的变体（不推荐使用这种方式，具体见下一节） 创建的补丁，可以使用 `git apply` 命令来应用。`git apply` 命令采用了一种“全部应用，否则就全部撤销（apply all or abort all）”的模型。对补丁运行 `git apply --check` 命令检查补丁是否可以顺利应用。
- **Am**：由 `format-patch` 命令生成的补丁，应该使用 `git am` 命令。

```plain
## 传递 -3 选项来使 Git 尝试进行三方合并
$ git am -3 0001-seeing-if-this-helps-the-gem.patch

## -i交互模式
$ git am -3 -i mbox
```

**应用分支补丁**

1. checkout远程分支到本地
2. 检查合入的修改

```plain
## contrib分支中排除 master 分支中的提交，等同于master..contrib
$ git log contrib --not master

## 通过把 … 置于另一个分支名后来对该分支的最新提交与两个分支的共同祖先进行比较
## 显示自当前主题分支与 master 分支的共同祖先起，该分支中的工作。 这个语法很有用，应该牢记。
$ git diff master...contrib
```

1. 合入修改

1. 基于`git merge`合入
2. 基于 `git rebase` 合入
3. 基于 `git cherry-pick <sha1>` 挑选合入

1. 为发布打加签tag

通过`git tag -s`为tag加签名

```plain
$ git tag -s v1.5 -m 'my signed 1.5 tag'
```

1. 生成构建号

运行 `git describe` 命令，Git 将会生成一个字符串， 它由最近的标签名、自该标签之后的提交数目和你所描述的提交的部分 SHA-1 值（前缀的 g 表示 Git）构成。

```plain
$ git describe master
v1.6.2-rc1-20-g8c5b85c
```

1. 发布快照

使用 `git archive` 命令将代码打包发出

```plain
$ git archive master --prefix='project/' | gzip > `git describe master`.tar.gz
$ ls *.tar.gz
v1.6.2-rc1-20-g8c5b85c.tar.gz
```

1. 提交简报

使用 `git shortlog` 命令可以快速生成一份包含从上次发布之后项目新增内容的修改日志（changelog）类文档

```plain
$ git shortlog --no-merges master --not v1.0.1
```

## #GitHub

本章节只有一个技巧有价值学习

**合并请求引用**

处理许多合并请求，不想添加一堆 remote 或每次都要做一次拉取，再进行本地合并的话

实际上 GitHub 在服务器上把合并请求分支视为一种 “假分支”。 默认情况下你克隆时不会得到它们，但它们还是隐式地存在，可以使用`ls-remote` 的低级命令，得到一个版本库里所有的分支，标签和其它引用（reference）的列表.

此命令会得到一些以 refs/pull/ 开头的引用。 它们实际上是分支，但因为它们不在 refs/heads/ 中，所以正常情况下你克隆时不会从服务器上得到它们。

```plain
$ git ls-remote origin
10d539600d86723087810ec636870a504f4fee4d	HEAD
10d539600d86723087810ec636870a504f4fee4d	refs/heads/master
6a83107c62950be9453aac297bb0193fd743cd6e	refs/pull/1/head
```

每个合并请求有两个引用——其中以 /head 结尾的引用指向的提交记录与合并请求分支中的最后一个提交记录是同一个。 所以如果有人在我们的版本库中开启了一个合并请求，他们的分支叫做 bug-fix， 指向 a5a775 这个提交记录，那么在 我们的 版本库中我们没有 bug-fix 分支（因为那是在他们的 fork 中）， 但我们 可以 有一个 pull/<pr#>/head 指向 a5a775。 这意味着我们可以很容易地拉取每一个合并请求分支而不用添加一堆远程仓库。

```plain
$ git fetch origin refs/pull/958/head
From https://github.com/libgit2/libgit2
 * branch            refs/pull/958/head -> FETCH_HEAD
```

这告诉 Git： “连接到 origin 这个 remote，下载名字为 refs/pull/958/head 的引用。” 然后把指针指向 .git/FETCH_HEAD 下面你想要的提交记录。 然后你可以用 `git merge FETCH_HEAD` 把它合并到你想进行测试的分支。

## Git工具

### 查看提交记录

**查看单个提交**

1. 运行 git log 命令来定位提交
2. git show <sha1>来查看提交
3. git show <branchName>来查看分支最后一次提交
4. **git rev-parse** <branchName>来查看分支对应的提交sha1
5. git reflog来查看引用日志，其记录了最近几个月的 HEAD 和分支引用所指向的历史，引用日志只存在于本地仓库
6. **祖先引用**：在引用的尾部加上一个 `^`表示该引用的上一个提交，使用 `git show HEAD^` 来查看上一个提交，也就是 “HEAD 的父提交”；

1. 在 `^` 后面添加一个数字来指明想要 哪一个 父提交——例如 `d921970^2` 代表 “d921970 的第二父提交” 这个语法只适用于合并的提交，因为合并提交会有多个父提交。
2. `~`号也表示祖父应用，区别于`^`在于`HEAD~2` 代表“第一父提交的第一父提交”，也就是“祖父提交”
3. 可以组合使用这两个语法——你可以通过 `HEAD~3^2` 来取得之前引用的第二父提交（假设它是一个合并提交）。

**查看提交区间**

1. **双点**：选出在一个分支中而不在另一个分支中的提交，即减法。如`git log master..experiment`表示在 experiment 分支中而不在 master 分支中的提交

1. 当查看超过两个以上的分支时，可使用`^` 字符或者 `--not`，如`git log refA refB ^refC`表示在A、B分支，不在C分支的提交

1. **三点**：选择出被两个引用 之一 包含但又不被两者同时包含的提交，即并集-交集。如`git log --left-right master...experiment`表示在master 或者 experiment 中包含的但不是两者共有的提交。

### 交互式暂存

通过运行 `git add` 时使用 `-i` 或者 `--interactive` 选项，进入一个交互式终端模式，可实现将工作目录中的文件或内容拆分为若干提交。

```plain
$ git add -i
           staged     unstaged path
  1:    unchanged        +0/-1 TODO
  2:    unchanged        +1/-1 index.html
  3:    unchanged        +5/-1 lib/simplegit.rb

*** Commands ***
  1: [s]tatus     2: [u]pdate      3: [r]evert     4: [a]dd untracked
  5: [p]atch      6: [d]iff        7: [q]uit       8: [h]elp
```

- **status**：等同于`git status`
- **update**：选择需要stage的文件，每个文件前面的 * 意味着选中的文件将会被暂存。 如果在 Update>> 提示符后不输入任何东西并直接按回车，Git 将会暂存之前选择的文件。
- **revert**：撤销已经staged的文件，变为unstaged
- **patch**：对已选择文件内容的每一个部分，它都会一个个地显示文件区别并询问你是否想要暂存
- **diff**：查看已staged的文件内容差异，等同于git diff --cached

### 贮藏与清理

#### 贮藏

- 通过`git stash`将(1)工作目录跟踪的修改和(2)暂存的改动，保存到一个栈上，可以在任何时候重新应用这些改动（甚至在不同的分支上）。
- 通过`git stash list`列出贮藏的东西
- 通过`git stash apply` 应用贮藏的修改。变形命令`git stash apply stash@{2}`应用更旧的贮藏。如果有任何东西不能干净地应用，Git 会产生合并冲突。

```
git stash apply`不会将之前暂存的文件重新暂存，需保持和贮藏前一致，使用`git stash apply --index
```

- 通过 `git stash drop` 加上将要移除的贮藏的名字来移除它，或运行 `git stash pop` 来应用贮藏然后立即从栈上扔掉它

```plain
$ git stash
Saved working directory and index state \
  "WIP on master: 049d078 added the index file"
HEAD is now at 049d078 added the index file
(To restore them type "git stash apply")

$ git stash list
stash@{0}: WIP on master: 049d078 added the index file

$ git stash apply --index

$ git stash drop stash@{0}
## git stash pop
```

- `git stash --keep-index` 贮藏所有已暂存的内容，同时还要将它们保留在暂存区(Index)
- `git stash -u`会贮藏任何未跟踪文件。 然而，仍不会包含ignore的文件。 要额外包含ignore的文件，请使用 --all 或 -a 选项
- `git stash --patch`会交互式地提示哪些改动想要贮藏、哪些改动需要保存在工作目录中
- 通过`git stash branch <new branchname>` 以你指定的分支名创建一个新分支，检出贮藏工作时所在的提交，在新分支轻松恢复贮藏内容，然后在应用成功后丢弃贮藏

#### 清理工作目录

- 通过`git clean` 从工作目录中移除未被追踪的文件
- 通过`git clean -d` 移除工作目录中所有未追踪的文件以及空的子目录
- 通过`git clean -n`或`--dry-run` 做一次演习然后告诉你 将要 移除什么
- 通过`git clean -x`移除内容包含忽略的未跟踪文件（在.gitignore中的）
- 通过`git clean -i`启动交互模式清理

记住`git clean -n -d -x`的组合命令

### 签署

通过签署，验证提交是不是真正地来自于可信来源，是密码访问基础上的加强。

1. 首先，需要使用GPG私钥`gpg --gen-key`，然后配置到git中`git config --global user.signingkey`。
2. tag加签：签名者使用私钥，在创建标签时通过`-s`签名，如`git tag -s v1.5 -m 'my signed'`
3. tag验签：签署者的公钥需要在你的钥匙链中，`git tag -v <tag-name>`验证签名
4. commit加签：commit通过`-S`签名，如`git commit -a -S -m 'signed commit'`。

1. `git log` 也有一个 `--show-signature` 选项来查看及验证这些签名
2. `git merge` 命令附加 `-S` 选项来签署自己生成的合并提交

1. commit验签：`git merge` 与`git pull`可以使用 `--verify-signatures` 选项来检查并拒绝没有携带可信 GPG 签名的提交。如`git merge --verify-signatures non-verify`

签署标签与提交很棒，但是如果决定在正常的工作流程中使用它，你必须确保团队中的每一个人都理解如何这样做。

### 搜索

通过`git grep`可以从工作目录中查找字符串或表达式

- `-n`通过`git grep -n <keyword>`输出匹配行的行号
- `-c`通过`git grep -c <keyword>`输出匹配文件的概述的信息
- `-p`通过`git grep -p <keyword> <path>`每一个匹配的字符串所在的方法或函数
- `--and`通过`git grep <keyword1> --and <keyword2> <branchName>`，在指定分支中查找多个匹配在同一文本行的记录

通过`git log`从日志中查找

- `-S`通过`git log -S <keyword> --oneline`输出匹配内容所在的历史日志
- `-L`通过`git log -L <keyword:file>`输出匹配代码中一行或者一个函数的历史

### 重写历史

以下内容都会导致sha1变化，如果已经推送了最后一次提交就不要修正它。

**修改最后一次提交**

- 作出你想要补上的修改， 暂存它们，通过`git commit --amend`将最后一次的提交信息载入到编辑器中供你修改，替换最后一次提交。

**修改历史提交**

1. 通过`git rebase -i <父提交>`批量变基以改变历史。`HEAD~3`表示修改最后三次提交。

```plain
## 在 HEAD~3..HEAD 范围内的每一个修改了提交信息的提交及其 所有后裔 都会被重写
$ git rebase -i HEAD~3
pick f7f3f6d changed my name a bit
pick 310154e updated README formatting and added blame
pick a5f4a0d added cat-file

# Rebase 710f0f8..a5f4a0d onto 710f0f8
......
```

1. 提交显示的顺序与log是相反的，最旧的提交在最上面，因为它是第一个将要rebase的
2. 根据rebase命令弹出编辑器的提示，将想修改的每一个提交前面的 `pick' 改为 `edit'，当保存并退出编辑器时，Git 将你按自上而下的顺序，展示最上一个标记为`edit`的提交。接下来通过`git commit --amend`修改提交内容，修改好后通过`git rebase --continue`去修改下一个提交，直至结束

**重新排序提交**

- 通过`git rebase -i <父提交>`的交互界面，修改提交顺序并保存，实现提交重新排序

**压缩提交**

- 通过`git rebase -i <父提交>`的交互界面，修改的提交前面的 `pick' 改为 `squash'并保存，可以将提交压缩进父提交中（父提交必须是pick，否则无法squash）

**拆分提交**

- 通过`git rebase -i <父提交>`的交互界面，修改的提交前面的 `pick' 改为 `edit'并保存，Git 带你到edit提交，在这里通过 `git reset HEAD^` 做一次该提交的mixed reset，撤消该提交并将修改的文件取消暂存。现在可以暂存并提交文件直到有几个提交，然后当完成时运行 `git rebase --continue`继续。

**filter-branch**

git filter-branch 有很多陷阱，不再推荐使用它来重写历史。 请考虑使用 git-filter-repo来实现下列案例

通过脚本的方式改写大量提交的话可以使用filter-branch，如下常用用途：

1. **从每一个提交中移除一个文件**：误提交包含密码的文件场景，通过`--tree-filter`选项在检出项目的每一个提交后运行指定的命令然后重新提交结果。为了让 filter-branch 在所有分支上运行，可以给命令传递 `--all` 选项

```plain
$ git filter-branch --tree-filter 'rm -f passwords.txt' HEAD
Rewrite 6b9b3cf04e7c5686a9cb838c3f36a8cb6a0fc2bd (21/21)
Ref 'refs/heads/master' was rewritten
```

1. **全局修改邮箱地址**：通过使用`--commit-filter`一次性修改多个提交中的邮箱地址。

```plain
$ git filter-branch --commit-filter '
        if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ];
        then
                GIT_AUTHOR_NAME="Scott Chacon";
                GIT_AUTHOR_EMAIL="schacon@example.com";
                git commit-tree "$@";
        else
                git commit-tree "$@";
        fi' HEAD
```

### 重置解密（reset和checkout）

此篇比较复杂，我尝试简单概述下，如果还不理解就需要看原文了

#### checkout和reset的区别

要搞清楚区别，需要先理解git的三个区域（原文是三棵树），分为HEAD、Index(暂存区)、Workdir(工作目录)

- HEAD 是当前分支引用的指针，它总是指向该分支上的最后一次提交。 
- Index 是下一次提交的内容，即暂存区
- 工作目录 是开发工程中的文件

checkout原理

当修改工作目录中的文件后，通过`git add`将工作目录中的内容，复制到Index区中，接着运行 git commit，Git会将Index中的内容保存为一个快照，然后创建一个指向该快照的提交对象，最后更新 HEAD和master 来指向本次提交。

当执行checkout命令时，流程正好相反：

1. Git会修改 HEAD 指向新的分支引用（注意：HEAD会指向新分支，master不变）
2. 将 Index 填充为该分支最后提交的快照
3. 然后将 Index 的内容复制到 工作目录 中

![img](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1740479540680-9db345d2-0af9-4a0b-a163-fc4eb71d0504.png)

reset原理

当执行reset命令时

1. `--soft`：Git会移动 HEAD 的指向，这与改变 HEAD 自身不同（checkout 所做的），这意味着如果 HEAD 设置为 master 分支（例如，你正在 master 分支上）， 运行 git reset 9e5e6a4 将会使 master 指向 9e5e6a4。命令到此结束。`git reset --soft HEAD~`本质上是撤销了上一次 git commit 命令。
2. `--mixed`(默认选项)：执行第1步，然后`reset` 会用 HEAD 指向的当前快照的内容来更新Index区，命令到此为止。该动作与checkout一致
3. `--hard`：执行第1、2步，然后`reset` 会将 Index 的内容复制到 工作目录。该操作会强制覆盖文件，不可找回，是危险命令。

#### reset常用用法

**限制reset范围**

- 通过`git reset <path>`，重置该路径的Index区（HEAD由于只是一个指针，无法让它同时指向两个提交中各自的一部分，所以HEAD移动是跳过不动的）
- 通过`git reset <branch> <path>`，重置指定版本、指定路径的Index区，HEAD跳过不动

**压缩提交**

相比`rebase -i`更简单。`reset --soft`重置HEAD指向祖先提交，Index、工作目录保持不变，再重新提交即可。

```plain
git reset --soft HEAD~2
git commit
```

#### 总结

- 不带路径：`git checkout [branch]`与`git reset --hard [branch]`非常相似。

- 差异(1)：checkout对工作目录采用合并，更安全，而reset直接覆盖
- 差异(2)：checkout移动HEAD自身指向，reset移动HEAD分支的指向

- 带路径：`git checkout [branch] <path>`与`git reset --hard [branch] <path>`相似。

- 相同(1)：跳过HEAD移动，保持不变
- 相同(2)：checkout直接覆盖工作目录中path文件，不安全。

|                             | **HEAD** | **Index** | **Workdir** | **WD Safe?** |
| --------------------------- | -------- | --------- | ----------- | ------------ |
| **Commit Level**            |          |           |             |              |
| `reset --soft [commit]`     | REF      | NO        | NO          | YES          |
| `reset [commit]`            | REF      | YES       | NO          | YES          |
| `reset --hard [commit]`     | REF      | YES       | YES         | **NO**       |
| `checkout <commit>`         | HEAD     | YES       | YES         | YES          |
| **File Level**              |          |           |             |              |
| `reset [commit] <paths>`    | NO       | YES       | NO          | YES          |
| `checkout [commit] <paths>` | NO       | YES       | YES         | **NO**       |

### 高级合并

#### 合并冲突

**中断合并**

```plain
$ git merge --abort
```

**忽略空白**

- 使用 `-Xignore-all-space` 或 `-Xignore-space-change` 选项。 第一个选项在比较行时 完全忽略 空白修改，第二个选项将一个空白符与多个连续的空白字符视作等价的。

```plain
$ git merge -Xignore-space-change whitespace
```

**手动解决冲突再合并**

1. 通`git show sha1 > file`导出任何一边的文件，接下来为这些单独的文件重试一次合并

1. stage 1 是它们共同的祖先版本，stage 2 是你的版本ours，stage 3 来自于 `MERGE_HEAD`，即你将要合并入的版本（“theirs”）

```plain
$ git show :1:hello.rb > hello.common.rb
$ git show :2:hello.rb > hello.ours.rb
$ git show :3:hello.rb > hello.theirs.rb
```

1. 通过 对`hello.theirs.rb`修改，解决冲突
2. 通过`git merge-file -p hello.ours.rb hello.common.rb hello.theirs.rb > hello.rb`合并最终文件
3. 最后通过`git clean -f`清理导出的临时文件

**checkout冲突**

- 要看一下合并冲突是什么，查明这个冲突是如何产生，使用带 `--conflict` 选项的 `git checkout`。可以传递参数 diff3 或 merge（默认选项）。 如果传给它 diff3，Git 会使用一个略微不同版本的冲突标记： 不仅仅只给你 “ours” 和 “theirs” 版本，同时也会有 “base” 版本在中间来给你更多的上下文。

```plain
$ git checkout --conflict=diff3 hello.rb
def hello
<<<<<<< ours
  puts 'hola world'
||||||| base
  puts 'hello world'
=======
  puts 'hello mundo'
>>>>>>> theirs
end
```

- `git checkout` 命令也可以使用 `--ours` 和 `--theirs` 选项，这是一种无需合并的快速方式，你可以选择留下一边的修改而丢弃掉另一边修改。

**合并日志**

通过`git log`回顾历史，查询冲突分支的提交差异

```plain
$ git log --oneline --left-right HEAD...MERGE_HEAD
< f1270f7 update README
< 9af9d3b add a README
< 694971d update phrase to hola world
> e3eb223 add more tests
> 7cff591 add testing script
> c3ffff1 changed text to hello mundo
```

通过`--merge`只显示冲突文件的提交历史；`-p`显示冲突内容

```plain
$ git log --oneline --left-right --merge
< 694971d update phrase to hola world
> c3ffff1 changed text to hello mundo
```

**组合式差异格式**

- 在合并冲突中，通过`git diff`查看冲突差异，每一行给你两列数据。 第一列为“ours” 分支与工作目录的文件区别， 第二列为 “theirs” 分支与工作目录的文件区别。

- 通过 `git diff --ours`、`--theirs` 、`--base`可以查看单独的差异

```plain
$ git diff
def hello
++<<<<<<< HEAD
 +  puts 'hola world'
++=======
+   puts 'hello mundo'
++>>>>>>> mundo
  end
```

- 冲突解决后

```plain
$ git diff
def hello
-   puts 'hola world'
 -  puts 'hello mundo'
++  puts 'hola mundo'
  end
```

#### 撤销合并

- **修复引用**：通过`git reset --hard HEAD~`，重置到父提交，会有变基风险。
- **还原提交**：通过`git revert -m 1 HEAD`，提交一个撤销`frombranch`内容的新提交，其中`-m 1`表示被保留下来的父结点为HEAD。

#### 其他合并

- **合并ours或theirs**：通过给`merge`命令传递 `-Xours` 或 `-Xtheirs` 参数，当有任何冲突时，Git简单地选择一边。

- 通过给`merge`命令传递`-s ours`或`-s theirs`，Git会做一次假的合并，直接以两边分支作为父结点创建新合并提交，并直接应用所传参数的分支，而不管另一个。

- **子树合并：*****不常用，了解即可***

子树合并思想是有两个项目，并且其中一个是另一个项目的子目录。当执行一个子树合并时，Git 可以识别是子树并正确的合并。

1. 在master 分支中，将 rack_back 分支拉取到 master 分支中的 rack 子目录。

```plain
$ git read-tree --prefix=rack/ -u rack_branch
```

1. 当 Rack 项目有更新时，我们可以切换到子树分支来拉取上游的变更。

```plain
$ git checkout rack_branch
$ git pull
```

1. 接着，我们可以将这些变更合并回 master 分支。 使用 `--squash` 选项和使用 `-Xsubtree` 选项（它采用递归合并策略）， 用来可以拉取变更并且预填充提交信息。 Rack 项目中所有的改动都被合并了，等待被提交到本地。

```plain
$ git checkout master
$ git merge --squash -s recursive -Xsubtree=rack rack_branch
```

1. 想查看 rack 子目录和 rack_branch 分支的差异—— 来确定你是否需要合并它们——你不能使用普通的 diff 命令。 取而代之的是，你必须使用 git diff-tree 来和你的目标分支做比较

```plain
$ git diff-tree -p rack_branch
```

### Rerere

**作用**：让 Git 记住解决一个块冲突的方法， 这样在下一次看到相同冲突时，Git 可以为你自动地解决它

**场景**：需要做很多次重新合并，或者想要一个开发分支始终与 master 分支保持最新但却不想处理一大堆合并， 或者经常变基，打开 rerere 功能可以帮助得更好。

**使用方法**：启用 rerere 功能，只需运行以下配置选项即可

```plain
$ git config --global rerere.enabled true
```

启动后，每次解决冲突merge后，会输出`Recorded preimage for FILE`，下一次看到相同冲突时(相同文件相同内容），Git 可以为你自动地解决它，并输出`Resolved FILE using previous resolution.`

```plain
$ git merge i18n-world
Auto-merging hello.rb
CONFLICT (content): Merge conflict in hello.rb
Recorded preimage for 'hello.rb' // 记录解决方法
Automatic merge failed; fix conflicts and then commit the result.
```

其他命令：

```plain
$ git rerere diff // 将会显示解决方案的当前状态——开始解决前与解决后的样子
$ git rerere status // 记录的合并前状态
$ git rerere // 手动触发重新解决当前冲突
```

### 使用Git调试

#### 文件标注Blame

**作用**：显示任何文件中每行最后一次修改的提交记录

**使用方法**：`git blame -L 行号起始,行号结束 文件名`

**示例**：

- 前缀 `^` 指出了该文件自第一次提交后从未修改的那些行。
- 在 `git blame` 后面加上一个 `-C`，Git 会分析你正在标注的文件， 并且尝试找出文件中从别的地方复制过来的代码片段的原始出处。

```plain
$ git blame -L 69,82 Makefile
b8b0618cf6fab (Cheng Renquan  2009-05-26 16:03:07 +0800 69) ifeq ("$(origin V)", "command line")
b8b0618cf6fab (Cheng Renquan  2009-05-26 16:03:07 +0800 70)   KBUILD_VERBOSE = $(V)
^1da177e4c3f4 (Linus Torvalds 2005-04-16 15:20:36 -0700 71) endif
^1da177e4c3f4 (Linus Torvalds 2005-04-16 15:20:36 -0700 72) ifndef KBUILD_VERBOSE
```

#### 二分查找Bisect

**作用**：已经的提交中，通过使用 git bisect 来帮助查找引入问题的提交是哪个

**使用方法**：

- 首先执行 `git bisect start` 来启动，接着执行 `git bisect bad` 来告诉系统当前你所在的提交是有问题的。 然后你必须使用 `git bisect good <good_commit>`，告诉 bisect 已知的最后一次正常状态是哪次提交。然后Git会checkout出中间的提交，你继续测试，通过 `git bisect good` 或 `git bisect bad`来告诉 Git 当前提交是否有问题，然后继续寻找，直至Git确定引入问题的提交是哪个。完成这些操作之后，你应该执行 `git bisect reset` 重置你的 HEAD 指针到最开始的位置。
- 如果你有一个脚本在项目是正常的情况下返回 0，在不正常的情况下返回非 0，你可以使 git bisect 自动化这些操作。通过 bisect start 命令的参数来设定这两个提交，第一个参数是项目不正常的提交，第二个参数是项目正常的提交。

```plain
$ git bisect start HEAD v1.0
$ git bisect run test-error.sh
```

### 子模块

**作用**：允许你将一个Git仓库作为另一个Git仓库(主仓库)的子目录。主仓库通过记录Submodule的URL和commit hash来追踪Submodule。

**使用方法**：子模块在协作上存在比较多的问题，其更新、拉取代码都有差异，不熟悉的话很容易踩坑，不推荐在团队内推广，感兴趣的话看原文吧

### 打包

**作用**：可以在网络中断时，将提交传给合作者

**使用方法**：

- 通过`git bundle create <filename> HEAD master`，创建打包文件，包含HEAD引用和master分支
- 通过`git bundle create <filename> master ^9a466c5`，创建打包文件，包含`9a466c5..master`提交
- 通过`git clone <filename> master`，从打包文件克隆项目master分支，打包文件就像URL一样
- 通过`git fetch <filename> master:mine`，从包中取出 master 分支到我们仓库中的 mine 分支
- 通过`git bundle verify <filename>`验证文件是否可以导入

### 替换

**作用**：允许你在Git仓库中替换现有的提交。当你需要修复错误、更新敏感信息或重写项目的提交历史时，这会特别有用。

**场景**：归档历史提交记录，需要时通过relace还原回来

**使用方法**：

- 通过`git replace <目标提交> <新提交>`实现替换提交
- 通过`git replace HEAD~2 <新提交哈希值1>`替换多个提交
- 通过`git replace 5d6e7f8 --delete`从Git历史中删除一个提交

### 存储凭证

**作用**：存储访问Git服务器的凭证，方便提交

**使用方法**：

- 通过`git config --global credential.helper cache`，将凭证存放在内存中一段时间。 密码永远不会被存储在磁盘中，并且在15分钟后从内存中清除
- 通过`git config --global credential.helper 'store --file ~/.my-credentials'`，“store” 模式可以自定义存放密码的文件路径，明文存储并且永不过期。
- 通过如下命令，主动store配置信息，并get配置信息

```plain
$ git credential-store --file ~/git.store store (1)
protocol=https
host=mygithost
username=bob
password=s3cre7
$ git credential-store --file ~/git.store get (2)
protocol=https
host=mygithost

username=bob (3)
password=s3cre7

$ cat ~/git.store
https://bob:s3cre7@mygithost
```

- 可以自定义脚本，如定义脚本xxxx，通过执行`git credential-xxxx`运行

## 自定义Git

### 配置Git

`git config`的命令

#### 配置作用范围

- `/etc/gitconfig`：系统级别配置，每位用户都生效。通过`git config --system`配置
- `~/.gitconfig`：用户级别配置。通过`git config --global`配置
- `.git/config`：本地仓库级别配置。通过`git config --local`配置

#### 客户端配置

- 用户信息

```plain
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

- 提交编辑器

```plain
$ git config --global core.editor emacs
```

- 提交模版信息

```plain
$ git config --global commit.template ~/.gitmessage.txt
```

- log和diff分页器

```plain
$ git config --global core.pager '' # 空字符串表示不分页，默认用的是 less
```

- GPG 签署密钥

```plain
$ git config --global user.signingkey <gpg-key-id>
```

- 全局忽略文件

```plain
$ git config --global core.excludesfile ~/.gitignore_global
```

- 自动纠正

只要有一个命令被模糊匹配到了，Git 会自动运行该命令

```plain
$ git config --global help.autocorrect 1
```

- 输出着色

```plain
$ git config --global color.ui false # 关掉输出内容着色

# 分别为不同的命令输出配置着色
# color.branch
# color.diff
# color.interactive
# color.status
$ git config --global color.diff.meta "blue black bold"
```

- 合并工具

可以通过图形化工具替换原生命令行工具

```plain
$ git config --global merge.tool extMerge # 使用extMerge脚本合并
$ git config --global mergetool.extMerge.cmd \
  'extMerge "$BASE" "$LOCAL" "$REMOTE" "$MERGED"'
$ git config --global mergetool.extMerge.trustExitCode false
$ git config --global diff.external extDiff # 使用extDiff脚本比较
```

- 格式化和空白符

跨平台协作，Linux和Windows在换行上有差异，Git 提供了一些配置项来帮助你解决这些问题

```plain
$ git config --global core.autocrlf true # 提交时把回车和换行转换成换行，检出代码时把换行转换成回车和换行，在 Windows 系统上开启
$ git config --global core.autocrlf true # 提交时把回车和换行转换成换行，检出时不转换
```

Git 预先设置了一些选项来探测和修正多余空白字符问题

默认被打开的三个选项是：`blank-at-eol`，查找行尾的空格；`blank-at-eof`，盯住文件底部的空行； `space-before-tab`，警惕行头 tab 前面的空格。

默认被关闭的三个选项是：`indent-with-non-tab`，揪出以空格而非 tab 开头的行（你可以用 tabwidth 选项控制它）；`tab-in-indent`，监视在行头表示缩进的 tab；`cr-at-eol`，告诉 Git 忽略行尾的回车。

```plain
# 打开除`space-before-tab` 之外的所有选项
$ git config --global core.whitespace \
    trailing-space,-space-before-tab,indent-with-non-tab,tab-in-indent,cr-at-eol
```

#### 服务器配置

- 禁止非fast-forwards提交

```plain
$ git config --system receive.denyNonFastForwards true
```

- 禁止删除分支

```plain
$ git config --system receive.denyDeletes true
```

- 检查提交对象

确认每个对象的有效性以及 SHA-1 检验和是否保持一致，推送的文件很大的情况下很耗时

```plain
$ git config --system receive.fsckObjects true
```

### Git属性

`.gitattributes`配置

**识别文件格式**

例如：用 Git 属性让 Git 知道哪些是二进制文件，让 Git 把所有 pbxproj 文件当成二进制文件，在 `.gitattributes` 文件中如下设置

```plain
*.pbxproj binary
```

**比较二进制文件**

在比较时，采用指定的工具，如果不指定则Git会以二进制格式处理。

1. 先配置比较工具

```plain
$ git config diff.word.textconv docx2txt # 为word比较工具
$ git config diff.exif.textconv exiftool # 为exif比较工具
```

1. 然后`.gitattributes` 文件中如下设置

```plain
*.docx diff=word # 使用docx2txt工具比较docx文件的内容
*.png diff=exif # 使用exiftool工具比较图片文件的元数据
```

1. 在对docx和png使用git diff时，就会用上述工具比较

**占位符替换**

将提交的相关信息，替换到提交文件中的占位符。比如：在文件中加上提交的时间戳。

编写过滤器来实现文件提交或检出时的关键字替换。 过滤器由“clean”和“smudge”两个子过滤器组成。 在 .gitattributes 文件中，你能对特定的路径设置一个过滤器，然后设置文件checkout前的处理脚本（“smudge”过滤器会在文件被检出时触发）和文件暂存前的处理脚本（“clean”过滤器会在文件被暂存时触发）。

1. 定义“indent”过滤器，指定在 smudge 和 clean 时分别写入时间戳和清理时间戳

```bash
$ git config --global filter.indent.smudge expand_date # expand_date脚本从 git log 中得到最新提交日期，将其注入所有输入文件的 $Date$ 字段
$ git config --global filter.indent.clean 'perl -pe "s/\\\$Date[^\\\$]*\\\$/\\\$Date\\\$/"' # Perl 代码会删除 $Date$ 后面注入的内容，恢复它的原貌
```

1. 在`.gitattributes`配置，对所有date*.txt应用上述过滤器

```bash
date*.txt filter=dater
```

**合并策略**

对项目中的特定文件指定合并策略，如：有数据库文件 database.xml不希望在合并时被弄乱。

1. 定义一个虚拟的合并策略

```bash
$ git config --global merge.ours.driver true
```

1. 在`.gitattributes`配置

```bash
database.xml merge=ours
```

### Git钩子

Git 下 `hooks` 子目录中的脚本

把一个正确命名（不带扩展名）且可执行的文件放入 .git 目录下的 hooks 子目录中，即可激活该钩子脚本。

#### 客户端钩子

克隆某个版本库时，它的客户端钩子 并不 随同复制。 如果需要靠这些脚本来强制维持某种策略，建议你在服务器端实现这一功能。

**提交commit钩子**

1. **pre-commit**：在输入提交信息前运行。一般用于检查即将提交的内容，非零值退出将放弃提交。可以用 `git commit --no-verify` 绕过这个环节。
2. **prepare-commit-msg**：在启动提交信息编辑器之前，默认信息被创建之后运行。接收参数：当前提交信息的文件的路径、提交类型和修补提交的提交的 SHA-1 校验。一般用于结合提交模板来使用它，动态地插入提交信息
3. **commit-msg**：在编辑提交信息后运行。接收参数：提交信息文件路径。一般用于在提交前检查提交信息，非零值退出将放弃提交。
4. **post-commit**：在整个提交过程完成后运行。一般用于通知的事情。

**电子邮件git am钩子**

1. **applypatch-msg**：接收参数：请求合并信息的临时文件的名字。一般用于检查提交信息。
2. **pre-applypatch**：运行于应用补丁之后，产生提交之前。一般用于在提交前检查快照和工作区。
3. **post-applypatch**：运行于提交产生之后。一般用于通知的事情。

**其他钩子**

1. **pre-rebase**：在rebase前运行。一般用于禁止对已经推送的提交变基
2. **post-rewrite**：在替换提交记录的命令调用，跟 post-checkout 和 post-merge 差不多。
3. **post-checkout**：在 git checkout 成功后运行。一般用于调整工作目录或配置等
4. **post-merge**：在 git merge 成功后运行。
5. **pre-push**：在git push开始前运行。一般用于验证。
6. **pre-auto-gc**：在垃圾回收开始之前被调用。一般用于回收通知，或者中断回收。

#### 服务端钩子

**pre-receive**：在处理push前运行 pre-receive。可用于阻止non-fast-forward更新，或者对对文件进行访问控制。

**update**：和 pre-receive 脚本十分类似，不同之处在于它会为每一个准备更新的分支各运行一次。 假如推送者同时向多个分支推送内容，pre-receive 只运行一次，相比之下 update 则会为每一个被推送的分支各运行一次。

**post-receive**：在处理push后运行。可以用来更新其他系统服务或者通知用户。

## Git内部原理

由于 Git 最初是一套面向版本控制系统的工具集，而不是一个完整的、用户友好的版本控制系统， 所以它还包含了一部分用于完成底层工作的子命令。 这些命令被设计成能以 UNIX 命令行的风格连接在一起，抑或藉由脚本调用，来完成工作。 这部分命令一般被称作“底层（plumbing）”命令，而那些更友好的命令则被称作“上层（porcelain）”命令。

.git 目录下的HEAD 文件、index 文件，objects 目录、refs 目录是核心组成部分，Git会通过底层命令管理这4个部分。

- objects 目录存储所有数据内容；
- refs 目录存储指向数据（分支、远程仓库和标签等）的提交对象的指针；
- HEAD 文件指向目前checkout的分支；
- index 文件保存暂存区信息。

其他已经介绍过的还有

- config 文件包含项目特有的配置选项。 
- info 目录包含一个全局性排除（global exclude）文件， 用以放置那些不希望被记录在 .gitignore 的文件
- hooks 目录包含客户端或服务端的钩子脚本

### Git对象

Git 是一个内容寻址文件系统，核心部分是一个简单的键值对数据库。

#### 数据对象

- `git hash-object`：通过底层命令 `git hash-object` 可将任意数据保存于 .git/objects 目录（即 对象数据库），并返回指向该数据对象的唯一的键.

```bash
$ echo 'test content' | git hash-object -w --stdin
d670460b4b4aece5915caf5c68d12f560a9fe3e4 # 前两个字符用于命名子目录，余下的 38 个字符则用作文件名
或 直接将文件写入
$ git hash-object -w test.txt
d670460b4b4aece5915caf5c68d12f560a9fe3e4

$ find .git/objects -type f
.git/objects/d6/70460b4b4aece5915caf5c68d12f560a9fe3e4
```

- `git cat-file`: 通过 `cat-file` 命令从 Git 那里取回数据，`-p` 选项可指示该命令自动判断内容的类型

```plain
$ git cat-file -p d670460b4b4aece5915caf5c68d12f560a9fe3e4
test content
```

- `-t `命令，可以让 Git 告诉我们其内部存储的任何对象类型

```plain
$ git cat-file -t 1f7a7a472abf3dd9643fd615f6da379c4acb3e3a
blob
```

上述类型的对象我们称之为 **数据对象**（blob object）

#### 树对象

树对象（tree object），记录文件名，并可以将多个文件组织到一起形成树。

每条记录含有一个指向数据对象或者子树对象的 SHA-1 指针，以及相应的模式、类型、文件名信息。

```plain
# 查看树对象，master^{tree} 语法表示 master 分支上最新的提交所指向的树对象。
$ git cat-file -p master^{tree}
100644 blob a906cb2a4a904a152e80877d4088654daad0c859      README
100644 blob 8f94139338f9404f26296befa88755fc2598c289      Rakefile
040000 tree 99f1a6d12cb4b6f19c8655fca46c3ecf317074e0      lib

# 文件模式为 100644，表明这是一个普通文件；100755，表示一个可执行文件；120000，表示一个符号链接
# 上述三种模式即是 Git 文件（即数据对象）的所有合法模式，还有其他一些模式，但用于目录项和子模块。
```

![树对象模型(简化)](/Users/zhangchenxue/CodeProject/njzcx/ChenXueBlog/开发工具/版本控制/images/ProGit/1741922872335-cbd61177-d335-4573-b70e-ced3d6a74636.png)

Git 根据Index区创建并记录一个对应的树对象，所以需要通过`git update-index`暂存一些文件来创建一个暂存区。 

- 必须指定 --add 选项，如果此前该文件并不在暂存区中。
- 必需指定 --cacheinfo 选项，如果将要添加的文件位于 Git 数据库中，而不是位于当前目录下。

通过 `git write-tree` 命令将Index区内容写入一个树对象。 

```bash
$ git update-index --add --cacheinfo 100644 \
  1f7a7a472abf3dd9643fd615f6da379c4acb3e3a test.txt
$ git update-index --add new.txt

$ git write-tree
0155eb4229851634a0f03eb265b69f5a2d56f341
$ git cat-file -p 0155eb4229851634a0f03eb265b69f5a2d56f341
100644 blob fa49b077972391ad58037050f2a75f74e3671e92      new.txt
100644 blob 1f7a7a472abf3dd9643fd615f6da379c4acb3e3a      test.txt
```

通过 `git read-tree` 命令，可以把树对象读回Index区。 

- 通过对该命令指定 --prefix 选项，将一个已有的树对象作为子树读入暂存区

```bash
$ git read-tree --prefix=bak d8329fc1cc938780ffdd9f94e0d364e0ea74f579 # 已经存在于Git数据库中的树对象
$ git write-tree
3c4e9cd789d88d8d89c1073707c3585e41b0e614
$ git cat-file -p 3c4e9cd789d88d8d89c1073707c3585e41b0e614
040000 tree d8329fc1cc938780ffdd9f94e0d364e0ea74f579      bak
100644 blob fa49b077972391ad58037050f2a75f74e3671e92      new.txt
100644 blob 1f7a7a472abf3dd9643fd615f6da379c4acb3e3a      test.txt
```

#### 提交对象

提交对象，记录提交人、提交时间、提交快照(树对象)

通过调用 `commit-tree <树对象>` 命令创建一个提交对象，指定一个树对象的 SHA-1 值，以及该提交的父提交对象（如果有的话）

```bash
$ echo 'first commit' | git commit-tree d8329f
fdf4fc3344e67ab068f836878b6c4951e3b15f3d

# cat-file 提交对象返回格式
# 先指定一个顶层树对象，代表当前项目快照；
# 然后是可能存在的父提交（前面描述的提交对象并不存在任何父提交）；
# 之后是作者/提交者信息（依据你的 user.name 和 user.email 配置来设定，外加一个时间戳）；
# 留空一行，最后是提交注释。
$ git cat-file -p fdf4fc3
tree d8329fc1cc938780ffdd9f94e0d364e0ea74f579
author Scott Chacon <schacon@gmail.com> 1243040974 -0700
committer Scott Chacon <schacon@gmail.com> 1243040974 -0700

first commit
```

通过`commit-tree <树对象> -p <父提交>`，分别引用各自的上一个提交，可得到货真价实的`git log`提交历史

```bash
$ echo 'second commit' | git commit-tree 0155eb -p fdf4fc3
cac0cab538b970a37ea1e769cbbde608743bc96d
$ echo 'third commit'  | git commit-tree 3c4e9c -p cac0cab
1a410efbd13591db07496601ebc7a059dd55cfe9
```

#### 对象存储

**Git存储对象的逻辑**

1. 构建header信息：识别出的对象的类型作为开头来构造一个头部信息，本例中是一个“blob”字符串。 接着 Git 会在头部的第一部分添加一个空格，随后是数据内容的字节数，最后是一个空字节。

1. `header = "blob 16\u0000"`

1. 将上述头部信息和原始数据拼接起来，并计算出这条新内容的 SHA-1 校验和。

1. `sha1 = SHA1.hexdigest(header + content)`

1. Git 会通过 zlib 压缩对象内容。

1. `zlib_content = Zlib.deflate(store)`

1. 将 zlib 压缩的内容写入磁盘，写入路径为 SHA-1 值的前两个字符作为子目录名称，后 38 个字符则作为子目录内文件的名称。

#### 总结

上述通过底层操作便完成了一个 Git 提交历史的创建。 这就是每次我们运行 `git add` 和 `git commit` 命令时，Git 所做的工作实质就是将被改写的文件保存为数据对象， 更新暂存区，记录树对象，最后创建一个指明了顶层树对象和父提交的提交对象。 这三种主要的 Git 对象——数据对象、树对象、提交对象——均以单独文件的形式保存在 .git/objects 目录下。 

```bash
$ find .git/objects -type f
.git/objects/01/55eb4229851634a0f03eb265b69f5a2d56f341 # tree 2
.git/objects/1a/410efbd13591db07496601ebc7a059dd55cfe9 # commit 3
.git/objects/1f/7a7a472abf3dd9643fd615f6da379c4acb3e3a # test.txt v2
....
```

### Git引用

引用是用一个简单名字的文件来保存 提交记录 SHA-1 值，从而代替SHA1简化记忆。该文件保存在.git/refs 目录下。

```plain
$ find .git/refs
.git/refs
.git/refs/heads # 分支引用
.git/refs/tags # 标签引用
.git/refs/remotes # 远程引用
```

#### 分支引用

分支引用，顾名思义，就是master、develop等以分支名称，指向提交记录的引用。

手动更新引用文件，有如下2个方式

```bash
# 不提倡直接编辑引用文件
$ echo 1a410efbd13591db07496601ebc7a059dd55cfe9 > .git/refs/heads/master

# 更新某个引用，Git 提供了一个更加安全的命令 update-ref 来完成此事
$ git update-ref refs/heads/master 1a410efbd13591db07496601ebc7a059dd55cfe9
```

运行类似于 `git branch <branch>` 这样的命令时，Git 实际上会运行 `update-ref` 命令， 取得所在分支最新提交 SHA-1 值，更新在引用文件中。

#### HEAD引用

HEAD引用是单独的一个文件，位于`.git/HEAD`，通常是一个符号引用，指向目前所在的分支。 所谓符号引用，表示它是一个指向其他引用的指针。

```bash
$ cat .git/HEAD
ref: refs/heads/master
```

手动更新引用文件，有如下2个方式

```bash
# 方式一，不提倡
echo "ref: refs/heads/test" > .git/HEAD
# 方式二
$ git symbolic-ref HEAD refs/heads/test
```

运行类似 `git commit`、`git checkout`，Git 会更新 HEAD 文件。

#### 标签引用

标签引用位于`.git/refs/tags`

- 轻量标签：可以像这样创建一个轻量标签，指向一个提交对象

```bash
$ git update-ref refs/tags/v1.0 cac0cab538b970a37ea1e769cbbde608743bc96d
```

- 附注标签：要创建一个附注标签，Git 会创建一个标签对象，并记录一个引用来指向该标签对象，而不是直接指向提交对象。

**标签对象**：它包含一个标签创建者信息、日期、注释信息，以及一个的指针，该指针通常指向提交对象，当然也可以指向树对象、数据对象等任意类型。它像是一个永不移动的分支引用。

下面是附注标签文件的内容

```bash
$ cat .git/refs/tags/v1.1
9585191f37f7b0fb9444f35a9bf50de191beadc2  # 标签对象sha1

$ git cat-file -p 9585191f37f7b0fb9444f35a9bf50de191beadc2 # 标签对象sha1
object 1a410efbd13591db07496601ebc7a059dd55cfe9 # 提交对象sha1
type commit
tag v1.1
tagger Scott Chacon <schacon@gmail.com> Sat May 23 16:48:58 2009 -0700

test tag
```

#### 远程引用

Git 会记录下最近一次push/fetch操作时，每一个远程分支所对应的提交记录。保存在 `refs/remotes` 目录下。

```bash
# 添加一个叫做 origin 的远程版本库，然后把 master 分支推送上去
$ git remote add origin git@github.com:schacon/simplegit-progit.git
$ git push origin master
a11bef0..ca82a6d  master -> master

# 查看 refs/remotes/origin/master 文件，可以发现远程引用所对应的SHA1值，就是最近一次与服务器通信时本地分支SHA1值
$ cat .git/refs/remotes/origin/master
ca82a6dff817ec66f44342007202690a93763949
```

远程引用和分支引用之间最主要的区别在于，远程引用是只读的。 虽然可以 git checkout 到某个远程引用，但是 Git 并不会将 HEAD 引用指向该远程引用。因此，你永远不能通过 commit 命令来更新远程引用。 Git 将这些远程引用作为记录远程服务器上各分支最后已知位置状态的书签来管理。

### 包文件

Git 会时不时地将多个这些对象打包成一个称为“包文件（packfile）”的二进制文件，以节省空间和提高效率。 

- 打包原理：Git 打包对象时，会查找命名及大小相近的文件，并只保存文件不同版本之间的差异内容。但最新的版本完整保存了文件内容，而旧版本反而是以差异方式保存的——这是因为大部分情况下需要快速访问文件的最新版本。
- 打包时机：①当版本库中有太多的松散对象；②或者你手动执行 `git gc` 命令；③或者你向远程服务器执行推送时Git 要看到打包过程，你可以手动执行 `git gc` 命令让 Git 对对象进行打包。

打包前

```bash
$ find .git/objects -type f
.git/objects/01/55eb4229851634a0f03eb265b69f5a2d56f341 # tree 2
.git/objects/1a/410efbd13591db07496601ebc7a059dd55cfe9 # commit 3
.git/objects/1f/7a7a472abf3dd9643fd615f6da379c4acb3e3a # test.txt v2
.git/objects/3c/4e9cd789d88d8d89c1073707c3585e41b0e614 # tree 3
.git/objects/83/baae61804e65cc73a7201a7252750c76066a30 # test.txt v1
.git/objects/95/85191f37f7b0fb9444f35a9bf50de191beadc2 # tag
.git/objects/ca/c0cab538b970a37ea1e769cbbde608743bc96d # commit 2
.git/objects/d6/70460b4b4aece5915caf5c68d12f560a9fe3e4 # 'test content'
.git/objects/d8/329fc1cc938780ffdd9f94e0d364e0ea74f579 # tree 1
.git/objects/fa/49b077972391ad58037050f2a75f74e3671e92 # new.txt
.git/objects/fd/f4fc3344e67ab068f836878b6c4951e3b15f3d # commit 1
```

打包后

```bash
$ find .git/objects -type f
.git/objects/bd/9dbf5aae1a3862dd1526723246b20206e5fc37 # 未提交
.git/objects/d6/70460b4b4aece5915caf5c68d12f560a9fe3e4 # 未提交
.git/objects/info/packs
.git/objects/pack/pack-978e03944f5c581011e6998cd0e9e30000905586.idx
.git/objects/pack/pack-978e03944f5c581011e6998cd0e9e30000905586.pack
```

新创建了包文件pack和一个索引idx。 包文件包含了刚才从文件系统中移除的所有对象的内容。 索引文件包含了包文件的偏移信息，我们通过索引文件就可以快速定位任意一个指定对象。

git verify-pack 这个底层命令可以让你查看已打包的内容。`033b4`和`b042a`是同一个文件，`b042a`是最新版本。

```bash
$ git verify-pack -v .git/objects/pack/pack-978e03944f5c581011e6998cd0e9e30000905586.idx
... # 省略了一些输出
2431da676938450a4d72e260db3bf7b0f587bbc1 commit 223 155 12
702470739ce72005e2edff522fde85d52a65df9b commit 165 118 756
d368d0ac0678cbe6cce505be58126d3526706e54 tag    130 122 874
deef2e1b793907545e50a2ea2ddb5ba6c58c4506 tree   136 136 1178
d982c7cb2c2a972ee391a85da481fc1f9127a01d tree   6 17 1314 1 \
  deef2e1b793907545e50a2ea2ddb5ba6c58c4506
b042a60ef7dff760008df33cee372b945b6e884e blob   22054 5799 1463
033b4468fa6b2a9547a70d88d1bbe8bf3f9ed0d5 blob   9 20 7262 1 \
  b042a60ef7dff760008df33cee372b945b6e884e
1f7a7a472abf3dd9643fd615f6da379c4acb3e3a blob   10 19 7282
non delta: 15 objects
chain length = 1: 3 objects
.git/objects/pack/pack-978e03944f5c581011e6998cd0e9e30000905586.pack: ok
```

### 引用规范

**引用规范**：是仓库中的 `.git/config` 文件中的配置， 指定远程仓库的名称（origin）、URL 和一个用于获取操作的 **引用规范**（refspec），由一个可选的 + 号和紧随其后的 <src>:<dst> 组成。  <src> 代表远程版本库中的引用； <dst> 是本地跟踪的远程引用的位置。 + 号告诉 Git 即使在不能快进的情况下也要（强制）更新引用。

引用规范分为：fetch获取引用、push推送引用

```plain
[remote "origin"]
	url = https://github.com/schacon/simplegit-progit
	fetch = +refs/heads/*:refs/remotes/origin/*
  push = refs/heads/master:refs/heads/qa/master
```

默认情况下，引用规范由 git remote add origin 命令自动生成。Git 获取服务器中 refs/heads/ 下面的所有引用，并将它写入到本地的 refs/remotes/origin/ 中。

如果想让 Git 每次只拉取远程的 master 分支，而不是所有分支， 可以把（引用规范的）获取那一行修改为只引用该分支。这仅是针对该远程版本库的 git fetch 操作的默认引用规范。

```plain
fetch = +refs/heads/master:refs/remotes/origin/master
```

上述配置是持久化的，如果临时fetch或push，可以直接在fetch或push命令中指定引用规范

```bash
# 将远程的 master 分支拉到本地的 origin/mymaster 分支
$ git fetch origin master:refs/remotes/origin/mymaster

# master 分支推送到远程服务器的 qa/master 分支上
$ git push origin master:refs/heads/qa/master
```

### 传输协议

本节展示了Git客户端和服务端之间的基本交互过程和命令，实际使用并不涉及，简单讲下。

Git 通过两种主要的方式在版本库之间传输数据：“哑（dumb）”协议和“智能（smart）”协议。

- 哑（dumb）：在Git传输过程中，抓取过程是一系列客户端向服务端发起 HTTP 的 GET 请求，这种情况下，服务端不需要有针对 Git 特有的代码，客户端自行推断出服务端 Git 仓库的布局。很简单但效率略低，它不能从客户端向服务端发送数据。
- 智能（smart）：服务端运行一个进程，而这和哑协议的差别——它可以读取本地数据，理解客户端有什么和需要什么，并为它生成合适的包文件。 总共有两组进程用于传输数据，它们分别负责上传（send-pack、receive-pack）和下载数据（fetch-pack、upload-pack）。

### 维护与数据恢复

- **维护**： `git gc`命令会做以下事情，收集所有松散对象并将它们放置到包文件中， 将多个包文件合并为一个大的包文件，移除与任何提交都不相关的陈旧对象。 

- 大约需要 7000 个以上的松散对象或超过 50 个的包文件才能让 Git 启动一次真正的 gc 命令。 你可以通过修改 gc.auto 与 gc.autopacklimit 的设置来改动这些数值。
- gc 将会做的另一件事是打包你的引用到一个单独的文件。refs 目录中的引用文件，会被移动到名为 .git/packed-refs 的文件中

- **数据恢复**：

- 向前恢复：通过`git reset --hard 1a410e`，硬重置到之前的某次提交。
- 向后恢复：通过`git reflog`，查找 HEAD 曾经变更的历史记录；或通过 `git log -g`，输出HEAD引用日志。得到向后的提交SHA1后，通过`git branch <newbranc> <sha1>`创建新分支指向该提交。

- **移除对象**：移除历史提交中的某个大对象，此操作会对历史提交变基

- 通过`git count-objects -v`查看占用空间大小
- 通过`git gc` + `git verify-pack .git/objects/pack/pack-29…69.idx` 命令， 对输出内容的第三列（即文件大小）进行排序，从而找出这个大文件的提交。 
- 使用 `git rev-list --objects --all | grep 82c99a3` 命令找出大文件提交中，具体是哪个文件
- 通过`git log --oneline --branches -- git.tgz`查看这个文件的改动影响哪些提交
- 使用 `filter-branch` 命令，从 Git 该文件最早的提交到当前提交中完全移除这个文件

```plain
git filter-branch --index-filter \
  'git rm --ignore-unmatch --cached git.tgz' -- 7b30847^..
```

### 环境变量

本节包含了可以影响Git行为的主要环境变量，这些环境变量作用与git config有重合，实际采用默认值即可。

- 全局行为：设置git路径、设置分页、设置编辑器
- 版本库位置：设置.git目录、设置index路径、设置object路径
- 路径规则：设置通配符启用、设置大小写忽略
- 提交：设置作者信息、设置提交者信息
- 网络：设置https验证、设置限流
- 比较和合并：设置diff行数、设置diff程序、设置默认合并策略
- 调试：设置调试开关

## 总结

本书需要重点掌握Git的使用和原理；

Git的使用上主要涉及：

- 初始化类：init、clone、config
- 常用功能：add、commit、push、fetch、merge、rebase、reset、checkout、clean
- 辅助功能：remote、branch、tag、cherry-pick、revert、stash、rm、mv
- 查看类：log、show、grep、diff、blame
- 底层命令：filter-branch、rev-parse、reflog

Git的原理上，主要理解：

- 三个区域：HEAD、Index、工作目录
- 分支模型：blob对象 -> 树对象 -> 提交对象 -> 分支指针
- 存储模型：HEAD文件、Index文件、object目录、refs目录
- git原理：写入对象、更新Index、写入树对象、写入提交对象；对象文件格式
- 合并模式：fast-forward、三方合并
- checkout与reset区别：reset soft、mixed、hard分别操作三个区域；checkout近似于reset --hard，区别在于reset移动HEAD分支的指向
# 场景
有时候我们在多分支并行开发时，经常碰到开发分支的bugfix需要合入之前的版本或其他分支中。有时候你需要这样做，只合并你需要的那些commits，不需要的commits就不合并进去了。

# 处理办法
## 1、合并某个分支上的单个commit
首先，用git log查看一下你想要进行合并commit
```shell
commit fd1f9e290fb199ba8eada61a8aa45c347b2a14b8 [master]

commit 2a92a024e6deb1b05430f5044e7fde722e06e9c1 [feature]
```
比如，feature 分支上的commit 2a92a024非常重要，它含有一个bug的修改。无论什么原因，你现在只需要将2a92a024合并到master，而不合并feature上的其他commits，所以我们用git cherry-pick命令来做：
```shell
git checkout master
git cherry-pick 2a92a024e6deb1b05430f5044e7fde722e06e9c1
```
现在2a92a024就被合并到master分支，并在master中添加了commit（作为一个新的commit）。cherry-pick 和merge比较类似，如果git不能合并代码改动（比如遇到合并冲突），git需要你自己来解决冲突并手动添加commit。
## 2、合并某个分支上的一系列commits
在一些特性情况下，合并单个commit并不够，你需要合并一系列相连的commits。这种情况下就不要选择cherry-pick了，rebase 更适合。还以上例为例，假设你需要合并feature分支的commit 76cada12 ~ 2a92a024到master分支。

首先需要基于feature创建一个新的分支，并指明新分支的最后一个commit：
```shell
git checkout -b newbranch 2a92a024
```
然后，rebase这个新分支的commit到master（--onto master）。76cada12^ 指明你想从哪个特定的commit开始。
```shell
git rebase --onto master 76cada12^
```
得到的结果就是feature分支的commit 76cada12 ~ 2a92a024都被合并到了master分支。

[参考链接](https://www.devroom.io/2010/06/10/cherry-picking-specific-commits-from-another-branch/)
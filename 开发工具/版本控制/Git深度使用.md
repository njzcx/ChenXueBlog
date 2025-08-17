### 历程&重要Feature

由Linus在2005年创建，截止2022年共2000个贡献者

#### 重要Feature

1. **条件包含（IncludeIf）**

1. 为不同仓库提供不同的默认配置，eg：不同的项目仓库使用不同的配置，如公司和自己的项目做邮箱区分

```git
git config --global "includeIf.gitdir:~/repos-job/.path" job.inc
git config --global "includeIf.gitdir:~/repos-my/.path" my.inc
```

1. **大仓库克隆（解决大仓库的性能瓶颈）**

1. 稀疏检出：按需检出需要的文件

```git
git clone --sparse ...
git sparse-checkout <init|add|reapply> ...
```

1. 协议2.0：client和server之间新的协议，针对引用或分支多的仓库，消除了引用广播，提升了性能
2. 部分克隆：只克隆元数据。可需获取文件内容。

```git
git clone --filter=blob:none ...
```

1. **多工作区（worktree）**

1. 对同一个仓库(.git)，一套元数据，不同分支checkout到不同工作区。

```git
git worktree add <工作区目录地址> <branch> # 创建工作区
git worktree list # 列出工作区和分支
git worktree remove <工作区目录地址> # 删除工作区
git worktree purne # 工作区删除后，清理残存数据
```

1. **使用 watchman 提升工作区扫描效率**

1. 解决当仓库中包含数以万计的文件时，git status速度变慢

```git
# 安装watchman脚本
mv .git/hooks/fsmonitor-watchman.sample .git/hooks/fsmonitor-watchman
# 指定脚本
git config core.fsmonitor .git/hooks/fsmonitor-watchman
```

1. **checkout 命令拆分为两条新命令：switch 和restore**，降低复杂度的 checkout 命令的学习成本；

1. switch提供分支切换操作；restore提供检出操作

```git
# 创建并切换到新分支
git switch -c <new-branch>
# 将文件在暂存区中的版本检出到工作区
git restore --file
# 将文件在指定提交中的版本检出到工作区
git restore -s <version> -- file
```

1. **git clean --interactive**：采用交互式操作，**清理本地未跟踪文件**
2. 用git commit --fixup/--squash 以及 git rebase -i --autosquash 修改历史提交
3. git range-diff 比较两个版本序列之间的差异
4. 使用 proc-receive 挂钩和 report-statsu-v2 设计免特性分支的评审工作流（主干开发模式）
5. Git本地化
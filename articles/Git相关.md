# 追加提交

很常见的一个问题, 信心满满地commit完后发现又有些小瑕疵改动, 然后又要提交

可以用

```bash
git commit --amend
```

来追加提交, 不会增加commit记录

# git 配置文件

git 配置文件有三个位置

1. /etc/gitconfig 系统级(注意前缀没有".")
2. ~/.gitconfig 用户级
3. .git/config 文件夹级

后者覆盖前者

[参考](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%88%9D%E6%AC%A1%E8%BF%90%E8%A1%8C-Git-%E5%89%8D%E7%9A%84%E9%85%8D%E7%BD%AE)

# merge 操作

顺序上, merge 会把其他分支合并到当前所在的分支, 所以一般都是先`git checkout master`然后再git merge xxx`

## 放弃解决冲突

`git merge --abort`

# 分支的切换

传统: `git checkout <branch>`, 不存在则创建后切换`git checkout -b <branch>`

新的: `git switch <branch>`, 不存在则创建后切换`git checkout -c <branch>`

由于` git checkout`身兼多职, 就拆分成了`git switch`和 `git restore`

# 贮藏修改

`git stash`, 将改动全部保存到一个位置后, 清空工作目录所有改动

`git stash list`查看贮藏

`stash`可以多次使用

`git stash apply`还原最近一次贮藏, 贮藏任然存在, `git stash pop还原最近一次贮藏, 贮藏删除

# fetch 和 pull

"pull"在"fetch"的基础上有一个"merge"的操作

# push

`git push <remote> <branch> -f`选项"-f"是以本地为准覆盖掉远端代码, 有一定危险性

一般都要先"pull"一下, 再"push"

# git忽略文件

在多人项目中有一些个人专有文件(比如特定编辑器的配置文件)需要被排除, 为了保持项目的通用与干净, 一般不推荐添加在`.gitignore`中.

取而代之的是添加在当前目录的`.git/info/exclude`内, 按照文档(`git help ignore`)描述:

> Patterns which are specific to a particular repository but which do
>
> not need to be shared with other related repositories (e.g.,
>
> auxiliary files that live inside the repository but are specific to
>
> one user's workflow) should go into the **$GIT_DIR/info/exclude** file.

# git 回退

把退回到某个 commit, 改变的文件都放到暂存区

`git reset --soft <commit>`

# git 变基

形象的比喻: 把一根树杈折下来接到另一个位置.

所有会有冲突的可能

一般的好处是保持历史记录比较干净, 区别于 merge, 后者会在历史记录里留下非常多的交织, 视觉上比较混乱. 而 rebase 的表现为只有一根线, 每个人占据其中一段, 而实际是并发的.

# 推送到仓库时添加私钥

`ssh-add 私钥路径`, 有密码需要输密码

## 常用操作

### 提交 mr

`git commit --amend`

`git push -u origin [分支名]`//第一次推送时, 远端没有这个分支, 使用"-u"设置

`git push origin [分支名] -f` //不安全

### 同步本地 master

`git pull remoteRepo master`

*一般都是 fast forward*

### master拉取最新代码

`git pull dejavuWeb master`
(fast-forward)

### 回退到某个 commit(会改变工作区文件!)

`git log # 展示已有的 commit`
`git reset --hard [commitID] # 回退到某个 commit 并将工作区还原到该 commit 状态`

### 取消刚刚的 commit

`git reset --soft HEAD^`

### 取消解决冲突

`git merge --abort`

### 更上最新进度

切到 master, 拉取最新代码, 切回特性分支

`git rebase master`

## 查看最近一次提交的更改变化

`git show`

# 名词术语

working tree == working directory, 就是工作目录, working tree 是以前的叫法, 参考源码里的[称呼变更](https://github.com/git/git/commit/89aef71d0eb5b5e06216c2efbba76cffe17679f7)

index 就是 staging area, 暂存区

shallow repository, 具有不完整 commits 的仓库, 比如在 git clone 时使用了 --depth, 节省了带宽提高了性能

# 一些正反操作组

git add [file] <=> git reset [file]


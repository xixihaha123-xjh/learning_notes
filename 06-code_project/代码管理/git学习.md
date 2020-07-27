# 1 是什么？

（1）是什么？ 一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统

（2） 特点

1. 直接记录快照，而非差异比较
2. 近乎所有操作都是在本地执行
3. git 保证完整性

4. git 一般只添加数据

## 1.1 版本管理的演变

（1）vcs出现前

1. 用目录拷贝区别不同版本
2. 公共文件容易被覆盖
3. 成员沟通成本很高，代码集成效率低下

（2）本地版本控制系统 RVS

1. 工作原理是在硬盘上保存补丁集（补丁是指文件修订前后的变化），通过应用所有的补丁，可以重新计算出各个版本的文件内容

（3）集中式 vcs：历史版本的搜索能力，不同版本的比较能力

1. 有集中式的版本管理服务器、具备文件版本管理和分支管理能力，集成效率有明显地提高，客户端必须时刻和服务器相连
2. 缺点：中心数据库所在磁盘发生损坏，将丢失所有数据----包括项目的整个变更历史，只剩下人们在各自机器上保留的单独快照
3. 代表：cvs、svn

（3）分布式VCS

1. 分布式vcs - 服务端和客户端都有完整的版本库，脱离服务端，客户端照样可以管理版本，查看历史和版本比较等多数操作，都不需要访问服务器，比集中式vcs更能提高版本管理效率
2. 代表：git --> github（全球最大的开源社区 -- 代码管理，代码评审，项目管理 ） --> gitlab（阿里云平台、携程、去哪儿

3. git的特点：最优的存储能力、非凡的性能、开源的、很容易做备份、支持离线操作、很容易定制工作流程

------

## 1.2 svn与git比较

（1）**git 是分布式的，svn不是，这是git和其它非分布式的版本控制系统，例如svn，cvs等，最核心的区别**。因为git是分布式的，所以Git支持离线工作，在本地可以进行很多操作，包括接下来将要重磅推出的分支功能。而 SVN 必须联网才能正常工作。

集中式与分布式版本控制系统的区别？ 单一的集中管理的服务器，保存所有文件的修订版本 。分布式版本控制系统： 客户端并不只提取最新版本的文件快照， 而是把代码仓库完整地镜像下来，包括完整的历史记录 

（2）集中式版本控制系统： 存储每个文件与初始版本的差异 ，git存储项目随时间改变的快照。

（2）**Git复杂概念多，SVN简单易上手**。 所有同时掌握 Git 和 SVN 的开发者都必须承认，Git 的命令实在太多了，日常工作需要掌握`add`,`commit`,`status`,`fetch`,`push`,`rebase`等，若要熟练掌握，还必须掌握`rebase`和`merge`的区别，`fetch`和`pull`的区别等，除此之外，还有`cherry-pick`，`submodule`，`stash`等功能，仅是这些名词听着都很绕。在易用性这方面，SVN 会好得多，简单易上手，对新手很友好。但是从另外一方面看，Git 命令多意味着功能多，若我们能掌握大部分 Git 的功能，体会到其中的奥妙，会发现再也回不去 SVN 的时代了。

（3）**Git分支廉价，SVN分支昂贵**。团队开发过程中，常常存在创建分支，切换分支的需求。Git分支上指针指向某次提交，而SVN分支是拷贝的目录。 这个特性使 Git 的分支切换非常迅速，且创建成本非常低。 Git 有本地分支，SVN 无本地分支 

## 1.3 git工作流程

（1）git提交代码

1. git add 从工作区提交到暂存区
2. git commit 从暂存区提交到本地仓库
3. git push从本地仓库提交到远程仓库

![git工作流程](pic\5-git工作流程.jpg)

（2）git工作流程

1. 克隆Git资源作为工作目录
2. 在克隆的资源上添加或修改文件
3. 如果其他人修改了，你可以更新资源
4. 在提交前查看修改
5. 提交修改
6. 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交

![6-git工作流程](pic\6-git工作流程.png)

## 1.4 核心概念

（1）工作区 (Workspace)是电脑中实际的目录。 

（2）暂存区 (Index)类似于缓存区域，临时保存你的改动。英文叫stage, 或index。**一般存放在 ".git目录下" 下的index文件（.git/index）中**，所以我们把暂存区有时也叫作索引（index） 

（3）仓库区 (Repository)，分为本地仓库和远程仓库。**工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。** 

![git核心概念](pic\4-git核心概念.jpg)

# 2 Git探秘

## 2.1 探秘 .git 目录

（1）.git 目录

1. HEAD：编辑此文件和切换分支作用相同  `git checkout 分支名`
2. config：配置信息
3. refs：标签及分支的信息
4. objects：git对象类型 commit、blob、tree只要任何文件的文件内容相同，在git的眼里它就是唯一 一个 blob

![探秘.git目录](pic\1-探秘.git目录.png)

## 2.2 commit、tree和blbo三个对象之间的关系

（1）git对象彼此关系

1. 一次commit对应一棵树，这棵树代表了什么呢？这个commit对应着一个视图，这个视图存放着快照，这个快照里放着当前commit对应的本项目仓库中所有的文件夹以及文件的一个快照，也就是说哪个时间点，你这个文件夹长什么样，文件是什么样。就是通过这颗树呈现出来了。
2. 912fa6.... 这棵树中，又存放着tree 96b67e....，指的是images这个文件夹，blob指的是某个具体的文件。blob与文件名没有任何关系，只要文件的内容相同，在git的眼里就是唯一的一个blob，这样的好处就是可以节约存储空间。
3. tree 96b67e....这棵树，也就是这个文件夹下，存放的是 git-logo.png 这个文件。

![git对象](pic\2-git对象.png)



```shell
F:\src\101-GitRunner\git_learning>git log                     # git log查看历史版本信息
commit acc301cd33f0bd0100fe49238ae1a0ede30a7abd (HEAD -> version_1)
Author: xixihaha123-xjh <123456789@123.com>
Date:   Sat Jul 11 23:17:15 2020 +0800

F:\src\101-GitRunner\git_learning>git cat-file -p acc301cd33f0bd0100fe
tree a6a3b1d50f645118d9d893bc877e86c9d9547ba2                  # commit包含了一棵树
parent 13714a964ee76373b3d96ae5a48d7ad86059d231
author xixihaha123-xjh <123456789@123.com> 1594480635 +0800
committer xixihaha123-xjh <123456789@123.com> 1594480635 +0800

add style and script                                           # commit message

F:\src\101-GitRunner\git_learning>git cat-file -p a6a3b1d50f645118d9 # commit对应的树信息
100644 blob 01f27d5b4aed227d6f50f268a65285ca1874b918    readme.md
100644 blob e69de29bb2d1d6434b8b29ae775ad8c2e48c5391    script.js
100644 blob e69de29bb2d1d6434b8b29ae775ad8c2e48c5391    style.css

F:\src\101-GitRunner\git_learning>git cat-file -p 01f27d5b4aed227d6  # readme.md 文件的内容
------------------------- test branch ---------------------------
```

## 2.3 数一数tree的个数

（1）新建的Git仓库，有且仅有1个commit，仅仅包含 /doc/readme，请问内含多少个tree，多好个blob？

![1594562259314](F:\00-C++后台服务器学习\06-代码工程\代码管理\pic\3-git对象的关系.png)

1. commit提交，参数一个commit对象
2. commit 对象对应一棵树 --- 本项目仓库中所有的文件夹以及文件的一个快照
3. 文件夹 /doc/  对应的tree --- 存放文件 readme
4. readme对应一个blob，存放readme文件信息

## 2.4 分离头指针情况下的注意事项

（1）You are in 'detached HEAD' state. 分离头指针，其本质是我们正工作在一个没有分支的状态下，切换分支时，你的变更信息可能会被 git 当作垃圾清除掉。

（2）如果做变更要与某个分支挂钩，在这个分支下，对分支做变更，这样commit，git才不会将变更清除

（3）分离头指针带来的好处，如果你想做一些变更，但是这个变更你只是尝试性的变更，没准做下来你发现效果不好，你完全可以随时把它扔掉。如果切换分支后，想要保存分离头指针下的变更信息，你可以创建一个新的分支保存变更信息  `git branch <new-branch-name> xxx `

```shell
F:\src\101-GitRunner\git_learning>git checkout acc301cd33f0bd0
Note: switching to 'acc301cd33f0bd0'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.
```

## 2.5 进一步理解HEAD和branch

（1）HEAD指向一个分支，也可以指向具体的某一个commit --- 分离头指针状态。做分支切换时，HEAD也是跟着变换的。HEAD最终还是落脚在某一个commit。

```shell
# git删除分支
F:\src\101-GitRunner\git_learning>git branch -d fix_readme
Deleted branch fix_readme (was acc301c).
# 创建基于 master的分支fix_readme并切换到新的分支上
F:\src\101-GitRunner\git_learning>git checkout -b fix_readme master
Previous HEAD position was acc301c add style and script
Switched to a new branch 'fix_readme'
```

（2）比较两次commit的差异

```shell
F:\src\101-GitRunner\git_learning>git diff 8b772df6be9d1f6  563327ab37a
diff --git a/changme.md b/readme.md

F:\src\101-GitRunner\git_learning>git diff HEAD HEAD~1
```





# 3 常用命令

## 3.1 初始化

（1）git 安装参考网站------- https://git-scm.com/book/zh/v2/起步-安装-Git

（2）创建git仓库

1. 把已有的项目代码纳入Git管理： 在执行完成 **git init** 命令后，Git 仓库会生成一个 .git 目录，该目录包含了资源的所有元数据，其他的项目目录保持不变（不像 SVN 会在每个子目录生成 .svn 目录，Git 只在仓库的根目录生成 .git 目录）。 

```shell
$ cd 项目代码所在的文件夹
$ git init
```

2. 新建的项目直接用Git管理

```shell
$ cd 项目代码所在的文件夹
$ git init project 
# project目录下会出现一个名为 .git 的目录，所有Git需要的数据和资源都存放在这个目录中
cd project
```

```shell
# 初始化仓库
$ git init git_learning
# Initialized empty Git repository in F:/src/101-GitRunner/git_learning/.git/
# 设置user.name及设置user.email
$ git config --local user.name 'xixihaha123-xjh'
$ git config --local user.email '123456789@123.com
```

3. git clone

```shell
$ git clone <repo-url>              # git clone 从现有 Git 仓库中拷贝项目（类似 svn checkout）
$ git clone <repo-url> <directory>  # 克隆到指定的目录
```



## 3.2 配置

（1）配置 user.name 和user.email

1.  Git 单个仓库的配置文件位于 `~/$PROJECT_PATH/.git/config` 

```shell
$ git config --global user.name 'Nihaowa123-xjh'          # 设置提交代码时的用户名
$ git config --global user.email '13919250447@163.com'    # 设置提交代码时的用户邮箱

$ git config --list --global
user.email='13919250447@163.com'
user.name='Nihaowa123-xjh'
```

（2）git config的三个作用域

1. 缺省等同于 local

```shell
$ git config --local      # local只对某个仓库有效,
$ git config --global     # global对当前用户所有仓库有效
$ git config --system     # system对系统所有登录的用户有效
```

2. 显示config的配置，加list

```shell
$ git config --list --local
$ git config --list --global
$ git config --list --system
```

（3）为命令配置别名

```shell
# 为命令配置别名
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.st status
$ git config --global alias.br branch
```



## 3.3 增删

（1）工作目录 ----> git add file ----> 暂存区 ----->  git commit ----> 版本历史

（2）git commit -m  需用双引号，单引号报错

```shell
# git commit -m用于提交暂存区的文件，git commit -am用于提交跟踪过的文件
$ git commit -m'Add index + log'
error: pathspec 'index' did not match any file(s) known to git

$ git commit -m"Add index + log"
```

（3）增删文件

```shell
# git add命令。可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 将这个命令理解为“精确地将内容添加到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。

# 添加当前目录的所有文件到暂存区
$ git add .
​
# 添加指定文件到暂存区
$ git add <file1> <file2> ...
​
# 添加指定目录到暂存区，包括其子目录
$ git add <dir>
​
# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...
​
# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]
​
# 重命名文件
$ git mv [file-original] [file-renamed]
```

1. 把文件名 file1 添加到 .gitignore 文件里，Git 会停止跟踪 file1 的状态。 



## 3.4 查询

```shell
# 查看工作区文件修改状态
$ git status               
​
# 紧凑/简介的输出
git status -s 或 git status --short

# 查看工作区文件修改具体内容   
$ git diff [file]
​
# 查看暂存区文件修改内容
$ git diff --cached [file] 
​
# 查看版本库修改记录
$ git log                  
​
# 查看某人提交记录
$ git log --author=someone 
​
# 查看某个文件的历史具体修改内容
$ git log -p [file]        
​
# 查看某次提交具体修改内容
$ git show [commit]

# 列表的方式显示
$ git log --oneline

# 查看最近一次log信息
$ git log --oneline -n1

# 查看所有分支的log
$ git log --all

# log信息图形化显示
$ git log --all --graph

# 查看git帮助信息
$ git help --web log
```

## 3.5 拉取

```shell
# 下载远程仓库的所有变动 (Git only)
$ git fetch [remote]
​
# 显示所有远程仓库 (Git only)
$ git remote -v
​
# 显示某个远程仓库的信息 (Git only)
$ git remote show [remote]
​
# 增加一个新的远程仓库，并命名 (Git only)
$ git remote add [remote-name] [url]
​
# 取回远程仓库的变化，并与本地分支合并，(Git only), 若使用 Git-SVN，请查看第三节
$ git pull [remote] [branch]
​
# 取回远程仓库的变化，并与本地分支变基合并，(Git only), 若使用 Git-SVN，请查看第三节
$ git pull --rebase [remote] [branch]
```

## 3.6 分支

```shell
# 列出所有本地分支
$ git branch
​
# 列出所有本地分支和远程分支
$ git branch -a
​
# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]
​
# 新建一个分支，并切换到该分支
$ git checkout -b [new_branch] [remote-branch]
​
# 切换到指定分支，并更新工作区
$ git checkout [branch-name]
​
# 合并指定分支到当前分支
$ git merge [branch]
​
# 选择一个 commit，合并进当前分支
$ git cherry-pick [commit]
​
# 删除本地分支，-D 参数强制删除分支
$ git branch -d [branch-name]
​
# 删除远程分支
$ git push [remote] :[remote-branch]
```



## 3.7 提交

```shell
# 提交暂存区到仓库区
$ git commit -m [message]
​
# 提交工作区与暂存区的变化直接到仓库区
$ git commit -a
​
# 提交时显示所有 diff 信息
$ git commit -v
​
# 提交暂存区修改到仓库区，合并到上次修改，并修改上次的提交信息
$ git commit --amend -m [message]
​
# 上传本地指定分支到远程仓库
$ git push [remote] [remote-branch]
```



## 3.8 撤销

```shell
# 恢复暂存区的指定文件到工作区
$ git checkout [file]
​
# 恢复暂存区当前目录的所有文件到工作区
$ git checkout .
​
# 恢复工作区到指定 commit
$ git checkout [commit]
​
# 重置暂存区的指定文件，与上一次 commit 保持一致，但工作区不变
$ git reset [file]
​
# 重置暂存区与工作区，与上一次 commit 保持一致
$ git reset --hard
​
# 重置当前分支的指针为指定 commit，同时重置暂存区，但工作区不变
$ git reset [commit]
​
# 重置当前分支的HEAD为指定 commit，同时重置暂存区和工作区，与指定 commit 一致
$ git reset --hard [commit]
​
# 新建一个 commit，用于撤销指定 commit
$ git revert [commit]
​
# 将未提交的变化放在储藏区
$ git stash
​
# 将储藏区的内容恢复到当前工作区
$ git stash pop
```

## 3.9 图形化工具参考

（1）Git GUI汉化  参考网址------   https://www.cnblogs.com/chenghu/articles/12678500.html 

（2）gitk 使用



# 4 github



# 5 gitLab







## 02 修改最新commit message

（1）修改最近一次commit messge

```shell
F:\src\101-GitRunner\git_learning>git commit --amend
```

## 03 修改老旧commit的message

（1）修改老旧的commit的message

```shell
F:\src\101-GitRunner\git_learning>git rebase -i 13714a964ee763

reword 之编辑commit message信息
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
```

## 04 把连续的多个commit整理成1个

（1）把连续的多个commit整理成一个

```shell
F:\src\101-GitRunner\git_learning>git rebase -i 13714a964ee7

pick 082eb6a Add index and log
squash 1e0a9ba mv filename readme to changeme                                             squash a3d6db4 add commit.txt
```

## 05 把间隔的几个commit整理成1个

（1）把间隔的几个commit整理成1个

```shell
F:\src\101-GitRunner\git_learning>git rebase -i 13714a964ee7
```

## 06 - 比较暂存区和HEAD所含文件的差异？

（1）比较暂存区和HEAD所含文件的差异？ 若要查看已暂存的将要添加到下次提交里的内容，可以用 git diff --cached 命令。 

```shell
F:\src\101-GitRunner\git_learning>git diff --cached
```

## 07 比较工作区和暂存区所含文件的差异

（1）diff 默认比较的是工作区与暂存区文件的区别，git diff比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容。 git diff 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。 所以有时候你一下子暂存了所有更新过的文件后，运行 git diff 后却什么也没有，就是这个原因。 

```
F:\src\101-GitRunner\git_learning>git diff
```






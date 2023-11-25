---
title: 'Git 简易教程'
weight: 1001
---

# Git 指南

> 本教程涉及命令行指令说明时，`[]` 表示该参数为可选，`<>` 仅说明参数意义，需根据实际来定义

## Git 简介

Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理或大或小的项目。利用 git，可以在本地很好地控制项目版本，让项目有条不紊地进行，编写代码错误还能通过回退进行纠正；通过提交、拉取、推送等操作，与队友或同事参与远程共同开发。


在分布式版本控制系统中，系统保存的不是文件变化的差量，而是文件的快照，即把文件的整体复制下来保存，而不关心具体的变化内容。其次，最重要的是该控制系统是分布式的，开发者从中央服务器拷贝下来代码时，拷贝的是一个完整的版本库，包括历史纪录，提交记录等，这样即使某一台机器宕机也能找到文件的完整备份。





## Git 配置

Git 提供了一个叫做 git config 的工具，用于配置或读取相应的工作环境变量。这些环境变量决定了 Git 在各个环节的管理员信息和使用方式，存放于 /etc/gitconfig (所有用户生效 –system) 或 ~/.gitconfig (当前用户配置 –global)。

需要设置一下用户信息才能使用 Git 提交 commit，在终端输入：

```shell
git config --global user.name "yourname"
git config --global user.email "youremail"
```

## Git 基本流程

### 创建仓库

首先在 Github/Gitee 平台创建仓库 (New a repositry)，克隆到本地：

```shell
# write your answer here
git clone
```

或者在本地初始化仓库，并添加远程仓库链接：

```shell
git init    # 初始化仓库
git remote add origin <remote_url>  # 添加远程仓库，并命名为 origin（默认）
```

>
> 如果采用 clone 的方式，一定要 `cd` 进入到克隆下来的文件夹里，才能执行后续 Git 操作

### 修改提交

```shell
# 根据提示写出对应git命令
git add a.txt         # 将指定文件a.txt加入暂存区
git add .                   # 或使用.将项目所有文件加入暂存区（除.gitignore指定的文件外）
git commit -m "message"  # 将暂存区内容打包成commit，提交到本地仓库,提交信息为“message!"
git push                    # 将本地仓库内容提交到远程仓库
```

这样就实现了一次修改的提交和推送。

> 工作区、暂存区和本地仓库

- 工作区：即当前进行工作的文件目录，文件修改但未提交，处于已修改状态（modified）
- 暂存区：运行git add命令后文件保存的区域，也就是下次提交要保存的文件，文件处于已暂存状态（staged）
- 本地仓库：即版本库，记录了提交的完整状态和内容，该区域文件处于已提交状态（committed）

![](https://super-buaa-2021.github.io/SE-Labs/images/lab1/git.png)

### 同步远程仓库

在多人合作开发的情况下，需要定期 pull 仓库保持进度同步：

```shell
git pull [<remote_name> <remote_branch>[:<local_branch>]]
```

### 分支工作

在项目开发中，常常会建立两个分支：

- master/main：存储生产环境，可部署版本
- dev：存储开发环境

当然，在多人开发中，也可能会为每个人建立分支，大家在各自分支上完成自己的开发任务。

```shell
git branch -l                   # 查看所有分支
git checkout <branch_name>      # 切换到指定分支
git checkout -b <branch_name>   # 根据当前分支，新建一个分支并切换到该分支上
git merge [<branch1>] <branch2> # 将branch2合并到branch1，省略branch1参数时表示合并到当前分支
```

当合并分支出现冲突时，也就是两个人分别在各自分支上修改了同一处，Git 无法自动合并，则会报错冲突。此时需要手动修改冲突文件，可选择保留哪一分支的内容，之后再 `add/commit/push`。

### 错误回退

Git 多工作区和版本库机制，允许你回头是岸。

如果发现提交了错误文件：

- 若文件未添加至暂存区：使用 `git checkout <filename>` 将文件替换成暂存区版本，或 `git checkout .` 将工作区内的所有文件替换成暂存区内的文件（谨慎使用）
- 若文件已添加到暂存区但未提交到本地仓库：使用 `git reset HEAD .` 撤销暂存区的所有修改至工作区，接下来就回到了上一步
- 若已提交到本地仓库：使用 `git reset --hard HRAD^` 将所有文件回退至上一版本





# Git 指令全集



## 仓库

```shell
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库 (git init后面加参数)
$ git init "website"

# 下载一个项目和它的整个代码历史
$ git clone
```

## 配置

```shell
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e <--global>

# 设置提交代码时的用户信息
$ git config <--global> user.name "<name>"
$ git config <--global> user.email "<email address>"
```

## 增加/删除文件

```shell
# 添加指定文件到暂存区
$ git add

# 添加指定目录到暂存区，包括子目录
$ git add <dir>

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm <file1> <file2> ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached <file>

# 改名文件，并且将这个改名放入暂存区
$ git mv <file-original> <file-renamed>
```

## 代码提交

```shell
# 提交暂存区到仓库区
$ git commit -m "update"

# 提交暂存区的指定文件到仓库区
$ git commit a.txt -m "update"

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m <message>

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend <file1> <file2> ...
```

## 分支

```shell
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch <branch-name>

# 新建一个分支，并切换到该分支
$ git checkout -b <branch>

# 新建一个分支，与远程分支同步，并切换到该分支
$ git checkout -b <branch> <remote-branch>

# 新建一个分支，指向指定commit
$ git branch <branch> <commit>

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track <branch> <remote-branch>

# 切换到指定分支，并更新工作区
$ git checkout <branch-name>

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream-to=origin/<branch> <branch>

# 合并指定分支到当前分支
$ git merge <branch>

# 选择一个commit，合并进当前分支
$ git cherry-pick <commit>

# 删除分支
$ git branch -d <branch-name>

# 删除远程分支
$ git push origin --delete <branch-name>
$ git branch -dr <remote/branch>

```

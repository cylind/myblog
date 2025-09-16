---
title: Git使用教程
tags: Git
abbrlink: 8579
date: 2021-02-04 09:38:47
---

Git是一个分布式版本控制系统，可以记录仓库中所有文件的改动，每一个提交的改动成为一个版本（commit），可以据此进行版本的切换（通过移动HEAD指向不同的commit实现）等操作。

<!-- more -->

## 本地仓库

### 设置基本信息

```bash
$ git config --global user.name "<用户名>"
$ git config --global user.email "<电子邮件>"
$ git config --global color.ui auto
```

### 初始化Git仓库

```bash
$ git init
```

可以初始化一个空文件夹作为Git仓库，也可以初始化一个已经存在文件的文件夹作为Git仓库。执行完`git init` 后，当前文件夹下会出现一个`.git`文件夹（版本库），`.git` 文件夹里存储这个Git仓库的所有信息，包括版本变更等信息。

### 查看Git仓库的状态

```bash
$ git status
```

### 提交文件

提交文件前，先要要把文件添加到暂存区（stage）。可以通过`add` 指令将文件添加至暂存区，`add` 指令可一次添加多个文件，文件名用空格分隔。

```bash
$ git add <file>..
```

还可以通过指定`.`参数，一次添加所有文件

```bash
$ git add .
```

![工作区和版本库](https://raw.githubusercontent.com/cylind/cylind.github.io/static/img/20250916204921986.png)

文件添加到暂存区后，可以通过`commit`指令将文件提交到当前分支

```bash
$ git commit -m "first commit"
```

其中，`-m`表示提交说明，即本例中的 "first commit"。

### 创建分支

创建一个新分支并切换到该分支

```bash
git branch bugfix
git checkout bugfix
```

还可以直接合并简化成一条命令

```bash
git checkout -b bugfix
```

### 合并分支

#### merge

将HEAD切换回master(HEAD就是指向commit的指针，可通过checkout更改它指向的位置)，然后执行合并分支，将会在master之后产生一个同时指向bugfix和master的commit，然后master移动至该commit，合并完成。

```bash
git checkout master
git merge bugfix
```

#### rebase

rebase可将两分支合并表示为一条时间线，比较直观。将HEAD切换到bugfix，然后执行合并分支，这将会

```bash
git checkout bugfix
```

![](https://raw.githubusercontent.com/cylind/cylind.github.io/static/img/b644568a48e37c6c63dac789e0715c55.png)

```bash
git rebase main
```

![](https://raw.githubusercontent.com/cylind/cylind.github.io/static/img/20250916204527786.png)

### 查看日志

```bash
$ git log
```

### 版本回退

版本回退可以先通过git log 参看commit的时间线，确定回退到那个commit，确定后执行

```bash
git reset --hard 360dcc9 #此为commit id 可不必写全
```

要是平时commit时没有写有关键的说明，可能你不知道要回退到哪个版本合适（所以说平时提交commit时一定要注意这一点），这时候可以一个个commit往后找，先签出源码

```bash
git checkout 360dcc9
```

查看代码是不是符合你想回退的版本，如果不是，则继续签出下一个；如果是的话，先签出回当前分支代码，假设当前分支为master

```bash
git checkout master
```

再进行回退

```bash
git reset --hard 360dcc9
```

## 远程仓库

### 关联远程仓库

```bash
$ git remote add <name> <url>
```

其中，`<name>` 是我个远程仓库设置的别名，用来替代较长的仓库地址`url`，`<name>`一般设置为`origin`。执行推送或者拉取的时候，如果省略了远程数据库的名称，则默认使用名为”origin“的远程数据库。因此一般都会把远程数据库命名为origin。

### 推送到远程仓库

使用push命令向数据库推送更改内容。`<repository>`处输入目标地址，如果只有一个追踪分支，`<repository>`缺省；`<refspec>`处指定推送的分支，缺省则默认等前分支。

```bash
$ git push <repository> <refspec>...
```

如果当前分支与多个主机存在追踪关系，那么这个时候**-u选项（--set-upstream）**会指定一个默认主机，这样后面就可以不加任何参数使用git push。

```bash
$ git push -u origin master
```

上面命令将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push了。

### 从远程仓库拉取更新

使用pull指令进行拉取操作。省略数据库名称的话，会在名为origin的数据库进行pull。

```bash
$ git pull <repository> <refspec>...
```

### 整合修改记录

在执行pull之后，进行下一次push之前，如果其他人进行了推送内容到远程数据库的话，那么你的push将被拒绝。这种情况下，在读取别人push的变更并进行合并操作之前，你的push都将被拒绝。这是因为，如果不进行合并就试图覆盖已有的变更记录的话，其他人push的变更就会丢失。

执行合并即可自动合并Git修改的部分。但是，也存在无法自动合并的情况。

如果远程数据库和本地数据库的同一个地方都发生了修改的情况下，因为无法自动判断要选用哪一个修改，所以就会发生冲突。

![冲突](https://raw.githubusercontent.com/cylind/cylind.github.io/static/img/git-conflict.png)

==分割线上方是本地数据库的内容, 下方是远程数据库的内容。

这时候，有冲突的文件会被插入如上图的标注，需要手动修正冲突，即修改本地文件的冲突部分，然后 `git add --all && git commit` 

## 参考

https://git-scm.com/docs/gittutorial

https://try.github.io/

https://guides.github.com/introduction/git-handbook/

https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/

https://backlog.com/git-tutorial

[廖雪峰-Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

[git push 的 -u 参数具体适合含义？](https://www.zhihu.com/question/20019419)

[如何给开源项目贡献代码](https://gist.github.com/zxhfighter/62847a087a2a8031fbdf)

[ubuntu-a-beginners-guide-to-git-github](https://mvthanoshan.medium.com/ubuntu-a-beginners-guide-to-git-github-44a2d2fda0b8)

[How to make your first pull request on GitHub](https://mvthanoshan.medium.com/how-to-make-your-first-pull-request-on-github-9aefca5cc837)
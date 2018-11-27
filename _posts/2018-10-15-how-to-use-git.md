---
layout: post
title: 使用Git进行版本控制
date: 2018-10-15 14:26:19
tags: Git
categories: Tech
---


公司一直在使用SVN来做版本控制，虽然个人项目早已换成Git，但是对Git没有一个深入的了解，最近因为分支和标签用法的困惑，继而产生想系统学习Git使用的想法，本文是基于廖雪峰Git教程而整理的学习笔记。[Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

## Git概念

### 版本库和工作区

![版本库和工作区](/../assets/images/post/2018-10-12-15393197926757.jpg)

> `master`是Git自动创建的第一个分支

`Git`工作分为两步

1. `git add`把文件修改添加到暂存区
    ![git add](/../assets/images/post/2018-10-12-15393198237337.jpg)
2. `git commit`把暂存区的所有内容提交到当前分支
    ![git commit](/../assets/images/post/2018-10-12-15393198436289.jpg)
        
### Git协议

Git支持多种协议，`git://`和`https`等
`https`协议速度会慢一些

### 专有名词

`master`git自动创建的第一个分支
`HEAD`当前版本
`origin`远程库   

## 基础命令

1. 在目录中初始化Git

    ```
    git init
    ```

2. 将修改添加到Git

    ```
    # 添加全部改动（常用）
    git add .

    # 添加具体变动
    git add file1 file2
    ```

3. 将修改提交
    
    ```
    git commit -m "changelog"
    ```
    
4. 查看仓库当前状态
    
    ```
    git status
    ```
    
5. 查看文件修改内容

    ```
    git diff filename
    ```
    
6. 查看Git日志

    ```
    # 完整日志（带时间）
    git log
    
    #精简日志
    git log --pretty=oneline
    ```
    
7. 将当前版本回退（切换）到具体版本

    ```
    # 回退到具体版本号（版本号不需要写全，只需前几位）
    git reset --hard commit_id
    
    # 回退到当前版本的上一版本
    git reset --hard HEAD^
    ```
    
> HEAD表示当前版本
> 上一个版本就是HEAD\^
> 上上一个版本就是HEAD\^\^
> 当然往上100个版本写100个\^比较容易数不过来，所以写成HEAD~100
    
8. 查看历史命令（历史版本号）

    ```
    git reflog
    ```

9. 撤销工作区中的修改（使用版本库中的版本替换工作区版本）

    ```
    # “--”很重要，不然会和checkout命令冲突
    git checkout -- filename
    ```
    
> 1. 一种是file自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
> 2. 一种是file已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
    
10. 撤销暂存区中的修改

    ```
    git reset HEAD filename
    ```
    
11. 将本地仓库添加到远程库中

    ```
    git remote add origin git@github.com:zrocky/testgit.git
    ```
    
12. 将本地库中的内容推送到远程库上

    ```
    # 首次push需添加-u参数
    git push -u origin master
    
    # 提交到远程库
    git push origin master
    ```
    
> 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来
    
13. 从远程库中克隆到本地库

    ```
    git clone git@github.com:zrocky/testgit.git
    ```

## 分支与标签

### Branch（分支）

`master`为主分支

`HEAD`指向当前分支

![](/../assets/images/post/2018-10-12-15393228460803.jpg)

每次提交`master`分支都会向前移动一步
<video controls="controls" src="/../assets/images/post/master-branch-forward.mp4" width="100%"></video>

创建新的分支`dev`，将`HEAD`指向`dev`

![](/../assets/images/post/2018-10-12-15393233193864.jpg)

从现在开始，工作区的修改和提交就是针对`dev`分支的了

![](/../assets/images/post/2018-10-12-15393233502209.jpg)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上

![](/../assets/images/post/2018-10-12-15393233995876.jpg)

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支

![](/../assets/images/post/2018-10-12-15393234395115.jpg)

过程动画：
<video controls="controls" src="/../assets/images/post/master-and-dev-ff.mp4" width="100%"></video>


#### 分支命令

1. 创建分支

    ```
    git branch dev
    
    # 创建并切换分支
    git checkout -b dev
    ```
    
2. 切换分支

    ```
    git checkout dev
    ```
    
3. 查看当前分支

    ```
    git branch
    ```
    
4. 合并分支到当前分支

    ```
    git merge dev
    ```
    
5. 删除分支

    ```
    git branch -d dev
    ```

6. 查看分支合并图

    ```
    git log --graph
    
    # 显示简化图
    git log --graph --pretty=oneline --abbrev-commit
    ```
    
#### 分支合并冲突

![](/../assets/images/post/2018-10-12-15393286280378.jpg)

此种情况需解决冲突再提交

![](/../assets/images/post/2018-10-12-15393287303720.jpg)

#### 分支策略

如果分支合并没有冲突等异常情况，Git会使用`Fast forward（快进）`模式，该模式下，删除分支后，会丢失分支信息

使用`--no--ff`参数禁用`Fast forward`:

```
git merge --no-ff -m "merge with no-ff" dev
```

![](/../assets/images/post/2018-10-12-15393299601183.jpg)

##### 分支开发策略

`master`分支是非常稳定的，也就是仅用来发布新版本，平时不在上面工作，开发都在`dev`分支上，每个人再有自己的分支，按时向`dev`分支合并即可，团队合作的分支看起来是这样的

![](/../assets/images/post/15393300560209.jpg)

##### bug处理分支

在`dev`分支工作，手头工作并未完成，不能提交，但是bug急修，这个时候，可以使用`stash`先将工作区暂时“储藏”起来，等修复bug后再恢复

```
git stash
```

假定为修复线上bug，可从`master`分支创建临时分支

```
git checkout -b issus-101
```

bug修复完成后，合并分支到`master`，再切换回`dev`分支，使用`stash`恢复之前的工作区

查看`stash`挂起的任务

```
git stash list
```

恢复`stash`任务

```
# 只恢复任务，挂起的任务还在list中
git stash apply stash@{0}

# 恢复的同时把stash挂起任务删除
git stash pop
```

删除`stash list`中任务

```
git stash drop stash@{0}
```

##### Feature分支

feature分支和bug分支的处理类似，但如果feature最终放弃，需要删除分支时，使用原来的分支将会报错，需要删除未合并的分支时，要使用`-D`参数强行删除

```
git branch -D feature-vulcan
```

##### 多人协作

查看远程库信息

```
git remote

# 详细信息
git remote -v
```

推送分支到远程库

```
# 主分支
git push origin master

# 开发分支
git push origin dev

```

> 1. `master`分支为主分支，需要时刻与远程同步
> 2. `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步
> 3. bug和feature则按需求决定是否同步到远程

###### 抓取分支

从远程库`clone`到本地时，默认只能看到`master`分支，如果需要再`dev`等其它分支上开发，则必须创建`origin`远程库的`dev`分支到本地

```
git checkout -b dev origin/dev
```

如果`push`更新到远程分支时发生冲突，则需要使用`git pull`命令从远程拉取更新，在本地处理完冲突后，再用`git push origin <branch-name>`命令`push`到远程库

> 如果`git pull`命令提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`解决

##### Rebase（变基）

使用`rebase`可以整理本地`commit`的分叉，使之变成线性，而不再是“蜘蛛网”，`rebase`只对尚未推送或分享给别人的本地修改执行变基操作清理历史，从不对已推送至别处的提交执行变基操作

```
git rebase
```

### Tag (标签）

`tag`是版本库的一个快照，和`branch`相似，但是不能像`branch`一样移动

基于当前分支创建新标签

```
git tag v1.0
```

基于特定版本创建新标签

```
git tag v0.9 f52c633

# 指定说明文字
git tag -a v0.1 -m "version 0.1 released" 1094adb
```

查看所有标签

```
git tag 
```

查看某个标签的信息

```
git show v0.9
```

推送标签到远程库

```
git push origin v1.0
```

推送全部标签到远程库

```
git push origin --tags
```

删除本地标签

```
git tag -d v0.1
```

删除远程标签

```
# 先在本地删除
git tag -d v0.9

# 远程删除
git push origin :refs/tags/v0.9
```

## Git高级

### 远程库操作

删除远程库

```
git remote rm origin
```

> 只删除了关联关系，并未删除远程数据

添加多个远程库

```
git remote add bitbucket git@bitbucket.org:Zrocky/git_test.git
```

> 只需在push时，选择相应的远程库即可

### 忽略文件

有一些文件你不想上传至git仓库中，这个时候可以使用忽略功能，将需要忽略的文件标示给git。

首先在Git工作区的根目录创建一个`.gitignore`文件，把需要忽略的文件名填写进去，git就会自动忽略这些文件。GitHub创建了一些可供参考的预设文件，我们可以直接拿来修改使用:[gitignore](https://github.com/github/gitignore)

忽略文件的原则是：

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

### 搭建Git服务器

搭建Git服务器，需要准备一台运行Linux的机器，推荐使用Ubuntu或Debian

第一步，安装`git`

```
sudo apt-get install git
```

第二步，创建一个`git`用户，用来运行`git`服务：

```
sudo adduser git
```

第三步，创建证书登录

收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

第四部，初始化Git仓库

```
sudo git init --bare sample.git
```

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：

```
sudo chown -R git:git sample.git
```

第五步，禁用shell登录

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

```
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为

```
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

第六步，克隆远程仓库

```
git clone git@server:/srv/sample.git 
```

#### 管理公钥

如果团队成员比较多，可以使用[Gitosis](https://github.com/res0nat0r/gitosis)管理公钥

#### 管理权限

Git本身不支持权限控制，不过Git支持hook，所有可以在服务器端编写脚本实现权限控制，使用[Gitolite](https://github.com/sitaramc/gitolite)可以实现这个功能



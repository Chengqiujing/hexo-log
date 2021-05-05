---
title: git使用
date: 2021-05-05
comments: true
categories: git
tags: git基础使用
---

# git使用

git安装
---

1.[官网下载](https://git-scm.com/downloads)

2.Git是分布式版本控制系统,指定名字和Email

```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
--global 参数表示本机所有的仓库都使用这个名字
```

---

创建本地版本库
---

1.创建目录 /gitRepository

2.到目录下 执行 `git init` 初始化仓库

---

<!--more-->

链接到远程库
---

1. 先在远程如github中创建一个远程仓库（步骤网站提供）

由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的

>第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。
>如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
>
>```
>ssh-keygen -t rsa -C "youremail@example.com"
>你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码
>在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人
>```
>
>第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面： 
>然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容,点击添加

2.将本地目录链接到远程仓库 

`git remote -v //查看远程仓库`


```
git remote add origin git@github.com:michaelliao/learngit.git
- michaelliao : 账户名
- learngit 仓库名

// 创建完的远程仓库名为 origin

```

当第一次使用`git clone`或者`git push` 作用于remote git仓库时，可能会遇到下面问题

```
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?

这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
```

3.还可以使用 git clone的方式链接

```
//跳转到本地指定目录，执行克隆
git clone git@github.com:michaelliao/gitskills.git

你也许还注意到，GitHub给出的地址不止一个，还可以用https://github.com/michaelliao/gitskills.git这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。
使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。
```

4.多人远程协作

```
查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

git rebase//把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。


如果本地新建了一个分支branch_name，但是在远程没有，这时候push和pull指令就无法确定该跟踪谁,一般来说我们都会使其跟踪远程同名分支，
所以可以利用git push --set-upstream origin branch_name，这样就可以自动在远程创建一个branch_name分支，然后本地分支会track该分支。
后面再对该分支使用push和pull就自动同步。无需再指定分支。

如果远程新建了一个分支，本地没有该分支，可以用git checkout --track origin/branch_name，
这时候本地会新建一个分支名叫branch_name，会自动跟踪远程的同名分支branch_name。

一般我们就用git push --set-upstream origin branch_name来在远程创建一个与本地branch_name同名的分支并跟踪；
利用git checkout --track origin/branch_name来在本地创建一个与branch_name同名分支跟踪远程分支。
```

---

操作--增删改查
---

* 增

```
git add 文件名
git commit -m "信息"

提交部分文件
git commit   /test/web.config   -m   "commit" （提交部分代码加备注）

推送到远程
git push -u origin master
-u Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
$ git push origin master //master 是指本地master
```

* 查

```
git log //只显示最近三次 
git log --pretty=oneline //加上显示的更规整

git reflog //用来记录你的每一次命令

git status //工作区状态

git diff HEAD -- readme.txt //查看工作区和版本区的区别

git branch //命令查看当前分支

git log -1 //最后一次提交

git status 只能查看未传送提交的次数

git cherry -v只能查看未传送提交的描述/说明

git log master ^origin/master则可以查看未传送提交的详细信息
```

* 改

```
//撤回版本
git reset --hard HEAD^ | $ git reset --hard 1094a  
在Git中，用HEAD表示当前版本，也就是最新的提交1094adb...（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，
当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

//放弃暂存区修改
git checkout -- readme.txt
```

* 删

```
git rm 文件名 
git commit

小提示：先手动删除文件，然后使用git rm <file>和git add<file>效果是一样的。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
$ git checkout -- test.txt
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”


当git add 某个文件到缓存区，还没有git commit 但是你不想这个文件了
就可以使用git rm命令，两种选择：

git rm --cached “文件路径”，不删除物理文件，仅将该文件从缓存中删除；
git rm --f “文件路径”，不仅将该文件从缓存中删除，还会将物理文件删除（不会回收到垃圾桶）。

```

分支
---

```
git checkout -b dev  //-b 创建并切换分支 等于使用了 git branch dev， git checkout dev
 
git checkout master //切回主分支
git meger dev //将分支合并到主分支
git branch -d dev//合并完删除分支

 git switch -c dev    //新命令 等同git checkout -b dev
 
 //冲突可以提交后看到提示去文件中修改
 
 //查看合并情况                                     
 git log --graph --pretty=oneline --abbrev-commit
 
 ** 
 不使用 fast foward方式以便留下记录
 git merge --no-ff -m "merge with no-ff" dev 
 **
 
 
 git stash //可以把当前工作现场“储藏”起来，等以后恢复现场后继续工
 //git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug
 
 git stash list //查看储存列表
 
 //恢复有两种方式
 //一种
 git stash apply  //恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除
 //另一种
 git stash pop //恢复的同时把stash内容也删了
 
 
 **
 git cherry-pick 12wqer2 //将指定的一次提交复制到当前分支
 **
```

---

Tag
---

1.首先切换到分支上

```
//创建标签
git tag <name>
//创建带有说明的标签
$ git tag -a v0.1 -m "version 0.1 released" 1094adb

//删除标签
git tag -d <name>

//查询tag
git tag
//查看标签信息
git show <tagname>


//对某一次提交进行贴Tag，指定commit ID 就可以了
git tag v0.9 f52c633

//推送某个标签到远程
git push origin v1.0 
//一次性推送全部尚未推送到远程的本地标签
git push origin --tags 
//删除远程
git tag -d v0.9 //先删除本地
git push origin :refs/tags/v0.9 //在删除远程

```

---

Git config
---

```
git config --global color.ui true //让Git显示颜色
//配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。

每个仓库的Git配置文件都放在.git/config文件中
$ cat .git/config 
[core]
    repositoryformatversion = 0
    filemode = true
    bare = false
    logallrefupdates = true
    ignorecase = true
    precomposeunicode = true
[remote "origin"]
    url = git@github.com:michaelliao/learngit.git
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
[alias] 
    last = log -1  
//别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。

//在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件
```

---

忽略文件
---

使用Windows的童鞋注意了，如果你在资源管理器里新建一个.gitignore文件，它会非常弱智地提示你必须输入文件名，
但是在文本编辑器里“保存”或者“另存为”就可以把文件保存为.gitignore了。  
其中填入要忽略的文件

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini
```

最后一步就是把.gitignore也提交到Git，就完成了

---

别名
---

```
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
```

---

搭建git服务器
---

第一步：准备linux服务器

第二步：安装git 

```
$ sudo apt-get install git
```

第三步：创建一个git用户，用来运行git服务

```
$ sudo adduser git
```

第三步:创建证书登录  
集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，
把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

第四步:初始化Git仓库
先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：

```
$ sudo git init --bare sample.git
```

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，
所以不让用户直接登录到服务器上去改工作区，
并且服务器上的Git仓库通常都以.git结尾

然后，把owner改为git：

```
$ sudo chown -R git:git sample.git
```

第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

git:x:1001:1001:,,,:/home/git:/bin/bash
改为：
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

第六步，克隆远程仓库：

现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：

$ git clone git@server:/srv/sample.git
Cloning into 'sample'...
warning: You appear to have cloned an empty repository.
剩下的推送就简单了。

管理公钥
如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用Gitosis来管理公钥。

这里我们不介绍怎么玩Gitosis了，几百号人的团队基本都在500强了，相信找个高水平的Linux管理员问题不大。

管理权限
有很多不但视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。Gitolite就是这个工具。

这里我们也不介绍Gitolite了，不要把有限的生命浪费到权限斗争中。

小结
搭建Git服务器非常简单，通常10分钟即可完成；

要方便管理公钥，用Gitosis；

要像SVN那样变态地控制权限，用Gitolite。

# 问题：

fatal: refusing to merge unrelated histories
（拒绝合并不相关的历史）

**解决**

出现这个问题的最主要原因还是在于本地仓库和远程仓库实际上是独立的两个仓库。假如我之前是直接clone的方式在本地建立起远程github仓库的克隆本地仓库就不会有这问题了。

查阅了一下资料，发现可以在pull命令后紧接着使用--allow-unrelated-history选项来解决问题（该选项可以合并两个独立启动仓库的历史）。

**命令**：

`$git pull origin master --allow-unrelated-histories`
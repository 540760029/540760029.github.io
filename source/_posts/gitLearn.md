---
title: gitLearn
date: 2020-09-12 20:20:06
tags: git
categories: git
---

### 安装完git:
#### 1. 每个机器都必须自报家门：你的名字和Email地址
```bash
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
` --global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置.
<!--more-->
#### 2. 新建文件夹，交给git 托管
```bash
$ git init
```
#### 3. 新建readme.txt做完工作后添加修改的文件到仓库暂存区
```bash
$ git add readme.txt
```
然后提交到仓库
```bash
$ git commit -m "wrote a readme file"
```
#### 4、查看库当前的状态，看看工作区情况
```bash
$ git status
```
```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
modified:   readme.txt
no changes added to commit (use "git add" and/or "git commit -a")
```
#### 5.记不清上次怎么修改的，所以，需要用git diff这个命令看 顾名思义就是查看difference
```bash
$ git diff readme.txt
```
#### 6、版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用git log命令查看：
```bash
$ git log
```
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：
```bash
$ git log --pretty=oneline
```
#### 7、版本回退
|指令|含义|
|:-:|:-:|
|`$ git reset --hard HEAD^`|	退回到上一个版本|
|`$ git reset --hard HEAD^^`|	回到上上一个版本|
|………|	……|

#### 8、 `git reflog`用来记录你的每一次命令
```bash
$ git reflog
```

|版本号|   head |                      命令|
|:-:|:-:|:-:|
|86a9b0d| (HEAD -> master) HEAD@{0}:| commit: 添加一行(当前head的指向)|
|66853e9 |HEAD@{1}:| commit: dsfds
|5c159eb |HEAD@{2}:| reset: moving to 5c159eb
|1db339d |HEAD@{3}:| reset: moving to 1db339d
|5c159eb |HEAD@{4}:| reset: moving to file


回退到指定版本
```bash
	$ git reset --hard 5c159eb
```

|Git  log|	Git  reflog|
|:-:|:-:|
|显示所有提交过的版本信息|	可以查看所有分支的所有操作记录|

#### 9、用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别

####  10、`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
```bash
Git checkout -- test.txt     ( 将版本库中的文件替换到工作区)
```
#### 11、远程仓库
  ##### 1. 创建SSH Key。
在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
```bash
$ ssh-keygen -t rsa -C "youremail@example.com"
```
  ##### 2. 登录GitHub 添加 id_rsa.pub到 sshkey
 ##### 3. 新建 leargit 仓库
 ##### 4. 关联远程库与本地库
```bash
$ git remote add origin git@github.com:540760029/leargit.git
```

##### 5.  把本地库推送到远程库
```bash
$ git push -u origin master
```
由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令
之后直接使用
```bash
$ git push origin master
```
把本地master分支的最新修改推送至GitHub

**指定分支**推送到*指定分支*
```bash
$ git push origin source:source
```


##### 6. 从远程仓库克隆
```bash
$ git clone git@github.com:540760029/leargit.git
```
#### 12、分支与合并
##### 创建分支

创建`dev` 分支然后切换到`dev` 分支
```bash
$ git checkout -b dev
```
`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令
```git bash
$ git branch dev
$ git checkout dev
```
		
新版本支持
```bash
$ git switch -c dev
```
创建并切换分支

##### 查看当前分支
```bash
$ git branch
```
##### 合并分支
```bash
$ git merge dev
```
##### 删除分支
```bash
$ git branch -d dev
```
#### 13、 修复bug 保存现场与恢复
     
`git stash ` 将修改的地方保存到隐秘的地方。

切换分支 修改bug,

修改完之后，分支切回来 

将bug在本分支重放：
```bash
git cherry-pick  9726d19
```
查询隐秘地方保存的东西：
```bash
Git stash list
```
回复现场
```bash
	git stash pop
	或者
	$ git stash apply stash@{0}
```
继续工作 keeping!!!
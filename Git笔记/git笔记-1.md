# git笔记

## git简介

## 安装git

## 创建版本库

```bash
mkdir <repository name>    ## 建立仓库
cd <repository name>     ## 必须先进入，然后初始化
git init    ## 初始化该仓库
```

建立并初始化目录后，在当前目录下会生成一个`.git`的文件夹，在Windows端会显示，Linux端点开头文件为隐藏文件

```bash
git add <file name>    ## 向缓冲区添加一个文件
git commit -m "当次上传说明（建议填写且有意义）"    ## 提交到仓库
```

## 时光机穿梭

`git status`  返回当前仓库的状态

`git diff`  查看本地文件和仓库中的文件的difference

### 版本回退

`git log`  可以查看从最近到最远的提交日志，可以添加参数`--pretty=oneline` 使每条信息只显示一行

版本回退首先要明白回退到哪个版本，在git中庸HEAD表示当前的版本，所以上一个版本就是HEAD^，上上一个版本就是HEAD^^，往后就不会这么写了，写成HEAD~100

`git reset --hard HEAD^`  表示回退到上一个版本

那么回退到上一个版本之后后悔了怎么办呢？

`git reset --haed <之前的版本的commit号（输入前几位能区分就行）>`  就可以后悔了

### 工作区和暂存区

#### 工作区（working Directory）

电脑里能看到的目录，就是当前文件夹

#### 版本库（Repository）

就是工作区的`.git`文件夹。这个不算是工作区，是版本库。

Git的版本库有很多东西，最重要的是stage（或者叫index）的**暂存区**，还有第一个分支master以及指向该分支的指针HEAD。

当使用git add的时候,实际上是给文件一个指针存到stage里面去

然后使用commit的时候,再把该指针提交到分支当中去

### 管理修改

git管理的是一次次的修改,所以每一个文件,逢修改必添加,若第一次修改之后添加过,而后再修改,未添加就提交,那么提交的只是第一次的修改

### 撤销修改

1. 撤销工作区的修改,也就是恢复到最后一次add或者commit的版本

```
git restore <file name>
git checkout -- <file name>
效果相同
```

注:restore是git 2.23版本新增的命令,用来代替身兼多职的git checkout.功能是一样的

2. 当需要回退暂存区时(此时文件已经add到暂存区,还未进行commit)

```
git restore --staged<file name>
git reset HEAD<file name>
```

### 删除文件

先删除本地文件

`rm <file name>` 此时`git status` 晓得你在工作区已经删除了此文件,他会给出两个选择:

1. 从版本库彻底删除

   `git rm <file name>`

2. 误删,需要恢复到本地工作区

   `git checkout --<file name>`  (过时)

   `git restore <file name>` 

## 远程仓库

配置SSH

在用户目录下` ssh-keygen -t rsa -C "klelee@outlook.com"`所有选项回车默认

然后在用户目录下会生成.ssh文件夹,里面会有id_rsa and id_rsa.pub

其中id_rsa.pub中就是公钥,另外一个问价夹中是私钥

### 添加远程仓库

在github或者gitee建立新的仓库

#### 将本地仓库推送到远程仓库

```
git remote add origin https://gitee.com/klelee/learngit.git
git push -u origin master
```

上面推送的命令中u参数,表示不但上传了所有的文件,还将本地的master分支与远程的master分支进行了绑定

之后本地进行修改之后,可以通过

`git push origin master`进行推送

#### 删除远程库

想要删除一个远程库可以使用git remote rm <name>命令.使用前可以查看远程库的信息:

`git remote -v`

这样子的删除,只是删除了本地库和远程库之间的联系,本地库和远程库本身都还是存在的,要删除真正的远程库,需要登陆到远程库进行删除.








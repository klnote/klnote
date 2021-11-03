# Git笔记

## 分支管理

1. 创建新的分支并切换到新分支

   ` git cheackout -b dev ` 

   git checkout 加上-b命令表示，创建并切换到新分支，相当于两条命令

   ```
   git branch dev
   git checkout dev
   ```

然后可以用git branch来查看当前分支：

`git branch`它会列出所有的分支，并在当前分支前面用星号标示

---

这样，之后的所有操作就是在dev分支上进行了

---

当在新分支上的工作结束时，要将新的分支合并到master分支上

切换回到master分支`git checkout master`

合并两个分支`git merge dev`

合并之后删除新的分支`git branch -d dev`

#### switch

git在新版本中提供了新的切换分支的方式

`git switch -c dev` 可以创建并切换到dev分支

分支之间的切换`git switch dev`

### 解决冲突

假设在新的分支`feature1`上做了修改并添加提交，然后回到master分支，此时git还会提示我们当前的master比远程的master分支要超前一个提交，此时在master上修改。然后，master和新的分支feature1都有了新的提交，这种情况下git无法快速合并，若试图把各自的修改合并起来就会产生冲突。

所以，在遇到这种情况的时候就需要在文件中手动修改了。

### 分支管理策略

#### 强制禁用 Fast forword模式

在禁用fast froword的情况下，git在merge的时候就会生成一个新的commit，这样，在分支历史上就可以看出分支信息。

```
git switch -c dev
## 操作
git add <file>
git commit -m "reason"
git switch master
git merge --no-ff -m "merge with no-ff" dev ## 禁用fast forword的合并
```

在上面合并中，使用-m参数做上传说明使用为no-ff上传本身就带有一次commit

### bug分支

假设你在工作中突然接到命令需要修改一个其他的bug，此时？？？

首先，需要将现在手头的工作放下：`git stash` 可以将当前的进度保存

然后你就可以去你需要修改bug的分支，创建一个新的分支去修改bug了，加入你的bug在master分支上：

```
git switch master
git switch -c dev
## 修改bug
## bug修改完成
git switch master
git merge --no-ff -m "fixed bug" dev
git branch -d dev
```

然后返回到你之前工作的分支，用`git stash list`可以查看你保存的工作进度，

使用：

```
（1） 
git stash apply ## 可以恢复你的工作进度，但是不会删除内存上存储的你的进度，需要使用
git stash drop  ## 释放存在内存上你的进度
（2）
git stash pop  ## 可以直接从内存中提取出你的工作进度
```

反思：既然master分支上出现了bug，那么早期的dev分支的代码应该也有bug才对，那么早期dev分支的bug怎么办呢？

这里只需要把刚刚在master修改bug的commit复制到dev，只复制他的修改。这里git提供了一个cherry-pick命令，可以复制一个特定的提交到当前分支

### feature分支

开发新功能需要开辟的新的分支

在新分支中，对于已经添加并提交的修改，要删除需要强制删除使用参数-D

### 多人协作

#### 推送分支

`git push origin master`  主分支，时刻与远程同步

`git push origin dev`  开发分支，团队需要在dev分支上面工作，因此需要远程同步

bug分支，只用于修复bug不用同步

feature分支要不要推送取决于新功能你一个人能都开发的了

#### 抓取分支

`git pull` 多人协作时，当你和你的伙伴同时对一个文件进行修改，你的伙伴先push，那么你就得，先将最新的修改pull下来，在本地进行合并，如果合并有冲突，先解决冲突，然后进行提交

## 标签管理

### 创建标签

`git tag <tag name>` 创建一个新标签，在当前分支的最近一次提交上

`git tag <tag name> <commit code>`  创建一个新标签，在当前分支的制定commit上

`git tag` 查看所有标签

`git show <tag name>` 查看标签信息

### 操作标签

如果一个标签打错了，可以删除`git tag -d <tag name>`

推送某个标签到远程`git push origin <tag name>`

或者一次性将所有标签都推送到远程`git push origin --tags`

但是如果标签已经推送到远程再想要删除，就得先删除本地标签，然后在删除远程标签`git push origin :refs/tags/<tag name>`


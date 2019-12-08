# gitusage

简要介绍git的使用

## 1. git简介

### 1.1 git的诞生

由于Linux内核之前所选择的版本控制系统BitKeeper要收回Linux社区的免费使用权, Linus于是花了两周的时间用C语言写了一个分布式版本控制系统, 这就是git.

### 1.2 集中式vs分布式

![](./pic/distributed.png)

集中式版本控制系统的最大毛病就是必须联网才能工作, 如果在局域网内还好, 带宽够大, 速度够快, 可如果在互联网上, 遇到网速慢的话, 可能提交一个10M的文件就需要5分钟, 这还不得把人给憋死啊.

![](./pic/centralized.png)

和集中式版本控制系统相比, 分布式版本控制系统的安全性要高很多, 因为每个人电脑里都有完整的版本库, 某一个人的电脑坏掉了不要紧, 随便从其他人那里复制一个就可以了. 而集中式版本控制系统的中央服务器要是出了问题, 所有人都没法干活了.

### 1.3 创建版本库

创建一个版本库非常简单, 首先, 选择一个合适的地方, 创建一个空目录:

```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

第二步, 通过`git init`命令把这个目录变成git可以管理的仓库:

```
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

至此, git仓库就瞬间创建好了. 当前目录下多了一个`.git`的目录, 该目录是git用来跟踪管理版本库的.

## 2. 时光穿梭机

我们已经成功添加并提交了一个readme.txt文件, 现在, 是时候继续工作了, 于是, 我们继续修改readme.txt文件, 改成如下内容:

```
Git is a distributed version control system.
Git is free software.
```
现在, 运行`git status`命令看看结果:

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`git status`命令可以让我们时刻掌握仓库的当前状态, 上面的命令输出告诉我们, `readme.txt`被修改过了, 但是还没有准备提交的修改.

虽然git告诉我们`readme.txt`被修改了, 但是如果能看看具体修改了什么内容, 自然是很好的. 比如你休假两周从国外回来, 第一天上班时, 已经记不清上次怎么修改的`readme.txt`, 所以, 需要`git diff`这个命令看看:

```
$ git diff readme.txt
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

`git diff`顾名思义就是查看difference, 显示的格式正式Unix通用的diff格式, 可以从上面的命令输出看到, 我们在第一行添加了一个`distributed`单词.

知道了对`readme.txt`作了什么修改后, 再把它提交到仓库就放心多了, 提交修改和提交新文件是一样的两步, 第一步是`git add`:

```
$ git add readme.txt
```

同样没有任何输出. 在执行第二步`git commit`之前, 我们再运行`git status`看看当前仓库的状态:

```
$ git status
On branch master
Changed to be committed:
  (use "git reset HEAD <file>..." to unstage)

    modified:    readme.txt
```

`git status`告诉我们, 将要被提交的修改包括`readme.txt`, 下一步, 就可以放心地提交了:

```
$ git commit -m "add distributed"
[master e475afc] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

提交后, 我们再用`git status`命令看看仓库的当前状态:

```
$ git status
On branch master
nothing to commit, working tree clean
```

git 告诉我们当前没有需要需要提交的修改, 而且, 工作目录是干净(working tree clean)的.

## 3. 远程仓库

## 3. 分支管理

## 4. 标签管理

## 5. 自定义git

## 6. 总结

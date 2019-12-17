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

### 2.1 版本回退

现在, 你已经学会了修改文件, 然后把修改提交到git版本库, 现在, 再练习一次， 修改`readme..txt`文件如下:

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

然后尝试提交:

```
$ git add readme.txt
$ git commit -m "append GPL"
[master 1094adb] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
```

像这样, 你不断对文件进行修改, 然后不断提交修改到版本库里, 就好比玩RPG游戏时, 每通过一关就会自动把游戏状态存盘, 如果某一关没过去, 你还可以选择读取前一关的状态. 有些时候, 在打Boss之前, 你会手动存盘, 以便万一打Boss失败了, 可以从最近的地方重新开始. git也是一样, 每当你觉得文件修改到一定程度的时候, 就可以"保存一个快照", 这个快照在git中被称为commit. 一旦你把文件改乱了, 或者误删了文件, 还可以从最近的一个commit恢复, 然后继续工作, 而不是把几个月的工作成果全部丢失.

现在, 我们回顾一下`readme.txt`文件一共有几个版本被提交到git仓库里了:

版本1: wrote a readme file

```
Git is a version control system.
Git is free software.
```

版本2: add distributed

```
Git is a distributed version control system.
Git is free software.
```

版本3: append GPL

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

当然了, 在实际工作中, 我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容, 不然要版本管理系统干什么. 版本管理系统肯定有某个命令可以告诉我们历史记录, 在git中, 我们用`git log`命令查看:

```
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

`git log`命令显示从最近到最远的提交日志, 我们可以看到3次提交, 最近的一次是`append GPL`, 上一次是`add distributed`, 最早的一次是`wrote a readme file`.

如果嫌输出信息太多, 看得眼花缭乱的, 可以试试加上`--pretty=oneline`参数:

```
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

需要友情提示的是, 你看到的一大串类似`1094adb...`的`commit id`(版本号), 和SVN不一样, git的`commit id`不是1, 2, 3......递增的数字, 而是一个SHA1计算出来的一个非常大的数字, 用十六进制表示, 而且你看到的commit id和我的肯定不一样, 以你自己的为准. 为什么`commit id`需要用这么一大串数字表示呢? 因为git是分布式的版本控制系统, 后面我们还要研究多人在同一个版本里工作, 如果大家都用1, 2, 3......作为版本号, 那肯定就冲突了.

每提交一个新的版本, 实际上git就会把它们自动串成一条线. 如果使用可视化工具查看git历史, 就可以更清楚地看到提交历史的时间线:

![](./pic/version_line.png)

好了, 现在我们启动时光穿随机, 准备把`readme.txt`回退到上一个版本, 也就是`add distributed`的那个版本, 怎么做呢?

首先, git必须知道当前版本是哪个版本, 在git中, 用`HEAD`表示当前版本, 也就是最新的提交`1094adb...`(注意我的提交ID和你的肯定不一样), 上一个版本就是`HEAD^`, 上上一个版本就是`HEAD^^`, 当然往上100个版本写100个`^`比较容易数不过来, 所以写成`HEAD~100`.

现在, 我们要把当前版本`append GPL`会退到上一个版本`add distributed`, 就可以使用`git reset`命令:

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

`--hard`参数有啥意义? 这个后面再讲, 现在你先放心使用.

看看`readme.txt`的内容是不是版本`add distributed`:

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```

果然被还原了.

还可以继续会退到上一个版本`wrote a readme file`, 不过且慢, 让我们用`git log`再看看现在版本库中的状态:

```
$ git log
commit e475afc93c209a690c39c13a46716e8fa000c366 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

最新的那个版本`append GPL`已经看不到了! 好比你从21世纪坐时光穿梭机来到了19世纪, 想再回去已经回不去了, 肿么办?

办法其实是有的, 只要上面的命令行窗口还没有被关掉, 你就可以顺着往上找啊找啊, 找到那个`append GPL`的`commit id`是`1094adb...`, 于是就可以指定回到未来的某个版本:

```
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

版本号没必要写全, 前面几位就可以了, git会自动去找. 当然也不能只写前一两位, 因为git可能会找到多个版本号, 就无法确定是哪一个了.

再小心翼翼地看看`readme.txt`的内容:

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

果然, 我胡汉三又回来了.

git的版本回退速度非常快, 因为git在内部有个指向当前版本的HEAD指针, 当你回退版本的时候, git仅仅是把HEAD从指向`append GPL`:

```
┌────┐
│HEAD│
└────┘
   │
   └──> ○ append GPL
        │
        ○ add distributed
        │
        ○ wrote a readme file
```

改为指向`add distributed`:

```
┌────┐
│HEAD│
└────┘
   │
   │    ○ append GPL
   │    │
   └──> ○ add distributed
        │
        ○ wrote a readme file
```

然后顺便把工作区的文件更新了. 所以你让HEAD指向哪个版本号, 你就把当前版本定位在哪.

现在, 你回退到了某个版本, 关掉了电脑, 第二天早上就后悔了, 想恢复到新版本怎么办? 找不到新版本的commit id怎么办?

在git中, 总是有后悔药可以吃的. 当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时, 再想恢复到`append GPL`, 就必须找到`append GPL`的`commit id`. git提供了一个命令`git reflog`用来记录你的每一次命令:

```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

终于舒了口气, 从输出可知, `append GPL`的`commit id`是`1094adb`, 现在, 你又可以乘坐时光机回到未来了.

小结:

- `HEAD`指向的版本就是当前版本, 因此, git允许我们在版本的历史之间穿梭, 使用命令`git reset --hard commit_id`.
- 穿梭前, 用`git log`可以查看提交历史, 以便确定要回退到哪个版本.
- 要重返未来, 用`git reflog`查看命令历史, 以便确定要回到未来的哪个版本.


### 2.2 工作区和暂存区

git和其他版本控制系统如SVN的一个不同之处就是暂存区的概念.

先来看看名称解释.

- 工作区(Working Directionary)

就是你在电脑里能看到的目录, 比如我的learngit文件夹就是一个工作区:

![](./pic/working_directory.png)

- 版本库(Repository)

工作区有一个隐藏目录`.git`, 这个不算工作区, 而是git的版本库.

git的版本库里面存了很多东西, 其中最重要的就是称为stage(或者叫index)的暂存区, 还有git为我们自动创建的第一个分支`master`, 以及指向`master`的一个指针叫`HEAD`.

![](./pic/repository.png)

分支和`HEAD`的概念我们以后再讲.

前面讲了我们把文件往git版本库里添加的时候, 是分两步执行的:

第一步是用`git add`把文件添加进去, 实际上就是把文件修改添加到暂存区;

第二步是用`git commit`提交更改, 实际上就是把暂存区的所有内容提交到当前分支.

因为我们创建`git`版本库时, git自动为我们创建了唯一一个`master`分支, 所以, 现在`git commit`就是往`master`分支上提交更改.

你可以简单理解为, 需要提交的文件修改通通放到暂存区, 然后, 一次性提交暂存区的所有修改.

俗话说, 实践出真知. 现在, 我们再练习一遍, 先对`readme.txt`做个修改, 比如加上一行内容:

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
```
然后, 在工作区新增一个`LICENSE`文本文件(内容随便写).

先用`git status`查看一下状态:

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
```

git非常清楚地告诉我们, `readme.txt`被修改了, 而`LICENSE`从来没有被添加过, 所以它的状态是`Untracked`.

现在, 使用两次命令`git add`, 把`readme.txt`和`LICENSE`都添加后, 用`git status`再查看一下:

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   LICENSE
	modified:   readme.txt
```
现在, 暂存区的状态就变成这样了:

![](./pic/git_add.png)

所以, `git add`命令实际上就是把要提交的所有修改放到暂存区(Stage), 然后, 执行`git commit`就可以一次性把暂存区的所有修改提交到分支.

```
$ git commit -m "understand how stage works"
[master e43a48b] understand how stage works
 2 files changed, 2 insertions(+)
 create mode 100644 LICENSE
```

一旦提交后, 如果你又没有对工作区做任何修改, 那么工作区就是"干净"的:

```
$ git status
On branch master
nothing to commit, working tree clean
```

现在版本库变成了这样, 暂存区就没有任何内容了:

![](./pic/git_commit.png)

- 小结

暂存区是git非常重要的概念, 弄明白了暂存区, 就弄明白了git的很多操作到底在干什么.

### 2.3 管理修改

现在, 假定你已经完全掌握了暂存区的概念. 下面, 我们要讨论的就是, 为什么git比其他版本控制系统设计得优秀, 因为git跟踪并管理的是修改, 而非文件.

你会问, 什么是修改? 比如你新增了一行, 这就是一个修改, 删除了一行, 也是一个修改, 更改了某些字符, 也是一个修改, 删了一些又加了一些, 也是一个修改, 甚至创建一个新文件, 也算一个修改. 为什么说git管理的是修改, 而不是文件呢? 我们还是做实验. 第一步, 对readme.txt做一些修改, 比如加一行内容:

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
```
然后, 添加:

```
$ git add readme.txt
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   readme.txt
#
```

## 3. 远程仓库

## 4. 分支管理

## 5. 标签管理

## 6. 自定义git

## 7. 总结

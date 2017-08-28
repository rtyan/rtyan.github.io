---
layout: post
title:  GIT中需要知道的内容
date:   2017-08-14 18:56:22
categories: git
---
#### GIT中的stash
当我们在一个分支上正在开发完善功能A的时候，有需要修改功能A之前已经提交的一个bug，正在
开发的功能A的内容还未完成不能提交，这时候怎么办呢？
这时候就可以使用`git stash`命令，将正在开发的内容暂存起来(就像警察处理案件时候的保留
现场)，去开发修复bug，等bug提交之后再将功能A的现场恢复出来继续开发，就好像什么也没发生
过。看看下面的例子

#### GIT中的fetch
或许大多数人跟我一样经常使用`git pull`，很少使用`git fetch`，可能也搞不清楚这两个到底有啥区别。
`git fetch`其实就是把远程分支的内容拉取到本地，并保存到本地远程分支，不会主动合并，另外这里会将
远程分支的定点信息保存到`.git/FETCH_HEAD`文件中，看个例子：

项目为`tools`
我们首先在A目录下的what分支提交一次，提交之后通过`git log what --graph`看提交信息如下
```
* commit 7668fbfa5b6c9ac7e1f6f1506185877542a0e3bf
| Author: ruitao <ruitao@staff.weibo.com>
| Date:   Mon Aug 14 19:35:24 2017 +0800
|
|     测试fetch
|
* commit 09fb1f590df112cf49e3e5a1050b8e6bd7d5b043
| Author: ruitao <ruitao@staff.weibo.com>
| Date:   Fri Aug 11 19:20:25 2017 +0800
|
|     what test
|
* commit 5e1e048af8e49bcd11aa741e3b8c33256e2a9d3d
| Author: ruitao <ruitao@staff.weibo.com>
| Date:   Fri Aug 11 19:19:40 2017 +0800
|
|     what test
```
然后在B目录下通过`git fetch`拉取所有远程分支到本地，并记录`FETCH_HEAD`信息，执行完之后通过
`cat .git/FETCH_HEAD`，可以看到本地保存的远程分支顶点的信息，可以看到`what`分支的记录和另外
一个目录提交的最新信息一致
```
5f9bf3258788aae0edb51835fb4cff6fadc6fc05        branch 'master' of http://git.intra.weibo.com/ruitao/tools
998e06cb0d84fb6a0a514d030465eba603f5b8a7    not-for-merge   branch 'test-dev' of http://git.intra.weibo.com/ruitao/tools
7668fbfa5b6c9ac7e1f6f1506185877542a0e3bf    not-for-merge   branch 'what' of http://git.intra.weibo.com/ruitao/tools
```
除此之外，GIT中还有个关键字`FETCH_HEAD`，表示fetch下来的最新内容，当制定分支的时候就是制定分支
的最新内容，不指定分支拉取所有内容，`FETCH_HEAD`执行`master`分支的最新提交，也就是上面文件中的
第一个。

上面的状态执行`git diff FETCH_HEAD`之后发现内容为空，因为远程master未修改，执行`git diff origin/what`
之后，可以看到当前`master`分支和`origin/what`分支的区别为D1。

我们执行`git fetch origin what`之后，看到`cat .git/FETCH_HEAD`的内容为
```
7668fbfa5b6c9ac7e1f6f1506185877542a0e3bf        branch 'what' of http://git.intra.weibo.com/ruitao/tools
```
只有`what`分支的最新提交信息，`git diff FETCH_HEAD`之后也会显示区别问D1，因为这样`FETCH_HEAD`
指针指向了`what`分支的最新提交的定点。

#### git中的diff
git 中的diff分为以下几种情况：
```sh
git diff;
git diff --staged;
git diff --cached;
git diff HEAD
```
要理解这几个之间的区别，首先要了解git的几个工作区。
git中文件所处的状态可以分为3中：工作区、暂存区、提交版本。
git中为什么要改这么多的状态，`add`之后还要一次`commit`才能提交到版本库，这个是
因为git期初没有界面，添加一个暂存区，就类似于svn的GUI选择提交界面。更形象的表示
就是，暂存区就像一个购物车，想要的东西都放进去，最后付款的可能不是全部商品。

##### git diff
`git diff`是工作区和暂存区之间的比较，看下面的例子：

修改文件A，添加一行内容`test a file`，然后`git add A`，此时文件A进入暂存区。

再次修改文件A，添加一行内容`test two file`，此时运行`git status`，结果如下。

```
➜  tmp git:(master) ✗ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.md
```

运行`git diff`，可以看到你内容如下，可以看到工作区确实比暂存区多了一行
`test two file`

```
diff --git a/readme.md b/readme.md
index 2708cc2..871c1b7 100644
--- a/readme.md
+++ b/readme.md
@@ -1,2 +1,3 @@
 ### test
 test a file
+test two file
```

##### git diff --staged | git diff --cached
这两个命令是同一个意思，都是比较暂存区和提交版本的比较，就像上面那几个步骤，之后
执行`git diff --staged`或者`git diff --cached`，返回结果如下，可以看到，确实比
提交版本库多了一行`test a file`。
```
diff --git a/readme.md b/readme.md
index 071da0e..2708cc2 100644
--- a/readme.md
+++ b/readme.md
@@ -1 +1,2 @@
 ### test
+test a file
```

#####  git diff HEAD
这里适合制定版本比较，HEAD指向的数本地提交版本库的定点，所以沿着上面的例子，
继续执行`git diff HEAD`,不出意外，可以看到下面的结果，多加了两行内容：`test
 a file`和`test two file`：
```
diff --git a/readme.md b/readme.md
index 071da0e..871c1b7 100644
--- a/readme.md
+++ b/readme.md
@@ -1 +1,3 @@
 ### test
+test a file
+test two file
```

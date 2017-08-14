---
layout: post
title:  GIT中需要知道的内容
date:   2017-08-14 18:56:22
categories: git
---
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

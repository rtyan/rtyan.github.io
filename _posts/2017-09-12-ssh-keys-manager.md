---
layout: post
title:  ssh keys管理
date:   2017-09-12 21:23:35
categories: 工具
---

#### 引言
我有两个github账户，一个是平时正常使用的，另外一个是用来专门做博客用的，
因为之前常用的那个做博客名字不好看o(╯□╰)o。

这就引发了一个问题，我想在博客账户中添加`ssh keys`的时候，github会提示
我下面的信息

![Key is already in use]({{ site.url }}/assets/images/posts/ssh-keys-duplicate.png)

原来，同一个公钥只能在github系统中添加一次，重复添加的话会报这个错误，就算
是不同的账户也不能将同一个公钥添加多次。

这个问题要怎么解决呢？

#### 暴力法——重新生成
最简单的方法，也就是直接生成一套新的公钥密钥对，然后添加到我的博客账户中，
等下次再用常用的账号时候再重新生成一套，再替换常用账号中的`ssh keys`就可以
了，事实上我第一次就是这么干的，没完全弄清楚怎么回事，只能按照流程重新生成
一遍了，而且确实生效了。

但用了几次之后我发现不用重新生成、只要在本地生成两份`ssh keys`，然后每次讲要
使用的`ssh keys`命名为`id_rsa`、`id_rsa.pub`就可以了，不用每次替换账号中的配置了。

但用了一段时间，发现还是很麻烦，是在太难受了，于是就想着写一个半
自动化的创建、切换`ssh keys`的工具。

#### 半自动化法
沿着上面的思路，我可以生成多套`ssh keys`，然后将他们管理起来，每次使用的时候
只需要将他们替换到`id_rsa`、`id_rsa.pub`就可以了，省略了人工去切换。就像下面
这样：
```shell
➜  rtyan.github.io git:(master) git push origin master
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
➜  rtyan.github.io git:(master) sshkeys change rtyan
切换ssh keys对，从[aizuyan]到[rtyan]...
复制/Users/ritoyan/.ssh-keys-pair/rtyan到/Users/ritoyan/.ssh/id_rsa成功
复制/Users/ritoyan/.ssh-keys-pair/rtyan.pub到/Users/ritoyan/.ssh/id_rsa.pub成功
切换到rtyan成功
➜  rtyan.github.io git:(master) git push origin master
Counting objects: 8, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (8/8), done.
Writing objects: 100% (8/8), 77.54 KiB | 0 bytes/s, done.
Total 8 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), completed with 5 local objects.
To github.com:rtyan/rtyan.github.io.git
   a05e55f..50b54a7  master -> master
```

目前已经开发完成，用shell完成的，最后面会将代码贴出来。

工具生成的所有`ssh keys pairs`都保存在`~/.ssh-keys-pair/`目录下，就下下面
这样：

```shell
-rw-------  1 ritoyan  staff  3243  9 12 21:15 backup
-rw-r--r--  1 ritoyan  staff   741  9 12 21:15 backup.pub
-rw-r--r--  1 ritoyan  staff     6  9 12 21:16 nowRecord
-rw-------  1 ritoyan  staff  3243  9 12 21:15 rtyan
-rw-r--r--  1 ritoyan  staff   741  9 12 21:15 rtyan.pub
```

其中`backup`、`backup.pub`记录了软件安装之前的`ssh keys`信息（如果有的话）。

`nowRecord`记录了当前使用的是哪个`ssh keys`，里面的内容等同于`~/.ssh-keys-pair/`
目录下的密钥名称。

其他文件的话就是公钥密钥对了，密钥是<生成时候的名称>，公钥是<生成时候的名称>.pub

列出所有`ssh keys`列表的时候就是通过匹配`~/.ssh-keys-pair/`文件夹下面的`.pub`
后缀来的，匹配上了就算是一个公钥密钥对。

#### 安装
安装的话直接把最下面的shell代码复制到`/usr/local/bin/sshkeys`中（或其他`$PATH`
目录中），然后执行`chmod +x /usr/local/bin/sshkeys`给脚本加上可执行权限，然后
就可以看命令使用了。

#### 命令
这个工具一共5个命令，功能分别如下（也可以直接看代码）：

>`sshkeys init`

初始化环境，备份原来的`ssh keys`到`backup`，设置当前使用的`ssh keys`名称为`backup`

>`sshkeys list`

列出当前所有的`ssh keys`名称，当前使用的前面有星号：

```shell
➜  GIT sshkeys list
  aizuyan
  backup
* rtyan
```

>`sshkeys create <name> [<email>]`

上面的命令是创建一个名称为`<name>`的`ssh keys`，后面的`<email>`是可选项：

```shell
➜  GIT sshkeys create aizuyan ritoyan@163.com
开始创建密钥公钥对...
执行命令[ssh-keygen -t rsa -b 4096 -f /Users/ritoyan/.ssh-keys-pair/aizuyan -q -N "" -C "ritoyan@163.com"]
创建成功
```

>`sshkeys showpub [<name>]`

上面的命令是展示公钥，后面的`<name>`是可选的，默认展示当前正在使用的，加了参数显示
对应`<name>`的公钥：

```shell
➜  GIT sshkeys showpub aizuyan
ssh-rsa AA.........Vw== ritoyan@163.com
```

>`sshkeys change <name>`

切换到对应`<name>`的`ssh keys`

```shell
➜  GIT sshkeys change aizuyan
切换ssh keys对，从[rtyan]到[aizuyan]...
复制/Users/ritoyan/.ssh-keys-pair/aizuyan到/Users/ritoyan/.ssh/id_rsa成功
复制/Users/ritoyan/.ssh-keys-pair/aizuyan.pub到/Users/ritoyan/.ssh/id_rsa.pub成功
切换到aizuyan成功
```

#### 代码
代码又长又臭，可以忽略过了，想看的看下。
还有很多地方要优化，不过现在我用是够了，哈哈。

```shell
#/bin/bash

# 存放数据的根目录
softBase="${HOME}/.ssh-keys-pair/"
sshBase="${HOME}/.ssh/"
sshIdRsaPath="${sshBase}id_rsa"
sshIdRsaPubPath="${sshBase}id_rsa.pub"
nowSshPairRecordPath="${softBase}nowRecord"

action=$1

function init()
{
	# 判断根目录是否存在，不存在创建
	if [ -d $softBase ];then
		return 1
	fi
	echo "初始化..."

	#[1] 创建目录
	echo "[1]创建必要的目录..."
	mkdir -p -m 700 $softBase
	if [ $? != 0 ];then
		echo "[1-1]创建目录${softBase}失败"
	else
		echo "[1-1]创建目录${softBase}成功"
	fi

	#[2] 备份现在的sshkey
	echo "[2]备份当前的ssh key pair"
	if [ -f "${sshIdRsaPath}" ];then
		cp $sshIdRsaPath "${softBase}backup"
		if [ $? != 0 ];then
			echo "[2-1]备份${sshIdRsaPath}到${softBase}backup"
		else
			echo "[2-1]备份${sshIdRsaPath}到${softBase}backup"
		fi
	else
		echo "${sshIdRsaPath}文件不存在，不需要备份"
	fi
	if [ -f "${sshIdRsaPubPath}" ];then
		cp $sshIdRsaPubPath "${softBase}backup.pub"
		if [ $? != 0 ];then
			echo "[2-2]备份${sshIdRsaPubPath}到${softBase}backup.pub失败"
		else
			echo "[2-2]备份${sshIdRsaPubPath}到${softBase}backup.pub成功"
		fi
	else
		echo "${sshIdRsaPubPath}文件不存在，不需要备份"
	fi

	#[3] 设置当前sshkeys
	echo "[3]设置当前sshkeys记录"
	if [ -f "${sshIdRsaPubPath}" ];then
		echo "backup" > $nowSshPairRecordPath
		if [ $? != 0 ];then
			echo "[3-1]设置当前sshkeys记录为[backup]，写入${nowSshPairRecordPath}失败"
		else
			echo "[3-1]设置当前sshkeys记录为[backup]，写入${nowSshPairRecordPath}成功"
		fi
	fi

	echo "初始化成功\n"
	return 0
}

function nowRecord()
{
	local nowRecord=`cat ${nowSshPairRecordPath}`
	echo $nowRecord
	return 0
}

# 列出所有的公钥名称
function list()
{
	local files=`ls $softBase | grep "\.pub\$"`
	local file
	local nowRecord=`nowRecord`
	for file in ${files}
	do
		sshKeysName=${file%%.pub}
		if [[ $nowRecord == $sshKeysName ]]; then
			echo "* "$sshKeysName
		else
			echo "  "$sshKeysName
		fi
	done
}

# 创建一对新的密钥
# $1 密钥名称  必选
# $2 邮件 可选
function create()
{
	keyPairName=$1
	maybeEmail=$2
	createPath="${softBase}${keyPairName}"
	cmd="ssh-keygen -t rsa -b 4096 -f ${createPath} -q -N \"\""
	if [[ $maybeEmail ]]; then
		cmd="${cmd} -C \"${maybeEmail}\""
	fi
	echo "开始创建密钥公钥对..."
	echo "执行命令[${cmd}]"
	eval $cmd
	if [ $? != 0 ];then
		echo "创建失败"
	else
		echo "创建成功"
	fi
}

# 切换ssh keys
function change()
{
	local keyPairName=$1
	local rsaPath="${softBase}${keyPairName}"
	local rsaPubPath="${rsaPath}.pub"
	local preSshPairRecord=`nowRecord`
	echo "切换ssh keys对，从[${preSshPairRecord}]到[${keyPairName}]..."
	cp $rsaPath $sshIdRsaPath
	if [ $? != 0 ];then
		echo "复制${rsaPath}到${sshIdRsaPath}失败"
	else
		echo "复制${rsaPath}到${sshIdRsaPath}成功"
	fi
	cp $rsaPubPath $sshIdRsaPubPath
	if [ $? != 0 ];then
		echo "复制${rsaPubPath}到${sshIdRsaPubPath}失败"
	else
		echo "复制${rsaPubPath}到${sshIdRsaPubPath}成功"
	fi
	echo "${keyPairName}" > $nowSshPairRecordPath
	if [ $? != 0 ];then
		echo "切换到${keyPairName}失败"
	else
		echo "切换到${keyPairName}成功"
	fi
}

function showpub()
{
	local keyPairName=$1
	if [ -z "${keyPairName}" ]; then
		keyPairName=`nowRecord`
	fi
	local keyPubPath="${softBase}${keyPairName}.pub"
	cat $keyPubPath
}

case "${action}" in
	"init" )
		init
		;;
	"list" )
		list
		;;
	"create" )
		keyPairName=$2
		maybeEmail=$3
		create "${keyPairName}" "${maybeEmail}"
		;;
	"change" )
		keyPairName=$2
		change "${keyPairName}"
		;;
	"showpub" )
		keyPairName=$2
		showpub "${keyPairName}"
		;;
	* )
		echo "this is action none"
		;;
esac
```





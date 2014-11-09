---
layout: post
title: "阅读笔记，2014年11月"
description: ""
category: Reading Notes
tags: [reading, notes]
---
{% include JB/setup %}

## 目录 <!-- CONTENTS -->

* 这回生成一个目录，且这行文字会被忽略
{:toc}

## 05 Nov {#nov05}

### I. 手工安装CTAN的宏包 {#Install-CTAN-Manually}
参考文献：<http://tex.stackexchange.com/a/73017/65422>

我在使用Raspberry Pi的时候发现TeX Live的CTAN宏包版本都比较旧，并且软件源中也没有tlmgr工具，所以使用有些宏包时需要手动安装。

安装目录：

* 所有用户：使用`kpsewhich -var-value TEXMFLOCAL`可以得到为整个系统安装的路径，一般为`/usr/local/share/texmf`。
* 当前用户，使用`kpsewhich -var-value TEXMFHOME`可以得到为当前用户安装的路径，一般为`~/texmf`。
* 上面的路径我们称为`<base dir>`。

将下载的宏包解压到`<base dir>/tex/latex/`目录中，此时在`<base dir>/tex/latex/<package name>`目录中应该是相应的`.sty`和`.cls`文件；可选地，文档解压到`<base dir>/doc/latex/`。然后执行`mktexlsr`，为所有用户安装时需要`sudo`权限。

## 06 Nov {#nov06}

### II. 在树莓派上WHEEZY上安装GCC 4.8
原文：[GCC 4.8 ON RASPBERRY PI WHEEZY](http://somewideopenspace.wordpress.com/2014/02/28/gcc-4-8-on-raspberry-pi-wheezy/)

树莓派当前源中的软件一般都比较旧，所以如果要使用较新版本的软件的话需要从测试版仓库中安装。目前，稳定版仓库代号叫WHEEZY，测试版仓库代号是JESSIE。我们可以将`/etc/apt/sources.list`中的`wheezy`全部改为`jessie`；也可以同时保留两个，然后再安装的时候选择。如保留两个，大致流程如下：

编辑`/etc/apt/sources.list`：

~~~
deb http://mirrordirector.raspbian.org/raspbian/ wheezy main contrib non-free rpi
deb http://archive.raspbian.org/raspbian wheezy main contrib non-free rpi
# Source repository to add
deb-src http://archive.raspbian.org/raspbian wheezy main contrib non-free rpi
deb http://mirrordirector.raspbian.org/raspbian/ jessie main contrib non-free rpi
deb http://archive.raspbian.org/raspbian jessie main contrib non-free rpi
# Source repository to add
deb-src http://archive.raspbian.org/raspbian jessie main contrib non-free rpi
~~~

编辑（新建）`/etc/apt/preferences`：

~~~
Package: *
Pin: release n=wheezy
Pin-Priority: 900
Package: *
Pin: release n=jessie
Pin-Priority: 300
Package: *
Pin: release o=Raspbian
Pin-Priority: -10
~~~

然后执行：

~~~ bash
sudo apt-get update
~~~

以安装GCC 4.8 为例，执行：

~~~ bash
sudo apt-get install -t jessie gcc-4.8 g++-4.8
~~~

安装完了之后默认的gcc版本并不是新安装的这个，所以还需执行如下命令以添加alternative configuration：

~~~ bash
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.6 20
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 50
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.6 20
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 50
~~~

如果你的gcc/g++已经有了alternative configuration（默认是没有的），在执行上面的命令之前应当先删除：

~~~ bash
sudo update-alternatives --remove-all gcc 
sudo update-alternatives --remove-all g++
~~~

执行如下如下命令可以修改当前alternative configuration：

~~~ bash
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
~~~

## 09 Nov {#nov09}

### III. 一个成功的Git分支模型 {#a-successful-git-branching-model}
* 原文：[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)
* 作者：[Vincent Driessen](http://nvie.com/about/)，on Tuesday, January 05, 2010

本文介绍的版本控制系统Git的一种使用流程，值得一读。本文有多个中文翻译版本。

本文建议在开发中使用如下分支：master，develop，release，feature-xxx，hotfix。同时介绍了Bump Version脚本，以及在merge时使用`--no-ff`选项。



---
layout: post
title: "阅读笔记，2014年11月"
description: ""
category: Reading Notes
tags: [reading, notes]
---
{% include JB/setup %}

## 05 Nov {#nov05}

### I. 手工安装CTAN的宏包 {#Install-CTAN-Manually}
参考文献：<http://tex.stackexchange.com/a/73017/65422>

我在使用Raspberry Pi的时候发现TeX Live的CTAN宏包版本都比较旧，并且软件源中也没有tlmgr工具，所以使用有些宏包时需要手动安装。

安装目录：
* 所有用户：使用`kpsewhich -var-value TEXMFLOCAL`可以得到为整个系统安装的路径，一般为`/usr/local/share/texmf`。
* 当前用户，使用`kpsewhich -var-value TEXMFHOME`可以得到为当前用户安装的路径，一般为`~/texmf`。
* 上面的路径我们称为`<base dir>`。

将下载的宏包解压到`<base dir>/tex/latex/`目录中，此时在`<base dir>/tex/latex/<package name>`目录中应该是相应的`.sty`和`.cls`文件；可选地，文档解压到`<base dir>/doc/latex/`。然后执行`mktexlsr`，为所有用户安装时需要`sudo`权限。

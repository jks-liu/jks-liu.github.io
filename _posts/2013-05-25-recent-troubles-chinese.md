---
layout: post
title: "Recent Troubles (Chinese)"
description: ""
category: Essay
tags: []
tagline: 最近几件不顺心的事
---
{% include JB/setup %}

# 最近几件不顺心的事

## 关于 Tex
### 博客
最近在写论文， 说实话， 特别不爱写论文（不是文章， 特指论文）。 

其实我一直认为我有一个当编辑的潜质。 比如， 我偶尔会写一些博客啥啥的。
并且， 我写的时候从来都是追求完美， 虽然我的文笔不怎么样。 
我用 QQ 空间写过博客， 我用新浪博客写过博客， 我用 Wordpress 写过博客。 
最近， 我又转战到 github pages 的阵营。 

有点扯远了。

### 论文
之所以不喜欢写论文， 我想， 可能有以下几个方面的原因：

- **字数限制** 我又不是小学生
- **综述** 自认为没有写此等文章的水平
- **格式 0** 说实话， 我打开自己电脑上 Office 的此时可以用脚趾头数过来
- **格式 1** 再说句实话， 那个什么小四， 三线表， 小三真是太麻烦了

所以， 我做出了一个非常艰难的决定 \\cite\{QQ2010\}, 使用 \\LaTeX\{\}
来写论文， 最后在复制粘贴到 Word 里， 进行格式化。 说句不谦虚的话， 我是用
tex 还是很熟练的。 选择 latex， 主要有以下几个原因： 
> 搞技术的总能把道理说出个一二三四来。
>
>  ..................................................(小师姐)

- 可以方便的管理文献
- 可以方便编辑公式（当然， 我的论文没有公式）
- TikZ & Pgf 太强大了
- 跨平台
- 当然， 除了这些优点， 其他的全是坑。

于是我要做的就是：
> “一遍pdflatex，一遍bibtex，再一遍pdflatex，再一遍pdflatex”
>（懂我在说什么吗？不懂的话你功力不够，需要继续修炼TeX神功）
>
> ......................................([yihui: 自动化报告](https://github.com/yihui/r-ninja/blob/master/11-auto-report.md))

当然了， 正如王垠所说：
> [学术圈的很多人由于受到某种错误思想的“洗脑”](http://www.yinwang.org/blog-cn/2012/09/18/texmacs/)。

我毅然写下了如下的 Makefile：
{% highlight makefile %}
$(TNAME).pdf: $(TNAME).tex $(BIB)
	rm -f *.pdf
	pdflatex -shell-escape $^
	bibtex $(TNAME)
	pdflatex -shell-escape $^
	pdflatex -shell-escape $^
{% endhighlight %}

当然了， 既然谢益辉的那篇*自动化报告*提到 Lyx 神器， 
我自然就要尝试一下了， 由于论文要写 Word， 所以我使用的是实验室的
Win7 系统。 可能是人品的关系， 反正就是各种原因， Lyx 安装失败。
最坑的是， 原本可以编译通过的 tex 文件， 现在无法编译了（我有版本控制，
所以绝对不是我改了源文件， 并且在我的 Ubuntu 上没有问题），
当然了， 我是从来不仔细看 tex 的出错信息的。 折腾了好久，
未果。 于是只能使用最暴力的方法了， 卸载 MikTex， 清理注册表， 
重新安装 MikTex。 果然不出我所料， 问题自动消失。

总之， LaTex 拥有无数的坑， 当遇到中文的时候， 坑的数量加倍， 当在 Windows
上使用的时候， 再加倍。

现在发现， 一个工具， 要么是用的不顺手， 如 Latex， Word；
不够强大， 如 MarkDown， Org-Mode。 要么是网络不给力， 如 google drivers。

所以， 我又做了一个艰难的决定： 学习 RFC， 实行纯文本。 
（我是不是该把这篇随笔的格式都清除了？）

## 关于德州仪器 (TI)
曾经花 4.99 刀买了 TI 的 Stellaris® LM4F120 LaunchPad Evaluation Board.
使用的是 LX4F120H5QR, 当然了， 人家其实叫 LM， 不叫什么LX。
当然了， 人家现在也不叫 LM 了， 叫 TM4C1233H6PM。 人家不仅改了名， 连户口也销了，
连祖宗的姓氏也由 Stellaris® 改成 Tiva™ 。 难道， 它被 GOV wanted 了？

以前一直认为 TI 是个好公司， 直到膝盖中了一箭。

以前一直认为 Google 是个好公司， 直到， 它关了 Reader 。

## 最后， 祝大家 工作， 毕业， 读研， 出国 顺利。



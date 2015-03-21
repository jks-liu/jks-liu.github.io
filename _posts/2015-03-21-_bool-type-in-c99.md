---
layout: post
title: "C99中的_Bool类型"
description: ""
category: Manual
tags: [C语言, C99]
---
{% include JB/setup %}

在读*SDCC Compiler User Guide*的时候读到下面一段：

> `bit` and `sbit` types now consistently behave like the C99 `_Bool` type with respect to type conversion. The most
> common incompatibility resulting from this change is related to bit toggling idioms, e.g.:
>
> ~~~ c
> bit b;
> b = ~b; /* equivalent to b=1 instead of toggling b */
> b = !b; /* toggles b */
> ~~~
>
> In previous versions, both forms would have toggled the bit.

一直不明白说的什么意思，今天终于茅塞顿开。

首先，我们需要知道以下事实：

1. 任何非零的值赋值给`_Bool`类型的变量`b`，`b`都会得到值1，而不会像其它`unsigned`类型的变量一样截取；
2. 一元操作符`~`会对其操作数进行整数提升。

所以，即使`b`的值是1，`~b`首先会对`b`进行整数提升，再取反，结果肯定是一个非零值，在赋值给`b`，最终`b`的值还是1。

---
layout: post
title: "C语言宏进阶"
description: ""
category: C语言
tags: [C宏]
---
{% include JB/setup %}

## 全局代码优化的宏

~~~ C
#ifndef ROM
#define ROM
#endif

#if (__STDC_VERSION__ < 199901L) && !defined(inline)
#define inline
#endif

#ifndef __attribute__
#define __attribute__(x)
#endif
~~~

这三个是常用的用于提升程序性能的宏，其一般不会改变程序的行为，但不是所有的编译器都支持。所以，如果你的代码被用在多个地方，使用上面的宏可以帮助你的代码更具有可移植性。如果你的编译器不支持上门的宏，直接忽略就行，如果支持的话，可以按照下面的定义在编译选项中定义这些宏：

~~~ C
#define ROM ROM
#define inline inline
#define __attribute__ __attribute__
~~~

## 记号粘合

~~~ C
#define CONCATENATE(x, y) x##y
~~~

对吗？有问题。如：

~~~ C
#define ID 1120
CONCATENATE(Hello, ID)
~~~

会得到什么？`Hello1120`？不是。是`HelloID`。那么怎样实现`CONCATENATE`？

~~~ C
#define CONCATENATE_(x, y) x##y
#define CONCATENATE(x, y) CONCATENATE_(x, y)
~~~

## 字符串化

字符串化也有和记号粘合同样的问题。

~~~ C
#define STRING_(x) #x
#define STRING(x) STRING_(x)
~~~

以上*记号粘合*和*字符串化*的问题，本质上是由于[Argument Prescan](https://gcc.gnu.org/onlinedocs/cpp/Argument-Prescan.html)导致的。

## Bit Mask

当我们需要Mask或者Un-mask一个比特的时候，我们会写：

~~~ C
x |= 1 << 5; /* Mask bit 5 */
x &= ~(1 << 5); /* Un-mask bit 5 */
~~~

所以，我们会定义这样一个宏：

~~~ C
#define BIT(x) (1 << (x))
~~~

但问题是，我们不能保证当`x`大于16时的正确性。对于某些嵌入式系统，甚至不能保证当`x`大于8时的正确性，8位的潜入式系统在默认状态下往往有和标准不兼容的优化。

所以，如果是作为一个库的话，我们不介意库本身复杂一点，只有其使用简单，对于嵌入式系统可以生成紧凑的代码就是OK的。

~~~ C
#define BIT0 0x01
#define BIT1 0x02
#define BIT2 0x04
#define BIT3 0x08
#define BIT4 0x10
#define BIT5 0x20
#define BIT6 0x40
#define BIT7 0x80
#define BIT_BYTE(x) (1 << (x))

#define BIT8 0x0100u
#define BIT9 0x0200u
#define BIT10 0x0400u
#define BIT11 0x0800u
#define BIT12 0x1000u
#define BIT13 0x2000u
#define BIT14 0x4000u
#define BIT15 0x8000u
#define BIT_U(x) (1u << (x))

#define BIT16 0x00010000ul
#define BIT17 0x00020000ul
#define BIT18 0x00040000ul
#define BIT19 0x00080000ul
#define BIT20 0x00100000ul
#define BIT21 0x00200000ul
#define BIT22 0x00400000ul
#define BIT23 0x00800000ul
#define BIT24 0x01000000ul
#define BIT25 0x02000000ul
#define BIT26 0x04000000ul
#define BIT27 0x08000000ul
#define BIT28 0x10000000ul
#define BIT29 0x20000000ul
#define BIT30 0x40000000ul
#define BIT31 0x80000000ul
#define BIT_UL(x) (1ul << (x))

#if (__STDC_VERSION__ >= 199901L) /* C99 */
#define BIT32 0x0000000100000000ull
#define BIT33 0x0000000200000000ull
#define BIT34 0x0000000400000000ull
#define BIT35 0x0000000800000000ull
#define BIT36 0x0000001000000000ull
#define BIT37 0x0000002000000000ull
#define BIT38 0x0000004000000000ull
#define BIT39 0x0000008000000000ull
#define BIT40 0x0000010000000000ull
#define BIT41 0x0000020000000000ull
#define BIT42 0x0000040000000000ull
#define BIT43 0x0000080000000000ull
#define BIT44 0x0000100000000000ull
#define BIT45 0x0000200000000000ull
#define BIT46 0x0000400000000000ull
#define BIT47 0x0000800000000000ull
#define BIT48 0x0001000000000000ull
#define BIT49 0x0002000000000000ull
#define BIT50 0x0004000000000000ull
#define BIT51 0x0008000000000000ull
#define BIT52 0x0010000000000000ull
#define BIT53 0x0020000000000000ull
#define BIT54 0x0040000000000000ull
#define BIT55 0x0080000000000000ull
#define BIT56 0x0100000000000000ull
#define BIT57 0x0200000000000000ull
#define BIT58 0x0400000000000000ull
#define BIT59 0x0800000000000000ull
#define BIT60 0x1000000000000000ull
#define BIT61 0x2000000000000000ull
#define BIT62 0x4000000000000000ull
#define BIT63 0x8000000000000000ull
#define BIT_ULL(x) (1ull << (x))
#endif /* C99 */
~~~

## LOG库

本段参考了<http://bbs.21ic.com/icview-43224-1-1.html>。

由于C89不支持可变参数的宏，所以为了模拟可变参数的log，想了各种奇奇怪怪的方法。比如：

~~~ C
#ifdef NO_LOG
#define LOG /\
/
#else
#define LOG printf
#endif
~~~

经过测试，这种方法在某些编译器上不好用。更好的方法是使用上面的`CONCATENATE`：

~~~ C
#ifdef NO_LOG
#define LOG CONCATENATE(/, /)
#else
#include <stdio.h>
#define LOG printf
#endif
~~~

这种方法的缺点是，`LOG`只能写在一行。并且可能会有一些微妙的错误，如：

~~~ C
if (b)
    LOG("I Love Jks\n");

balabala;
~~~

如果关闭LOG，`balabala`就会悬挂到`if`下面。当然了，就当是敦促我们养成在所有的`if`中使用大括号的习惯吧。





C89不支持可变参数的宏，所以在写

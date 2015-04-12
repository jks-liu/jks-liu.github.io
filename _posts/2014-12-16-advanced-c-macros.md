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


## LOG库

本段参考了<http://bbs.21ic.com/icview-43224-1-1.html>。



C89不支持可变参数的宏，所以在写

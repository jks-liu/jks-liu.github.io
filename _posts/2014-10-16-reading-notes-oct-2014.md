---
layout: post
title: "阅读笔记，2014年10月"
description: ""
category: Reading Notes
tags: [reading, notes]
---
{% include JB/setup %}

## 16 Oct {#oct16}

### I. 什么样的软件值得信任 {#Reflections-on-Trusting-Trust}
原文：[Reflections on Trusting Trust](http://cm.bell-labs.com/who/ken/trust.html)

作者：Ken Thomps

本文通过self-reproducing程序引出编译器中的一种bug，通过某种方式可以让这个bug一直留在编译器中，即使我们在源代码中已经修改了这个错误。

所以，某个程序即使开放了源代码，也不应该完全信任它。除非其完全由你自己从头创建，包括其所有的构建工具，以及构建工具的构建工具，而在现实中这几乎是不可能的。


## 17 Oct {#oct17}

### II. 构建Web应用 {#The-Twelve-Factor-App}
原文：[The Twelve-Factor App](http://12factor.net/)

The twelve-factor app是一个构建SaaS（即Web应用）的方法论。

本文从1. 代码库，2. 依赖，3. 软件配置，4. 后端服务（如数据库），5. 构建，发布，运行，6. 进程，7. 端口绑定，8. 并发，9. 可处理性（稳健，快速运行，优雅地关闭），10. 开发与产品的等价，11. 日志，12. 管理过程，等十二个方面全面描述了构建SaaS的过程中应当注意的事项。

## 28 Oct {#oct28}

### 使用Clojure写Web应用 {#web-application-development-with-clojure}

作者：Vijay Kiran

全文分为5个部分：

* [第一部分](http://vijaykiran.com/2012/01/web-application-development-with-clojure-part-1/)讲解了如何建立新的工程。
* [第二部分](http://vijaykiran.com/2012/01/web-application-development-with-clojure-part-2/)介绍了数据库层。包括使用Lobos库莱建立数据库的Schema和Migrations。以及使用Korma来建立数据库的入口。
* [第三部分](http://vijaykiran.com/2012/01/web-application-development-with-clojure-part-3/)介绍了使用Fixture来建立数据库的测试数据，以及使用Enlive来建立Html模板。
* [第四部分](http://vijaykiran.com/2012/02/web-application-development-with-clojure-part-4/)结束了CSS的使用以及静态文件的部署。并结束了一个简单的认证逻辑。
* [第五部分](http://vijaykiran.com/2012/02/web-application-development-with-clojure-part-5/)简单介绍了管理员区域以及Session的使用。

其主要有以下几个原则:

* 使用一个VCS
* 不用隐含地依赖一个系统的包（或库）
* 将配置储存在环境变量中
* 不管是使用本地服务还是第三方服务（如数据库），代码应该是一样的
* 严格分离构建，发布和运行
* 进程应该是无状态的，且不分享任何东西
* 应用应当是自包含的
* 应用以绑定到端口的服务导出HTTP
* 进程是一等公民
* 进程可以随时的开启和关闭
* 进程的开启时间应该尽量的短
* 进程应当在接受到`SIGTERM`时应该优雅地关闭
* 进程应当是健壮的，并努力避免突然的关闭
* 开发和产品之间的差距应尽可能地小，以使得应用是可持续部署的
* 反对在开发和产品中使用不同的后端服务
* 应用不该关心其Log存储形式

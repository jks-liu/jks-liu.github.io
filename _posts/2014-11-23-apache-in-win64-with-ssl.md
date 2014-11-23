---
layout: post
title: "Windows 64位配置Apache HTTPS服务器"
description: ""
category: Meta
tags: [Apache, HTTPS, SSL]
---
{% include JB/setup %}

系统信息：Windows8.1 64位，Apache2.2。

Apache的配置文件写得还是很规范的，所以一开始改的时候就是跟着感觉走。

* 在`conf/httpd.conf`文件中分别找到下面两行，移除注释：

~~~ config
#LoadModule ssl_module modules/mod_ssl.so
#Include conf/extra/httpd-ssl.conf
~~~

* 如下面的示例所示，修改`conf/extra/httpd-ssl.conf`：

~~~ config
DocumentRoot "C:/Users/Jinchen/projects/github/ripple-client/build/bundle/web"
ServerName local.ripple.com:443
ServerAdmin jks@mrliu.org
SSLCertificateFile "C:/Program Files (x86)/Apache Software Foundation/Apache2.2/conf/server.crt"
SSLCertificateKeyFile "C:/Program Files (x86)/Apache Software Foundation/Apache2.2/conf/server.key"
~~~

这一步注意两点：

1. `DocumentRoot`配置的路径要和`httpd.conf`文件中的`DocumentRoot`和`<Directory>`一样；
2. 默认配置下`SSLcertificateKeyfile`对应的文件是应该是未加密的。

如果使用加密的key，会出现类似如下的错误：

~~~ log
[Sun Nov 23 13:37:28 2014] [error] Init: SSLPassPhraseDialog builtin is not supported on Win32 (key file C:/Program Files (x86)/Apache Software Foundation/Apache2.2/conf/server.key)
~~~

* 还是`httpd-ssl.conf`：将

~~~ config
#SSLSessionCache         "dbm:C:/Program Files (x86)/Apache Software Foundation/Apache2.2/logs/ssl_scache"
SSLSessionCache        "shmcb:C:/Program Files (x86)/Apache Software Foundation/Apache2.2/logs/ssl_scache(512000)"
~~~

改为：

~~~ config
SSLSessionCache         "dbm:C:/Program Files (x86)/Apache Software Foundation/Apache2.2/logs/ssl_scache"
#SSLSessionCache        "shmcb:C:/Program Files (x86)/Apache Software Foundation/Apache2.2/logs/ssl_scache(512000)"
~~~

据说这是只有在64位机器上才需要的。如果不这样修改的话，Apache Service Monitor会显示如下错误：

~~~
The requested operation has failed!
~~~

直接运行httpd会出现类似如下的错误：

~~~
Syntax error on line 62 of C:/Program Files (x86)/Apache Software Foundation/Apache2.2/conf/extra/httpd-ssl.conf:
SSLSessionCache: Invalid argument: size has to be >= 8192 bytes
~~~

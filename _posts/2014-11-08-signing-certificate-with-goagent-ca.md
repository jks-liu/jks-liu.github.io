---
layout: post
title: "使用Goagent CA给你的证书签名"
description: ""
category: How to
tags: [GoAgent, certificate, CA]
---
{% include JB/setup %}

[直到2014年10月28号，GoAgent一直默认使用公开私钥的根证书](https://github.com/goagent/goagent/commit/77c8e7f131f9eb7d857cded9c0bc2f662e80b78a)。由于GoAgent的广泛使用，估计其还会广泛地存在很长一段时间。如果你能无障碍访问<https://goagent-cert-test.bamsoftware.com/>，请立即删除默认的`CA.crt`。

本文教你如何使用这个证书给你的网站颁发证书。例子中我会使用`pi.mrliu.org`这个域名。

## 我的系统信息

~~~ shell
$ uname -a
Linux raspberrypi 3.12.31+ #720 PREEMPT Sat Nov 1 13:15:06 GMT 2014 armv6l GNU/Linux
$ openssl version
OpenSSL 1.0.1e 11 Feb 2013 (Library: OpenSSL 1.0.1j 15 Oct 2014)
$ python --version
Python 2.7.8
~~~

## 步骤

* 进入任意目录，并将`CA.crt`文件复制到这个目录。示例中，我用`/openssl/path/`代替这个目录。

* 为你的服务器生成密钥`server.key`：

~~~ sh
$ openssl genrsa -des3 -out server.key 2048
~~~

* 生成签名请求文件`server.csr`：

~~~ bash
$ openssl req -new -key server.key -out server.csr
~~~

填写信息时要注意Common Name这一项填你的域名，比如`*.mrliu.org`，`*`是通配符，表示签发给[mrliu.org](https://mrliu.org)的所有子域名。还有你的邮件地址至少看上去合法。

* 生成一个序列号文件：

~~~ bash
$ echo 01 > serial.txt
~~~

* 利用GoAgent CA为你的证书生成签名文件`server.crt`：

~~~ bash
$ openssl x509 -req -days 1234 -in server.csr -CA CA.crt -out server.crt -CAserial serial.txt
~~~

* 将签名文件和私钥合并到一个`server.pem`文件中：

~~~ bash
$ cat server.crt server.key > server.pem
~~~

## 测试

* 新建文件`https-server.py`，并写入如下内容：

~~~ python
#!/usr/bin/env python
## Jks Liu, https://mrliu.org/signing-certificate-with-goagent-ca/
## Take from https://gist.github.com/dergachev/7028596

## Change certfile path to yours.
## Root is required for port 443.

import BaseHTTPServer, SimpleHTTPServer
import ssl

httpd = BaseHTTPServer.HTTPServer(('', 443), SimpleHTTPServer.SimpleHTTPRequestHandler)
httpd.socket = ssl.wrap_socket (httpd.socket, certfile='/openssl/path/server.pem', server_side=True)
httpd.serve_forever()
~~~

注意修改其中的`server.pem`的地址。

* 运行测试服务器

~~~ bash
$ sudo python https-server.py
~~~

* 修改你的`hosts`文件

注意是你客户端上的`hosts`文件。当然，客户端可以和你的服务器在同一个机器上。假设你服务器的ip为192.168.1.111。

在`hosts`中添加如下行：

~~~
192.168.1.111   pi.mrliu.org
~~~

* 打开你客户端的浏览器：

浏览：<https://pi.mrliu.org>。

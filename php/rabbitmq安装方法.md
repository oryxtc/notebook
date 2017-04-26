---
title: rabbitmq安装步骤
date: 2017/1/14 20:46:25
categories: php
tags: 
- rabbitmq
description:  详细介绍了rabbitmq的安装步骤
---

### Windows 环境下安装RabbitMQ服务
1.因为RabbitMQ是由erlang语言实现的，所以先要安装erlang环境
erlang 下载安装 [http://www.erlang.org/download.html](http://www.erlang.org/download.html)

2.下载安装RabbitMQ服务,下载地址 [http://www.rabbitmq.com/install-windows.html](http://www.rabbitmq.com/install-windows.html)

3.安装成功后,菜单目录下有RabbitMQ目录的链接
![](/assets/QQ截图20161212203116.png)

3.访问RabbitMQ管理后台 [http://localhost:15672/](http://localhost:15672),默认用户名`guset`,默认密码`guset`
![](/assets/QQ截图20161212203443.png)


###Windows环境下 php5.6版本安装RabbitMQ扩展

1.php的amqp扩展下载地址：[http://pecl.php.net/package/amqp](http://pecl.php.net/package/amqp)

2.解压后如下所示
![](/assets/QQ截图20161212204455.png)

3.复制php_amqp.dll到php/ext    如我的放到 D:\xampp-php5.6\php\ext 目录下

4、php.ini中添加如下代码

```
[amqp]
extension=php_amqp.dll

```
5.复制rabbitmq.1.dll到php目录   如我的放到 D:\xampp-php5.6\php 目录下

6.修改apache配置文件httpd.conf添加入

```
LoadFile  "rabbitmq.1.dll文件路径"
```
7.重启apache   phpinfo显示如下![](/assets/QQ截图20161212204923.png)





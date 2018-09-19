---
title: redis扩展的安装
date: '2017/1/14 20:46:25'
categories: php
tags:
  - redis
  - 扩展
description: redis扩展的安装实现
---

# Redis扩展的安装

## Linux 环境下 安装redis扩展

> redis 驱动：下载地址为:[https://github.com/phpredis/phpredis/releases](https://github.com/phpredis/phpredis/releases)

```bash
$ cd /usr/local/src                      #进入你下载资源的文件夹
$ wget https://github.com/phpredis/phpredis/archive/2.2.4.tar.gz
$ cd phpredis-2.2.7                      #进入目录
$ /usr/local/php7/bin/phpize             #php安装后的路径 使用phpize
$ ./configure --with-php-config=/usr/local/php7/bin/php-config   #编译redis扩展
$ make
$ make install
```

> **如果是PHP7版本,则需要下载指定版本**
>
> ```text
> $ git clone -b php7 https://github.com/phpredis/phpredis.git
> $ cd ./phpredis
> ```

## 修改php.ini文件,增加如下内容

```bash
vi /usr/local/php7/lib/php.ini
```

```bash
extension_dir = "/usr/local/php/lib/php/extensions/no-debug-xxx-xxxxx" #根据自己的文件夹名改写

extension=redis.so
```


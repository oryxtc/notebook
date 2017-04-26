---
title: yii2前后台使用同一域名
date: 2017/1/14 20:46:25
categories: apche
tags: yii2
description: 由于`init()`函数与`_construct()`与`behaviors()`函数因为功能不一样，代码实现上也有细微不同.
---

### 在目录中新建`Dockerfile`文件

### 编辑`Dokcerfile`输入:
```yaml
#基于php-5.6-fpm基础镜像
FROM php:5.6-fpm

#执行命令 
RUN  apt-get update \	#更新软件包
	&& pecl install redis-2.2.7 \	#pecl方式安装redis扩展
	&& docker-php-ext-install -j$(nproc) bcmath pdo pdo_mysql \	#php基础镜像内置方法安装其他核心扩展
	&& docker-php-ext-enable redis \	#php基础镜像内置方法开启redis扩展
```

### 创建镜像
通过命令进入当前目录,命令行输入:
```bash
docker build -t <user-name>/<images-name>[:TAG] .
```

### 将该镜像推送到`Docker Hub`
先执行登录操作,命令行输入:
```bash
docker login
```
登录成功后,命令行再输入:
```bash
docker push <user-name>/<images-name>[:TAG]
```

---
title: dockerfile创建镜像
date: 2017/12/20
categories: docker
tags: dockerfile
description: 对于创建某一个镜像包,推荐使用dockerfile方式,本文示例了php-7.1-fpm镜像创建过程
---

### 在目录中新建`Dockerfile`文件

### 编辑`Dokcerfile`输入:
```yaml
#基于php-7.1-fpm基础镜像
FROM php:7.1-fpm

#执行命令 
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bf \
	&& curl -o /etc/apt/sources.list http://mirrors.163.com/.help/sources.list.jessie \ #使用国内镜像
	&& cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN apt-get update && apt-get install -y \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libmcrypt-dev \
		libpng12-dev \
		vsftpd \
	&& pecl install redis-3.1.4 \
	&& pecl install xdebug-2.5.5 \
	&& docker-php-ext-install -j$(nproc) bcmath pdo pdo_mysql fileinfo zip \ #php基础镜像内置方法安装其他核心扩展
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
	&& docker-php-ext-enable redis xdebug #php基础镜像内置方法开启redis等扩展
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

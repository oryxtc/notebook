---
title: 安装Docker for Windows
date: '2017/1/14 20:46:25'
categories: docker
description: 安装Docker for Windows所需注意准备情况
---

# docker安装-Windows版

> docker for Windows 下载地址为: [https://docs.docker.com/docker-for-windows/install/](https://docs.docker.com/docker-for-windows/install/)

## 首先请确保你系统为win10 企业或者教育版,并开启Hyper-V

![](http://ooqid2far.bkt.clouddn.com/myblog/docker-for-windows.png)

## 确认已开启支持虚拟化,若禁用,BIOS中开启 Intel Virtual Technology

![](http://ooqid2far.bkt.clouddn.com/myblog/docker%20for%20windows%282%29.png)

## 这里推荐使用阿里docker加速

登录阿里云后台复制加速地址,粘贴到 `Registry mirrors`中

![](http://ooqid2far.bkt.clouddn.com/myblog/docker%20for%20windows%283%29.png)

## 设置docker-machine

在命令行输入,新建虚拟机

```bash
docker-machine create -d hyperv  --engine-registry-mirror YOUR-ACCELERATED-ADDRESS
MACHINE-NAME
```


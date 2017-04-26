---
title: 安装Docker for Windows
date: 2017/1/14 20:46:25
categories: docker
description: 安装Docker for Windows所需注意准备情况
---

### 安装Docker for Windows

### 首先请确保你系统为win10 企业或者教育版,并开启Hyper-V
![](/assets/QQ图片20170228103513.png)

### 确认已开启支持虚拟化,若禁用,BIOS中开启 Intel Virtual Technology
![](/assets/QQ图片20170228103946.png)

### docker for Windows 下载地址为: [https://docs.docker.com/docker-for-windows/install/](https://docs.docker.com/docker-for-windows/install/)

### 因为国内访问docker hub速度实现难以忍受,这里推荐使用阿里docker加速,登录阿里云后台复制加速地址,粘贴到 `Registry mirrors`中
![](/assets/TIM截图20170411115427.png)


### 设置docker-machine

### 因为国内访问docker hub速度实现难以忍受,这里推荐使用阿里docker加速

### 在命令行输入,新建虚拟机
```bash
docker-machine create -d hyperv  --engine-registry-mirror YOUR-ACCELERATED-ADDRESS
MACHINE-NAME
```
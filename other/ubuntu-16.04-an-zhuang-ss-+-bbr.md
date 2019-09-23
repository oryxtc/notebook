---
title: ubuntu 16.04 安装SS + BBR
date: 2018/5/18
categories: other
description: 整理了一下算是ubuntu 16.04 安装SS + BBR最简单的方式.
---

# ubuntu 16.04 安装SS + BBR

## 搭建ss

### shadowsocks 服务器安装

更新软件源

`sudo apt-get update`

然后安装 PIP 环境

`sudo apt-get install python-pip`

直接安装 shadowsocks

`sudo pip install shadowsocks`

### 运行 shadowsocks 服务器

可以使用配置文件进行配置,创建/etc/shadowsocks.json文件 `vim /etc/shadowsocks.json`

填入以下内容:

```text
{
    "server":"0.0.0.0",
    "port_password":{
        "80":"password",
        "443":"password",
        "1080":"password",
        "33333":"password"
    },
    "local_address": "127.0.0.1",
    "local_port":1080,
    "timeout":300,
    "method":"rc4-md5",
    "fast_open":true
}
```

启动ss服务并后台启动

`sudo ssserver -c /etc/shadowsocks.json -d start`

### 配置开机自启动

编辑文件

`sudo vi /etc/rc.local`

在 exit 0 这一行的上边加入如下

`/usr/local/bin/ssserver –c /etc/shadowsocks.json -d start`

## 安装BBR

### 检测 BBR 是否开启

`sysctl net.ipv4.tcp_available_congestion_control`

在我的 Server 上，返回了：

`net.ipv4.tcp_available_congestion_control = cubic reno`

没有`bbr`我们再确认一次当前使用的控制算法：

`sysctl net.ipv4.tcp_congestion_control`

返回内容是：

`net.ipv4.tcp_congestion_control = cubic`

使用的是 cubic 这个默认的算法。接下去我们去启用 BBR。

### 为 Ubuntu 16.04 安装 4.10 + 新内核

使用 `uname -a` 查看你的内核版本

BBR 只能配合 Linux Kernel 4.10 以上内核才能使用,如果内核版本过低使用HWE方式升级内核,执行命令

`sudo apt-get install linux-generic-hwe-16.04`

> 假如你想使用更激进的新内核，可以使用 edge 版本： `sudo apt-get install linux-generic-hwe-16.04-edge`

安装好以后重启电脑，然后输入：

`uname -a`

看内核版本是否大于4.10

### 为 Ubuntu 16.04 启用 BBR

首先安装 BBR 模块：

`sudo modprobe tcp_bbr`

`echo "tcp_bbr" | sudo tee -a /etc/modules-load.d/modules.conf`

装载后，再执行命令检测`BBR`是否开启

`sysctl net.ipv4.tcp_available_congestion_control`

接下去再正式启用它：

`echo "net.core.default_qdisc=fq" | sudo tee -a /etc/sysctl.conf`

`echo "net.ipv4.tcp_congestion_control=bbr" | sudo tee -a /etc/sysctl.conf`

`sudo sysctl -p`

执行完这几条指令后，再执行命令检测算法是否变成`BBR`

`sysctl net.ipv4.tcp_congestion_control`


---
title: github自动部署
date: 2017/9/11 18:15:25
categories: linux
description: '通过node.js+webhook 实现github的自动部署'
---

### 以下命令,均是在Debian系统环境下,若是其他系统,使用对应命令即可

### 安装node.js
```bash
sudo apt-get install nodejs
```

### 安装npm
```bash
sudo apt-get install npm
```

### 我这里新建了一个目录作为自动部署服务的根目录,并进入该目录
```
mkdir /home/webhook;
cd /home/webhook;
```
### 这里需要用到node.js的中间件`github-webhook-handler`
```bash


---
title: 安装Docker for Debian
date: 2017/6/13 10:46:25
categories: docker
description: 安装Docker for Debian 所需注意准备情况
---

### 使用命令 `lsb_release -cs` 检测你Debian的版本

### 安装储存库
**Jessie 或者 Stretch输入:**
```bash
sudo apt-get update && apt-get -y install \
  apt-transport-https \
  ca-certificates \
  curl \
  software-properties-commo
```

**Wheezy:**
```bash
sudo apt-get update && apt-get -y install \
  apt-transport-https \
  ca-certificates \
  curl \
  python-software-properties
```

### 添加Docker官方GPG密钥:
```bash
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

### 设置稳定版本库:
**amd64**:
```bash
sudo add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/debian \
       $(lsb_release -cs) \
       stable"
```
**armhf**:
```bash
sudo echo "deb [arch=armhf] https://download.docker.com/linux/debian \
     $(lsb_release -cs) stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list
```

>Wheezy版本才有:add-apt-repository的版本添加了一个不存在的deb-src存储库。您需要注释掉该存储库否则运行apt-get update将失败。编辑/etc/apt/sources.list。找到如下所示的行，并将其注释掉或删除它
```
deb-src [arch=amd64] https://download.docker.com/linux/debian wheezy stable
```

### 安装docker

##### 更新apt软件包索引
```bash
sudo apt-get update
```

##### 安装最新版本的docker，或者到下一步安装指定版本的docker
```bash
apt-cache madison docker-ce
```

##### 执行如下命令安装指定的版本：
```bash
sudo apt-get install docker-ce=<VERSION>
```


---
title: docker下phpstorm使用xdebug
date: 2017/12/20
categories: docker
tags:
- xdebug
- phpstorm
description: 越来越多的开发者选择使用docker,本文介绍使用docker时,如何在phpstorm中调试xdebug。
---
### 配置php.ini的xdebug模块
```
...
[xdebug]
xdebug.remote_log ="/tmp/xdebug/remote.log"
xdebug.remote_autostart=1
xdebug.remote_enable=1
xdebug.remote_connect_back=0
xdebug.cli_color=1
xdebug.profiler_enable=0
xdebug.remote_handler=dbgp
xdebug.remote_mode=req
xdebug.remote_port=5902
xdebug.remote_host=xxx.xxx.x.xxx #你主机的ip  Windows输入ipconfig  linux输入ifconfig
xdebug.idekey=PHPSTORM
```

### 为了环境统一,首先确保docker正常运行,并对phpstorm进行配置.
打开`File->Settings->Build,Execution,Deployment->Docker`,然后新增一个部署,配置如下

![](http://ooqid2far.bkt.clouddn.com/myblog/docker%E4%B8%8Bphpstorm%E4%BD%BF%E7%94%A8xdebug-1.png)

### 在phpstorm中配置php使用引擎
打开`File->Settings->Languages&Frameworks->PHP`,然后点击`CLI Interpreter`新建一个,配置如下

![](http://ooqid2far.bkt.clouddn.com/myblog/docker%E4%B8%8Bphpstorm%E4%BD%BF%E7%94%A8xdebug-2.png)

### 填写完整php框架的配置
![](http://ooqid2far.bkt.clouddn.com/myblog/docker%E4%B8%8Bphpstorm%E4%BD%BF%E7%94%A8xdebug-3.png)

### 配置phpstorm中Debug
打开`File->Settings->Languages&Frameworks->PHP->Debug`,配置Xdebug的`Debug port`为`5902`

### 配置Debug
打开`Run->Edit Configurations`,新增`PHP Remote Debug`,`ide key`填入`PHPSTORM`,`Servers`配置如下

![](http://ooqid2far.bkt.clouddn.com/myblog/docker%E4%B8%8Bphpstorm%E4%BD%BF%E7%94%A8xdebug-4.png)

### 浏览器安装Debug插件
打开`File->Settings->Languages&Frameworks->PHP->Debug`
点击`isntall browser toolbar or bookmarklets`,下载你对应浏览器的插件,并启用

### 运行phpstorm的`Debug`按钮
最终运行效果如下

![](http://ooqid2far.bkt.clouddn.com/docker%E4%B8%8Bphpstorm%E4%BD%BF%E7%94%A8xdebug-5.png)

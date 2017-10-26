---
title: node.js常用命令
date: 2017/1/14 20:46:25
categories:
- node.js
tags: 
- node.js
description: 归纳总结了一些node.js的常用命令
---

### 初始化生成package.json
```
npm init
```

### 安装包并把依赖写入package.json
```
npm install xxxx--save-dev`  或者  `npm install xxxx -D
```

### 删除node_modules库
```
rimraf node_modules
```

### 删除模块
```
npm uninstall xxxx
```

### 查看包当前版本/最新版本号
```
npm outdated
```


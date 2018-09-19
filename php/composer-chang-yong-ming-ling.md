---
title: composer常用命令
date: '2017/1/14 20:46:25'
categories: php
tags: composer
description: 归纳总结了一些composer常用命令
---

# composer-命令详解

## 优化自动加载

```text
composer dump-autoload --optimize
```

## 仅更新单个库

```text
composer update foo/bar
```

## 仅更新composer.lock文件

```text
composer update nothing
```

## 安装库

```text
composer require "foo/bar:1.0.0"
```


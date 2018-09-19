---
title: 正则常用表达式
date: '2017/1/14 20:46:25'
categories: php
tags:
  - 正则
description: 归纳总结了一些正则常用的表达式
---

# 正则表达式整理

## PHP中preg\_match正则匹配的/u /i /s 分别的含义

```text
/u 表示按unicode(utf-8)匹配（主要针对多字节比如汉字）
/i 表示不区分大小写（如果表达式里面有 a， 那么 A 也是匹配对象）
/s 表示将字符串视为单行来匹配
```

## 匹配全角字符

```text
'/[\x{ff01}-\x{ff5e}]/u'
```


---
title: yii2前后端分离
date: 2017/1/14 20:46:25
categories: apche
tags: yii2
description: apache环境下实现yii2前后端分离
---

### 配置如下 
```
RewriteEngine On

# End the processing, if a rewrite already occurred

RewriteRule ^(frontend|admin)/web/ - [L]

RewriteRule ^frontend(/(.*))?$ frontend/web/$1 [L]

RewriteRule ^admin(/(.*))?$ backend/web/$1 [L]

# handle the case of frontend

RewriteRule .* website/index.html
```



---
title: yii2前后台使用同一域名
date: 2017/1/14 20:46:25
categories: apche
tags: yii2
description: apache环境实现同一域名指向yii2同一项目不同模块
---

### .htaccess配置如下
```
RewriteEngine On

# End the processing, if a rewrite already occurred

RewriteRule ^(frontend|admin)/web/ - [L]

# Handle the case of backend, skip ([S=1]) the following rule, if current matched

RewriteRule ^admin(/(.*))?$ backend/web/$2 [S=1]

# handle the case of frontend

RewriteRule .* frontend/web/$0


# Uncomment the following, if you want speaking URL

#RewriteCond %{REQUEST_FILENAME} !-f

#RewriteCond %{REQUEST_FILENAME} !-d

#RewriteRule ^([^/]+/web)/.*$ $1/index.php
```

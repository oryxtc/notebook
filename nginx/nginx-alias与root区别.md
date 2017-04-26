---
title: alias与root的区别
date: 2017/1/14 20:46:25
categories: nginx
tags: nginx
description: 详解nginx.conf中alias与root的区别
---

### 如果配置项为alias
```nginx
location /img/ {
    alias /var/www/image/;
}
```
>若按照上述配置的话，则访问/img/目录里面的文件时，ningx会去/var/www/image/目录找文件

### 如果配置项为root
```nginx
location /img/ {
    root /var/www/image;
}
```
>若按照上述配置的话，则访问/img/目录里面的文件时，ningx会去/var/www/image/img/目录找文件

### 图片胜过千言万语
**for alias**
![nginx-alias](http://ooqid2far.bkt.clouddn.com/myblog/nginx-alias.png)

**for root:**
![nginx-root](http://ooqid2far.bkt.clouddn.com/myblog/nginx-root.png)



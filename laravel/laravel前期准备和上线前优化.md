---
title: 前期准备和上线前优化
date: 2017/10/17
categories: php
tags: 
- laravel
description:  laravel前期一些准备工作,如代理配置,权限配置等.以及上线前端一些优化.
---
### nginx配置
这个文件可能需要根据你的服务器配置进行自定义

```nginx
server {
    listen 80;
    server_name example.com;
    root /example.com/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options "nosniff";

    index index.html index.htm index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.1-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

### 文件权限配置
```bash
chmod 777 storage
chmod 775 bootstrap/cache
```

### laravel生成密钥
```bash
php artisan key:generate
```

### 优化自动加载
部署项目到生产环境时，请确保你优化了 Composer 类的自动加载映射，以便 Composer 可以快速找到正确文件为给定类加载

```bash
composer install --optimize-autoloader
```

或者

```bash
composer dump-autoload --optimize
```

### 优化配置加载
```bash
php artisan config:cache
```

### 优化路由加载
```bash
php artisan route:cache
```
---
title: laravel+voyager实现后台管理
date: 2017/6/23 11:16
categories: laravel
description: 在原有基础上,实现前后台表分离,voyager汉化
---

### 安装laravel
```bash
laravel new website
```

### 安装voyager
```bash
composer require tcg/voyager
```

### 修改.env文件
```php
DB_HOST=localhost
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```

### 修改`APP_URL`储存图片时会使用该路径
```php
APP_URL=localhost:8000
```

###  添加voyager服务到`config/app.php`文件的`providers`数组中
```php
'providers' => [
    // Laravel Framework Service Providers...
    //...

    // Package Service Providers
    TCG\Voyager\VoyagerServiceProvider::class,
    // ...

    // Application Service Providers
    // ...
],
```

### 安装voyager,完成数据迁移,和资源发布,voyager会在users表中增加`avator`和`role_id`两个字段
```bash
php artisan voyager:install
```

### 创建超级管理员
```php
php artisan voyager:admin your@email.com --create
```
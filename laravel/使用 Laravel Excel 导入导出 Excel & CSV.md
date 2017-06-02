---
title: 使用 Laravel Excel 导入导出 Excel & CSV
date: 2017/6/2 15:16
categories: laravel
description: 使用优雅的方式导入，导出 Excel和CSV
---

### 安装

在 composer.json 中添加相应的包
```composer
"maatwebsite/excel": "~2.1.0"
```

然后在命令行执行以下代码更新包
```bash
composer update
```

修改 Laravel 的配置文件`config/app.php` ，在 $providers 数组中添加一个服务提供者。
```php
Maatwebsite\Excel\ExcelServiceProvider::class,
```

你可以添加门面来使用较短代码
```php
'Excel' => Maatwebsite\Excel\Facades\Excel::class,
```

### 配置
发布配置信息到你配置文件夹中
```php
php artisan vendor:publish --provider=Maatwebsite\Excel\ExcelServiceProvider
```

### 导入

##### 导入一个文件
```php
Excel::load('file.xls', function($reader) {

    //禁用第一行作为属性
    $reader->noHeading();
    
    // 获取数据的集合
    $results = $reader->get();
    
    // 获取第一行数据
    $results = $reader->first();
    
    // 获取前10行数据
    $reader->take(10);
    
    // 跳过前10行数据
    $reader->skip(10);
    
    // 以数组形式获取数据
    $reader->toArray();
    
    // 打印数据
    $reader->dump();
    
    // 遍历工作表
    $reader->each(function($sheet) {
    
        // 遍历行
        $sheet->each(function($row) {
        
        });
    });
});
```

##### 选择页和列
```php
// 选择指定页
Excel::selectSheets('sheet1')->load();

// 选择多页
Excel::selectSheets('sheet1', 'sheet2')->load();

// 选择第一页
Excel::selectSheetsByIndex(0)->load();

// 选择第一和第二页
Excel::selectSheetsByIndex(0, 1)->load();

// 获取指定的列
$reader->select(array('firstname', 'lastname'))->get();

// 获取指定的列
$reader->get(array('firstname', 'lastname'));
```

---
title: 实现RBAC权限管理
date: 2017/1/14 20:46:25
categories: yii2
tags: 
- RBAC
description: 用户或者管理员的权限控制属于网站不可或缺的环节,当对权限的业务需求不复杂时,为避免重复造轮子,可直接引用第三方库,本文引入yii2-admin实现RBAC的权利控制与管理
---

### 安装`mdmsoft/yii2-admin`
>在`github`上的托管地址为:[https://github.com/mdmsoft/yii2-admin](https://github.com/mdmsoft/yii2-admin)

使用`composer`方式引入
```bash
composer require mdmsoft/yii2-admin "~2.0"
```

### 在`common/config/main-local.php`中配置
```php
'components' => [
	'db' => [
	//...
	],
	'authManager' => [
		'class' => 'yii\rbac\DbManager', // 使用数据库管理配置文件
	]
],

// 添加行为 ACF访问权限管理
'as access' => [
	'class' => 'mdm\admin\components\AccessControl',
	'allowActions' => [
		'site/login',
		'site/error',
	]
],
```
### 使用命令在控制台中创建所需表
```bash
yii migrate --migrationPath=@mdm/admin/migrations yii migrate --migrationPath=@yii/rbac/migrations
```

### 配置模块
```php
'modules' => [
//rbac管理
	'rbac' => [
		'class' => 'mdm\admin\Module',
		'layout' => 'left-menu', // it can be '@path/to/your/layout'.
	],
]
```

### 如果数据库管理员表名需要重命名
例如我重命名表名为`administrator`,还需要修改`mdm\admin\components\Configs.php`

```php
/**
* @var string Menu table name.
*/
public $userTable = '{{%administrator}}';
```

### 验证是否引入成功
在浏览器地址栏中中输入`后台地址路径/rbac`(该地址经过`urlManager`美化),即可看见效果
![yii2-admin](http://ooqid2far.bkt.clouddn.com/myblog/yii2-admin.png)








---
title: 开源后台模板AdminLTE
date: 2017/1/14 20:46:25
categories:: yii2
tags: 
- AdminLTE
- 模板
description: {% fi http://ooqid2far.bkt.clouddn.com/myblog/adminLte.png, alt, /adminLte %}
---
##### 1.引入`AdminLTE`的代码
在`github`上的托管地址:[https://github.com/almasaeed2010/AdminLTE](https://github.com/almasaeed2010/AdminLTE)

使用`composer`方式引入

```bash
composer require "almasaeed2010/adminlte=~2.0"
```

##### 2.引入`AdminLTE Asset Bundle`的代码
在`github`上的托管地址:[https://github.com/dmstr/yii2-adminlte-asset](https://github.com/dmstr/yii2-adminlte-asset)
使用`composer`方式引入类

```bash
composer require dmstr/yii2-adminlte-asset "2.*"
```

##### 3.如果使用的YII2高级模式
后台应用在`/backend，所以`在`backend/config/main.php`中配置

```php
'components' => [
	'view' => [
		'theme' => [
			'pathMap' => [
				'@backend/views' => '@vendor/dmstr/yii2-adminlte-asset/example-views/yiisoft/yii2-app'
			],
		],
	],
]
```

或者直接复制`vendor/dmstr/yii2-adminlte-asset/example-views/yiisoft/yii2-app`下的文件夹,覆盖`backend/web/views`下的文件夹



##### 4.这是成功后，输入后台地址的显示结果

![](http://ooqid2far.bkt.clouddn.com/myblog/adminLte.png!ratio.1000)

---
title: 用户管理模块yii2-user
date: 2017/1/14 20:46:25
categories: yii2
tags: 
- 模块
description:  您可以使用yii2-user，它是基于Yii2的灵活的用户管理模块，用于处理常见任务，如注册，身份验证和密码检索
---

### 引入`dektrium/yii2-user`的代码
在`github`上的托管地址:[https:\/\/github.com\/dektrium\/yii2-user](https://github.com/dektrium/yii2-user),
使用`composer`方式引入类

```
composer require "dektrium/yii2-user:0.9.*@dev"
```

### 配置`main.php`的组件
>请确保你没有在你的配置文件中使用`user`组件配置

配置如下:
```php
'modules' => [
	'user' => [
		'class' => 'dektrium\user\Module',
	],
],
```

### 更新数据表
```bash
$ php yii migrate/up --migrationPath=@vendor/dektrium/yii2-user/migrations
```

### 修改视图模板

>跳转地址使用了url美化,请确保配置了`urlManager`组件

`@app\views\layouts\main.php`文件中将

```php
if (Yii::$app->user->isGuest) {
	$menuItems[] = ['label' => 'Signup', 'url' => ['/site/signup']];
	$menuItems[] = ['label' => 'Login', 'url' => ['/site/login']];
} else {
	$menuItems[] = '<li>'
	. Html::beginForm(['/site/logout'], 'post')
	. Html::submitButton(
		'Logout (' . Yii::$app->user->identity->username . ')',
		['class' => 'btn btn-link']
	)
	. Html::endForm()
	. '</li>';
}
```

替换为

```php
if (Yii::$app->user->isGuest) {
	$menuItems[] = ['label' => 'Sign in', 'url' => ['/user/security/login']];
	$menuItems[] = ['label' => 'Register', 'url' => ['/user/registration/register'], 'visible' => Yii::$app->user->isGuest];
} else {
	$menuItems[] = ['label' => 'Sign out (' . Yii::$app->user->identity->username . ')',
	'url' => ['/user/security/logout'],
	'linkOptions' => ['data-method' => 'post']];
}
```
### 输入你项目网址,效果如下
![yii2-user](http://ooqid2far.bkt.clouddn.com/myblog/yii2-user.png)

>当你注册新用户后,该扩展默认会发送邮件,必须邮箱验证后才能正式登陆,如果需要修改配置参数请查阅官方文档

### 如果你想在一个域中使用独立的会话,即登陆前端的`session`不能用来登陆后端

在`@frontend\config\main.php`中配置项如下
```php
'components' => [
	'user' => [
		'identityCookie' => [
			'name' => '_frontendIdentity',
			'path' => '/',
			'httpOnly' => true,
		],
	],
	'session' => [
		'name' => 'FRONTENDSESSID',
		'cookieParams' => [
			'httpOnly' => true,
			'path' => '/',
		],
	],
],
```
在`@backend\config\main.php`中配置项如下
```php
'components' => [
	'user' => [
		'identityCookie' => [
			'name' => '_backendIdentity',
			'path' => '/admin',
			'httpOnly' => true,
		],
	],
	'session' => [
		'name' => 'BACKENDSESSID',
		'cookieParams' => [
			'httpOnly' => true,
			'path' => '/admin',
		],
	],
],
```
### 错误排查

1.用户登陆后,点击注销登陆,错误提示为
`After logging in I'm redirected back without any sign of being logged in`

**解决方案**:在`main.php` 组件中修改`user`
```php
'user' => [
	'class' => 'app\components\User',
	'identityClass' => 'dektrium\user\models\User',
],
```







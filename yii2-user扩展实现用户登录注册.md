1.首先引入`dektrium/yii2-user`的代码,在`github`上的托管地址:[https:\/\/github.com\/dektrium\/yii2-user](https://github.com/dektrium/yii2-user),
使用`composer`方式引入类

```
composer require "dektrium/yii2-user:0.9.*@dev"
```

2.配置`main.php`的组件
>请确保你没有在你的配置文件中使用`user`组件配置


配置如下:
```php
'modules' => [
    'user' => [
        'class' => 'dektrium\user\Module',
    ],
],
```

3.更新数据表

    $ php yii migrate/up --migrationPath=@vendor/dektrium/yii2-user/migrations

4.修改视图模板

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
        $menuItems[] = ['label'       => 'Sign out (' . Yii::$app->user->identity->username . ')',
                        'url'         => ['/user/security/logout'],
                        'linkOptions' => ['data-method' => 'post']];
}
```
    
5.输入你项目网址,效果如下
![](/assets/QQ截图20160928103121.png)

>当你注册新用户后,该扩展默认会发送邮件,必须邮箱验证后才能正式登陆,如果需要取消请查阅官方文档

**错误排查**

6.1 用户登陆后,点击注销登陆,错误提示为
```php
After logging in I'm redirected back without any sign of being logged in
```
解决方案:在`main.php` 组件中修改`user`
```php
'user' => [
    'class' => 'app\components\User',
    'identityClass' => 'dektrium\user\models\User',
],
```

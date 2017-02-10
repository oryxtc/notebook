* ###得到相应内容 格式化为json格式
```php
Yii::$app->getResponse()->format = Response::FORMAT_JSON;
```

* ###设置身份验证信息
```php
Yii::$app->getUser()->setIdentity($identity)
```

* ###自定义键名 
```php
 $users = $connection->createCommand('SELECT * FROM user')->index("你的下标")->select("你查询的字段")->queryAll();
```

* ###实现返回上一页
```php
return $this->goBack(Yii::$app->getRequest()->headers['Referer']);
```
或者
```php
Yii::$app->request->urlReferrer;
```
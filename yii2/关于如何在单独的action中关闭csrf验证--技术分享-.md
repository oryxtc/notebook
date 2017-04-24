---
title: 用户管理模块yii2-user
date: 2017/1/14 20:46:25
categories: yii2
tags: 
- 模块
description: 由于`Yii2 Controller Csrf`验证是在`beforeAction`中完成的，所以在`action`中指定 `$this->enableCsrfValidation = false`,不能实现局部关闭Csrf.
---
#### 新建一个`Behavior`

```php
<?php 
use Yii; 
use yii\base\ActionEvent; 
use yii\base\Behavior; 
use yii\web\Controller; 
class NoCsrf extends Behavior 
{ 
    public $actions = []; 
    public $controller; 
    public function events() 
    { 
        return [Controller::EVENT_BEFORE_ACTION => 'beforeAction']; 
    } 
    public function beforeAction($event) 
    { 
        $action = $event->action->id; 
        if(in_array($action, $this->actions)){ 
            $this->controller->enableCsrfValidation = false; 
        } 
    }     
}
```

#### 在`Controller`中添加`Behavior`

```php
<?php 
    public function behaviors() 
    { 
        return [ 
            'csrf' => [ 
                'class' => NoCsrf::className(), 
                'controller' => $this, 
                'actions' => [ 
                    'action-name' 
                ] 
            ] 
        ]; 
    }
```

这样就实现了在`action`中关闭`Csrf`而不是在整个`Controller`中关闭。
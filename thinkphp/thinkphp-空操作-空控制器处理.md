---
title: thinkphp-空操作 空控制器处理
date: 2017/1/14 20:46:25
categories: thinkphp
tags: 
- thinkphp
description:  空操作处理&空控制器处理
---
#### 空操作处理

在控制器里添加一个_empty()的方法
```php
public function _empty(){
    //echo '您访问的操作不存在 ('.MODULE_NAME.'/'.CONTROLLER_NAME.'/'.ACTION_NAME.')';;
    $this->display('Manager@Public:empty');
}
```

#### 空控制器处理
我们可以给项目定义一个EmptyController类
```php
<?php
namespace Home\Controller;
use Think\Controller;
class EmptyController extends Controller{
    public function index(){
        echo '你当前访问的控制器不存在';
    }
}
```
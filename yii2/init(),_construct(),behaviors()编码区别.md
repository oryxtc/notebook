---
title: init(),_construct(),behaviors() 编码区别
date: 2017/1/14 20:46:25
categories: yii2
description: 由于`init()`函数与`_construct()`与`behaviors()`函数因为功能不一样，代码实现上也有细微不同.
---
#### `_construct()`函数编码风格
```php
public function __construct($param1, $param2, $config = []){
    // ... 配置生效前的初始化过程
    parent::__construct($config);
}
```

#### `init()`函数编码风格
```php
public function init(){
    parent::init();
    // ... 配置生效后的初始化过程
}
```

#### `behaviors()`函数编码风格
```php
public function behaviors() {
    $behaviors = parent::behaviors();
    //修改或者添加，删除具体行为
    return $behaviors;
}
```

> - 若你需要重写构造方法（Constructor），传入 `$config` 作为构造器方法最后一个参数， 然后把它传递给父类的构造方法。
> - 永远在你重写的构造方法结尾处调用一下父类的构造方法。
> - 如果你重写了 yii\base\Object::init() 方法，请确保你在 `init` 方法的开头处调用了父类的 `init` 方法。



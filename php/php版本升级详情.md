---
title: php版本升级详情
date: 2017/8/14 16:32:00
categories: php
tags: 手册
description: php各版本升级影响范围及详情
---

# 从 PHP 5.2.x 移植到 PHP 5.3.x

### 不向下兼容的变化

- 在 PHP 5.3.x 的所有绑定扩展中应用了新的内部参数解析API, 当给函数传递了不兼容的参数时将返回 NULL. 但有一些例外，比如函数 `get_class()` 在出现错误时将会返回 FALSE. 

- `clearstatcache()` 默认不再清除缓存的 realpath.

- `realpath()` 现在是完全与平台无关的. 结果是非法的相对路径比如 __FILE__ . "/../x" 将不会工作.

- `call_user_func()` 系列函数即使被调用者是一个父类也使用 $this.

- 数组函数 natsort(), natcasesort(), usort(), uasort(), uksort(), array_flip(), 和 array_unique() 将不再接受对象作为参数. 在将这些函数应用于对象时, 请首先将对象转换为数组.

- 按引用传递参数的函数在被按值传递调用时行为发生改变. 此前函数将接受按值传递的参数, 现在将抛出致命错误. 之前任何期待传递引用但是在调用时传递了常量或者字面值 的函数, 需要在调用前改为将该值赋给一个变量。

- 新的 mysqlnd 库需要使用 MySQL 4.1 新的 41 字节密码格式。继续使用旧的 16 字节密码将导致 mysql_connect() 和其它类似函数 抛出 "mysqlnd cannot connect to MySQL 4.1+ using old authentication." 错误.

- 新的 mysqlnd 库将不再读取 MySQL 配置文件(my.cnf/my.ini), 这与旧版本的 libmysql 库不同. 如果你的代码依赖于这些配置 文件, 你可以使用 mysqli_options() 显式地加载它. 注意, 这意味着如果 PDO 中的 MySQL 支持使用了 mysqlnd 进行编译，PDO 特有常量 PDO::MYSQL_ATTR_READ_DEFAULT_FILE 和 PDO::MYSQL_ATTR_READ_DEFAULT_GROUP 将是未定义的.

- SplFileInfo 及其相关目录类会移除末尾的 /.

- __toString 魔术方法不再接受参数.

- 魔术方法 __get, __set, __isset, __unset, and __call 应该总是公共的(public)且不能是静态的(static). 方法签名是必须的.

- 现在 __call 魔术方法在访问私有的(private)和被保护的(protected)方法时被调用.

- 函数内 include 或者 require 一个文件时，文件内 将不能使用 func_get_arg(), func_get_args() 和 func_num_args() 函数。

- 新增了一个包裹在 MHASH 扩展外面的仿真层。但是并非所有的算法都涉及到了，值得注意的是 s2k 哈希算法。这意味着 s2k 哈希算法在 PHP 5.3.0 中不再可用。





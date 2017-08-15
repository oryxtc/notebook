---
title: 从 PHP 5.2.x 升级到 PHP 5.3.x
date: 2017/8/14 16:32:00
categories: php
tags: 手册
description: PHP 5.3.x 的大多数改进对现有代码没有影响。需要注意的是有一些不兼容和新功能，在生产环境中切换 PHP 版本需要先行测试代码。
---

### 不向下兼容的变化

- 在 PHP 5.3.x 的所有绑定扩展中应用了新的内部参数解析API, 当给函数传递了不兼容的参数时将返回 NULL. 但有一些例外，比如函数 `get_class()` 在出现错误时将会返回 FALSE. 

- `clearstatcache()` 默认不再清除缓存的 realpath.

- `realpath()` 现在是完全与平台无关的. 结果是非法的相对路径比如 __FILE__ . "/../x" 将不会工作.

- `call_user_func()` 系列函数即使被调用者是一个父类也使用 $this.

- 数组函数 `natsort()`, `natcasesort()`, `usort()`, `uasort()`, `uksort()`, `array_flip()`, 和 `array_unique()` 将不再接受对象作为参数. 在将这些函数应用于对象时, 请首先将对象转换为数组.

- 按引用传递参数的函数在被按值传递调用时行为发生改变. 此前函数将接受按值传递的参数, 现在将抛出致命错误. 之前任何期待传递引用但是在调用时传递了常量或者字面值 的函数, 需要在调用前改为将该值赋给一个变量。

- 新的 mysqlnd 库需要使用 MySQL 4.1 新的 41 字节密码格式。继续使用旧的 16 字节密码将导致 `mysql_connect()` 和其它类似函数 抛出 "mysqlnd cannot connect to MySQL 4.1+ using old authentication." 错误.

- 新的 mysqlnd 库将不再读取 MySQL 配置文件(my.cnf/my.ini), 这与旧版本的 libmysql 库不同. 如果你的代码依赖于这些配置 文件, 你可以使用 `mysqli_options()` 显式地加载它. 注意, 这意味着如果 PDO 中的 MySQL 支持使用了 mysqlnd 进行编译，PDO 特有常量 `PDO::MYSQL_ATTR_READ_DEFAULT_FILE` 和 `PDO::MYSQL_ATTR_READ_DEFAULT_GROUP` 将是未定义的.

- `SplFileInfo` 及其相关目录类会移除末尾的 /.

- `__toString` 魔术方法不再接受参数.

- 魔术方法 `__get`, `__set`, `__isset`, `__unset`, and `__call` 应该总是公共的(public)且不能是静态的(static). 方法签名是必须的.

- 现在 `__call` 魔术方法在访问私有的(private)和被保护的(protected)方法时被调用.

- 函数内 `include` 或者 `require` 一个文件时，文件内 将不能使用 `func_get_arg()`, `func_get_args()` 和 `func_num_args()` 函数。

- 新增了一个包裹在 MHASH 扩展外面的仿真层。但是并非所有的算法都涉及到了，值得注意的是 s2k 哈希算法。这意味着 s2k 哈希算法在 PHP 5.3.0 中不再可用。

### 增加的新特性

- 添加了命名空间的支持.

- 添加了静态晚绑定支持.

- 添加了跳标签支持.

- 添加了原生的闭包(Lambda/匿名函数)支持.

- 新增了两个魔术方法, __callStatic 和 __invoke.

- 添加了 Nowdoc 语法支持, 类似于 Heredoc 语法, 但是包含单引号.

- 使用 Heredoc 来初始化静态变量和类属性/常量变为可能.

- 可使用双引号声明 Heredoc, 补充了 Nowdoc 语法.

- 可在类外部使用 const 关键词声明 常量.

- 三元运算操作符有了简写形式: ?:.

- HTTP 流包裹器将从 200 到 399 全部的状态码都视为成功。

- 动态访问静态方法变为可能.

- 异常可以被内嵌.

- 新增了循环引用的垃圾回收器并且默认是开启的.

- mail() 现在支持邮件发送日志. (注意: 仅支持通过该函数发送的邮件.)

### PHP 5.3.x 中弃用的功能

##### 下面是被弃用的 INI 指令列表. 使用下面任何指令都将导致 E_DEPRECATED 错误.

- `define_syslog_variables`

- `register_globals`

- `register_long_arrays`

- `safe_mode`

- `magic_quotes_gpc`

- `magic_quotes_runtime`

- `magic_quotes_sybase`

- 弃用 INI 文件中以 '#' 开头的注释.

##### 弃用函数

- `call_user_method()` (使用 `call_user_func()` 替代)

- `call_user_method_array()` (使用 `call_user_func_array()` 替代)

- `define_syslog_variables()`

- `dl()`

- `ereg()` (使用 `preg_match()` 替代)

- `ereg_replace()` (使用 `preg_replace()` 替代)

- `eregi()` (使用 `preg_match()` 配合 'i' 修正符替代)

- `eregi_replace()` (使用 `preg_replace()` 配合 'i' 修正符替代)

- `set_magic_quotes_runtime()` 以及它的别名函数 `magic_quotes_runtime()`

- `session_register()` (使用 `$_SESSION` 超全部变量替代)

- `session_unregister()` (使用 `$_SESSION` 超全部变量替代)

- `session_is_registered()` (使用 `$_SESSION` 超全部变量替代)

- `set_socket_blocking()` (使用 `stream_set_blocking()` 替代)

- `split()` (使用 `preg_split()` 替代)

- `spliti()` (使用 `preg_split()` 配合 'i' 修正符替代)

- `sql_regcase()`

- `mysql_db_query()` (使用 `mysql_select_db()` 和 `mysql_query()` 替代)

- `mysql_escape_string()` (使用 `mysql_real_escape_string()` 替代)

- 废弃以字符串传递区域设置名称. 使用 LC_* 系列常量替代.

- `mktime()` 的 is_dst 参数. 使用新的时区处理函数替代.

### PHP 5.3.x 保留的功能

- `is_a()` 函数被保留. 它不再抛出 E_STRICT 错误.

### PHP 5.3.x 新参数

##### PHP核心

- `clearstatcache()` - 新增 clear_realpath_cache 和 filename 参数.

- `copy()` - 新增流环境参数 context.

- `fgetcsv()` - 新增 escape 参数.

- `ini_get_all()` - 新增 details 参数.

- `nl2br()` - 新增 is_xhtml 参数.

- parse_ini_file()` - 新增 scanner_mode 参数.

- `round()` - 新增 mode 参数.

- `stream_context_create()` - 新增 params 参数.

- `strstr()` 和 `stristr()` - 新增 before_needle 参数.

##### json:

- `json_encode()` - 新增 options 参数.

- `json_decode()` - 新增 depth 参数.

##### 流(Streams):

- `stream_select()`, `stream_set_blocking()`, `stream_set_timeout()`, 和 `stream_set_write_buffer()` 使用用户空间流包裹器.

##### sybase_ct:

- `sybase_connect()` - 新增 new 参数.

### PHP 5.3.x 新函数

##### PHP 核心

- `array_replace()` - 将一个数组的元素用另外一个数组的元素进行替换.

- `array_replace_recursive()` - 将一个数组的元素用一组传递进来的数组进行递归替换.

- `class_alias()` - 为用户定义的类创建一个别名.

- `forward_static_call()` - 从一个方法环境调用一个用户函数.

- `forward_static_call_array()` - 从一个方法环境调用一个用户函数, 使用数组中的元素作为参数.

- `gc_collect_cycles()` - 强制收集任何存在的废物循环.

- `gc_disable()` - 撤销循环引用收集器.

- `gc_enable()` - 激活循环引用收集器.

- `gc_enabled()` - 返回循环引用收集器的状态.

- `get_called_class()` - 返回调用的静态方法所在的类的名称.

- `gethostname()` - 返回本地机器的当前主机名.

- `header_remove()` - 在使用 `header()` 函数之前移除 HTTP Header.

- `lcfirst()` - 蒋某一字符串第一个字符转化为小写.

- `parse_ini_string()` - 解析配置字符串.

- `quoted_printable_encode()` - 转换 8 位的字符串为引用的可打印字符串.

- `str_getcsv()` - 将 CSV 字符串解析为数组.

- `stream_context_set_default()` - 设置默认的流环境.

- `stream_supports_lock()` - 如果流支持锁定则返回 TRUE.

- `stream_context_get_params()` - 获取一个流环境的参数.

- `streamWrapper::stream_cast()` - 获取底层的流资源.

- `streamWrapper::stream_set_option()` - 更改流选项


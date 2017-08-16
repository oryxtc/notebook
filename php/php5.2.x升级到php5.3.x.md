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

##### Date/Time:

- `date_add()` - 向 DateTime 对象增加一定数量的天, 月, 年, 小时, 分钟和秒数.

- `date_create_from_format()` - 根据给定的格式, 返回一个 DateTime 对象.

- `date_diff()` - 返回两个 DateTime 对象的不同之处.

- `date_get_last_errors()` - 返回最后的日期/时间操作中产生的警告和错误.

- `date_parse_from_format()` - 获取一个日期的信息.

- `date_sub()` - 从 DateTime 对象减去一定数量的天, 月, 年, 时和秒数.

- `timezone_version_get()` - 返回时区数据库的版本.

##### GMP:

- `gmp_testbit()` - 测试一个比特是否被设置.

##### Hash:

- `hash_copy()` - 复制哈希环境.

##### IMAP:

- `imap_gc()` - 清除 IMAP 缓存.

- `imap_utf8_to_mutf7()` - 编码 UTF-8 字符串为改进的 UTF-7 编码.

- `imap_mutf7_to_utf8()` - 解码改进的 UTF-7 字符串为 UTF-8 编码.

##### JSON:

- `json_last_error()` - 返回最后发生的 JSON 错误.

##### MySQL 改进:

- `mysqli_fetch_all()` - 以关联数组、索引数组或者二者都有获取全部结果行.

- `mysqli_get_connection_stats()` - 返回客户端连接的统计资料.

- `mysqli_poll()` - 轮询连接.

- `mysqli_reap_async_query()` - 从异步查询中获取结果.

##### OpenSSL:

- `openssl_random_pseudo_bytes()` - 返回一个以伪随机字节填充的指定长度的字符串.

##### PCNTL:

- `pcntl_signal_dispatch()` - 为挂起信号调用信号处理器.

- `pcntl_sigprocmask()` - 设置和获取阻塞信号.

- `pcntl_sigtimedwait()` - 等待信号, 但是有超时时间.

- `pcntl_sigwaitinfo()` - 等待信号.

##### PCRE:

- `preg_filter()` - 执行正则查找和替换, 仅仅返回匹配正则的结果.

##### 信号:

- `msg_queue_exists()` - 检查消息队列是否存在.

- `shm_has_var()` - 检查在一个共享内存段中, 是否存在指定的键(key).

##### 以下函数被原生支持, 因此它们在所有运行 PHP 的操作系统上均可用.

- `acosh()`

- `asinh()`

- `atanh()`

- `expm1()`

- `log1p()`

### 新的流包装器

- glob://

- phar://

### 新的流过滤器

- `dechunk` (反转 HTTP 块编码)

- `bz2.decompress` 过滤器支持连续操作.

### 新增的类常量

##### PDO_FIREBIRD:

- `PDO::FB_ATTR_DATE_FORMAT` - 为日期设置格式.

- `PDO::FB_ATTR_TIME_FORMAT` - 为时间设置格式.

- `PDO::FB_ATTR_TIMESTAMP_FORMAT` - 为时间戳设置格式.

### 新增的方法

##### 日期/时间:

- `DateTime::add()` - 向一个 DateTime 对象加上若干天, 月, 年, 小时, 分钟和秒.

- `DateTime::createFromFormat()`- 根据给定的格式, 返回一个新的格式化的DateTime对象.

- `DateTime::diff()` - 返回两个 DateTime 对象的不同之处.

- `DateTime::getLastErrors()` - 返回最后的日期/时间处理中的警告和错误.

- `DateTime::sub()` - 从 DateTime 对象中减去若干天, 月, 年, 小时, 分钟和秒.

##### 异常(Exception):

- `Exception::getPrevious()` - 获取前一个异常.

##### DOM:

- `DOMNode::getLineNo()` - 返回解析节点的行数.

##### PDO_FIREBIRD:

- `PDO::setAttribute()` - 设置属性.

##### 反射(Reflection):

- `ReflectionClass::getNamespaceName()` - 返回类定义所在命名空间的的名称.

- `ReflectionClass::getShortName()` - 返回类的短名称(没有命名空间部分).

- `ReflectionClass::inNamespace()` - 返回类是否定义于一个命名空间.

- `ReflectionFunction::getNamespaceName()` - 返回函数定义所在命名空间的名字.

- `ReflectionFunction::getShortName()` - 返回函数的段名称(没有命名空间部分).

- `ReflectionFunction::inNamespace()` - 返回函数在一个命名空间中是否被定义.

- `ReflectionProperty::setAccessible()` - 设置是否可以请求非 public 属性.

##### SPL:

- `SplObjectStorage::addAll()` - 从另一个 SplObjectStorage 对象中新增全部元素.

- `SplObjectStorage::removeAll()` - 从另一个 SplObjectStorage 对象中移除全部元素.

##### XSL:

- `XSLTProcessor::setProfiling()` - 设置概况输出文件.

### 新增的扩展

- `Enchant` - 各种拼写库的抽象层

- `Fileinfo` - 已经被移除的 Mimetype 扩展的一个改进的、更加可靠的替代, 以 BC 为特色.

- `INTL` - 国际化扩展. INTL 是 » ICU 库的一个包装器.

- `Phar` - PHP 档案文件的实现.

- `SQLite3` - 支持 SQLite version 3 数据库.

### 被移除的扩展

- `dbase` - 不再被保持

- `fbsql` - 不再被保持

- `fdf` - 被保持

- `ming` - 被保持

- `msql` - 不再被保持

- `ncurses` - 被保存

- `sybase` - 停用; 使用 sybase_ct 扩展代替.

- `mhash` - 停用; 使用 hash 扩展代替. hash 全兼容 mhash; 全部使用旧函数的应用程序仍将可以工作.

### 扩展的其他改变

##### 以下扩展不再能在编译配置时进行关闭:

- `PCRE`

- `Reflection`

- `SPL`

#####　扩展行为和新功能的改变:

- `Datetime` - 获取时区时不再使用 TZ 环境变量.

- `cURL` - cURL 支持 SSH

- `Network` - dns_check_record() 现在返回一个额外的 "entries" 索引, 包含 TXT 元素.

- `Hash` - SHA-224 和 salsa 哈希算法获得支持.

- `mbstring` - 现在支持 CP850 编码.

- `OCI8` - 在持久连接上调用 `oci_close()` 或者在作用域外引用持久连接, 将回滚任何尚未提交的事务. 为了避免意外的行为, 根据需要明确发布和回滚事务. 使用 INI 指令`oci8.old_oci_close_semantics`可以打开旧的行为. 数据库驻留连接池(DRCP)和快速应用通知(FAN)获得支持. Oracle 外部认证获得支持(除了 Windows 平台). `oci_bind_by_name()` 函数现在支持 SQLT_AFC (又称 CHAR 数据类型).

- `OpenSSL` - 支持 OpenSSL 摘要和加密函数. 访问 DSA, RSA 和 DH 键的内部值变为可能.

- `Session`- 当应用了 open_basedir 限制, Session 不再将 Session 文件存储在 `"/tmp"` 目录中, 除非 `"/tmp"` 目录被明确添加到许可路径列表.

- `SOAP` 支持发送用户提供的 HTTP 头.

- `MySQLi` 通过在主机名前面添加 "p:" 来支持持久连接.

- `image` And `GD` `gd_info()` 函数返回的 "JPG Support" 索引变更为 "JPEG Support".

### PHP 5.3.x 新增的类

##### `datetime`:

- `DateInterval`

- `DatePeriod`

##### Phar:

- `Phar`

- `PharData`

- `PharException`

- `PharFileInfo`

##### SPL:

- `FilesystemIterator`

- `GlobIterator`

- `MultipleIterator`

- `RecursiveTreeIterator`

- `SplDoublyLinkedList`

- `SplFixedArray`

- `SplHeap`

- `SplMaxHeap`

- `SplMinHeap`

- `SplPriorityQueue`

- `SplQueue`

- `SplStack`

### 新的全局变量

###### PHP 核心:

- `__DIR__`

- `__NAMESPACE__`

- `E_DEPRECATED`

-`E_USER_DEPRECATED`

- `INI_SCANNER_NORMAL`

- `INI_SCANNER_RAW`

- `PHP_MAXPATHLEN`

- `PHP_WINDOWS_NT_DOMAIN_CONTROLLER`

- `PHP_WINDOWS_NT_SERVER`

- `PHP_WINDOWS_NT_WORKSTATION`

- `PHP_WINDOWS_VERSION_BUILD`

- `PHP_WINDOWS_VERSION_MAJOR`

- `PHP_WINDOWS_VERSION_MINOR`

- `PHP_WINDOWS_VERSION_PLATFORM`

- `PHP_WINDOWS_VERSION_PRODUCTTYPE`

- `PHP_WINDOWS_VERSION_SP_MAJOR`

- `PHP_WINDOWS_VERSION_SP_MINOR`

- `PHP_WINDOWS_VERSION_SUITEMASK`

##### cURL:

- `CURLOPT_PROGRESSFUNCTION`

##### GD:

- `IMG_FILTER_PIXELATE`

##### JSON:

- `JSON_ERROR_CTRL_CHAR`

- `JSON_ERROR_DEPTH`

- `JSON_ERROR_NONE`

- `JSON_ERROR_STATE_MISMATCH`

- `JSON_ERROR_SYNTAX`

- `JSON_FORCE_OBJECT`

- `JSON_HEX_TAG`

- `JSON_HEX_AMP`

- `JSON_HEX_APOS`

- `JSON_HEX_QUOT`

##### LDAP:

- `LDAP_OPT_NETWORK_TIMEOUT`

##### libxml:

- `LIBXML_LOADED_VERSION`

##### PCRE:

- `PREG_BAD_UTF8_OFFSET_ERROR`

##### PCNTL:

- `BUS_ADRALN`

- `BUS_ADRERR`

- `BUS_OBJERR`

- `CLD_CONTIUNED`

- `CLD_DUMPED`

- `CLD_EXITED`

- `CLD_KILLED`

- `CLD_STOPPED`

- `CLD_TRAPPED`

- `FPE_FLTDIV`

- `FPE_FLTINV`

- `FPE_FLTOVF`

- `FPE_FLTRES`

- `FPE_FLTSUB`

- `FPE_FLTUND`

- `FPE_INTDIV`

- `FPE_INTOVF`

- `ILL_BADSTK`

- `ILL_COPROC`

- `ILL_ILLADR`

- `ILL_ILLOPC`

- `ILL_ILLOPN`

- `ILL_ILLTRP`

- `ILL_PRVOPC`

- `ILL_PRVREG`

- `POLL_ERR`

- `POLL_HUP`

- `POLL_IN`

- `POLL_MSG`

- `POLL_OUT`

- `POLL_PRI`

- `SEGV_ACCERR`

- `SEGV_MAPERR`

- `SI_ASYNCIO`

- `SI_KERNEL`

- `SI_MESGQ`

- `SI_NOINFO`

- `SI_QUEUE`

- `SI_SIGIO`

- `SI_TIMER`

- `SI_TKILL`

- `SI_USER`

- `SIG_BLOCK`

- `SIG_SETMASK`

- `SIG_UNBLOCK`

- `TRAP_BRKPT`

- `TRAP_TRACE`

### INI 文件处理改变

##### PHP 5.3.0 显著改进了 INI 文件的性能和解析, 并且新增了若干语法功能.

- 标准的 `php.ini` 文件被重新组织和命名. `php.ini-development` 包含在开发环境中推荐使用的设置. `php.ini-production` 包含在生产环境中推荐使用的设置.

- 支持两个特殊章节: [PATH=/opt/httpd/www.example.com/] 和 [HOST=www.example.com]. 这两个章节里的指令不能被用户定义的 INI 文件或者运行时覆盖. 关于这些章节的更多信息, 可以这里找到.

- 移除了 `zend_extension_debug` and `zend_extension_ts`. 使用 `zend_extension` 指令来加载全部 Zend 扩展.

- 移除了 `zend.ze1_compatibility_mode`. 如果该 INI 指令被设置为 On, 启动时将抛出 E_ERROR 级别错误.

- 在 "extension" 指令中可以使用全路径来加载模块.

- "ini变量" 现在几乎在 php.ini 文件的任何地方都可以使用.

- 可以在运行时收紧 `open_basedir` 限制条件.

- 可以在 INI 选项数组中使用字母数字或者变量.

- `get_cfg_var()` 现在可以返回 "数组(array)" INI 选项.

- 添加了一个新指令 `mail.add_x_header`.

- `user_ini.filename` 是新增的

- `user_ini.cache_ttl` 也是新增的.

- `exit_on_timeout` 也是新增的.

- `open_basedir` 现在是 `PHP_INI_ALL` 的.

##### 新增以下指令:

- 新的 `.htaccess-style` 用户 INI 文件机制中的 `user_ini.filename` 和 `user_ini.cache_ttl`.

- 新增 `mbstring.http_output_conv_mimetype`. 该指令指定了 `mb_output_handler()` 激活内容类型的正则表达式.

- 新增 `request_order`. 允许控制哪些外部变量在 `$_REQUEST` 中可用.

##### 以下 ini 指令默认值更新:

- `session.use_only_cookies` 默认被设置为 "1"(打开).

- `oci8.default_prefetch` 变更为从 "10" 到 "100".

### 其他改变

- `SplFileInfo::getpathinfo()` 现在返回 `path name` 信息.

- `SplObjectStorage` 现在支持 `ArrayAccess`. 现在可以在 `SplObjectStorage` 中存储关联信息对象.

- 在 `GD` 扩展中, 通过 `imagefilter()` 函数, 可以提供像素支持.

- `var_dump()` 的输出现在包含对象的私有属性.

- 如果会话启动失败, `session_start()` 现在将返回 FALSE.

- `property_exists()` 可以检查一个属性的存在性, 而不管它的访问控制类型(类似于 `method_exists()`).

- `include_path` 现在可以使用Stream 包装器.

- `array_reduce()` 函数的 `initial` 参数现在可以是任何类型.

- 如果没有明确传递上下文环境, 目录函数 `opendir()`, `scandir()`, 和 `dir()` 将使用默认的流上下文环境.

- `crypt()` 函数支持 `Blowfish` 和 `DES` 算法, 并且 `crypt`() 的特点是非常便捷. PHP 有它自己内部的算法实现, 不管是否找到 `crypt` 或 `crypt_r`.

- 在全部平台上, `getopt()` 开始接受"长选项". 可选值和作为短选项分隔符的 = 被支持.

- `fopen()` 新增了一个模式选项(n), 它传递 `O_NONBLOCK` 常量给底层的 `open()` 系统调用. 注意, Windows 上该模式尚未得到支持.

- `getimagesize()` 现在支持 `icon` 文件 (.ico).

- `mhash` 扩展已经移动至 PECL, 但如果 PHP 使用 `--with-mhash` 选项参数进行编译, Hash 扩展也将提供 mhash 支持. 注意, 不管是否开启 mhash 算法, Hash 扩展都无需 mhash 库可用.
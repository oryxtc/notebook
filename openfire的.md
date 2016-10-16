1.首先引入`php-openfire-restapi`的代码,在`github`上的托管地址:[https://github.com/gidkom/php-openfire-restapi](https://github.com/gidkom/php-openfire-restapi)

使用`composer`方式引入类

    composer require "gidkom/php-openfire-restapi:dev-master"

2.可以在`Gidkom\OpenFireRestApi\OpenFireRestApi`类中配置默认属性
```php
class OpenFireRestApi
{
    public $host		= 'xmpp.xxxxxx.net';
    public $port		= '9090';
    public $plugin	  = '/plugins/restapi/v1';
    public $secret	  = 'xxxxxxxx';
    public $useSSL	  = false;
    protected $params   = array();
    public $client;

    /////////
}
```
3.或者实例化对象后,重新定义属性
```php
$api = new Gidkom\OpenFireRestApi\OpenFireRestApi;

// 设置必须的配置项参数
$api->secret = "MySecret";
$api->host = "jabber.myserver.com";
$api->port = "9090";  // default 9090

// 可选的参数 (没有设置为默认值)
$api->useSSL = false;
$api->plugin = "/plugins/restapi/v1";  // plugin 

// 创建一个新用户
$result = $api->addUser('Username', 'Password', 'Real Name', 'Email', array('Group 1'));

//删除一个用户
$result = $api->deleteUser($username);


//禁用一个用户
$result = $api->lockoutUser($username);


//启用一个用户
$result = $api->unlockUser($username);

/**
 * 更新用户信息
 *
 * The $password, $name, $email, $groups arguments are optional
 * 
 */
$result = $api->updateUser($username, $password, $name, $email, $groups)

// 添加到名册中
$api->addToRoster($username, $jid);

// 从名册中删除
$api->addToRoster($username, $jid);

// 更新名册的用户信息
$api->updateRoster($username, $jid, $nickname, $subscription);

// 获取所有组
$api->getGroup();

// 获取某组信息 
$api->getGroup($name);

// 创建一个组
$api->createGroup($group_name, $description);

// 更新某组的描述
$api->updateGroup($group_name, $description);

// 删除某组
$api->deleteGroup($group_name);

```


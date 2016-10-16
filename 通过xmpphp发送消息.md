1.可在xmpp官方中下载对应语言的类库,地址为:[http://xmpp.org/software/libraries.html---](http://xmpp.org/software/libraries.html)

2.这里通过xmpphp实现消息的推送
```php
include('XMPP.php');
public function sendMessage2($uid,$content){
        $host='xmpp.xxxxxx.net';
        $port=5222;
        $user='xxxxxx';
        $password='xxxxxxx';
        $resource='xmpphp';
        $server='xxxxxxx';
        $conn=new \XMPPHP_XMPP($host, $port, $user, $password, $resource, $server , $printlog = false, $loglevel = \XMPPHP_Log::LEVEL_INFO);
        try {
            $conn->connect();
            $conn->useEncryption(false);    //不使用ssl验证
            $conn->processUntil('session_start');
            $conn->presence();
            $conn->message('JId.iZ115it3c8tZ',$content,'groupchat');
            $conn->disconnect();
            return true;
        } catch (\XMPPHP_Exception $e) {
            die($e->getMessage());
            return false;
        }
    }
```

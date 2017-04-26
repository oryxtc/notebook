---
title: xmpphp
date: 2017/1/14 20:46:25
categories: php
tags: 
- xmpphp
description: 有时候需要服务器主动向各房间或者用户发送消息,本文详解通过xmpphp如何实现
---

### 可在xmpp官方中下载对应语言的类库
>地址为:[http://xmpp.org/software/libraries.html---](http://xmpp.org/software/libraries.html)

### 这里通过php实现消息的推送
```php
include('XMPP.php');
public function sendMessage($uid,$content){
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
            $conn->message('JId.server','content','groupchat');
            $conn->disconnect();
            return true;
        } catch (\XMPPHP_Exception $e) {
            die($e->getMessage());
            return false;
        }
    }
```

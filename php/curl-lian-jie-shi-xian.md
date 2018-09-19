---
title: curl连接实现
date: '2017/1/14 20:46:25'
categories: php
tags: curl
description: curl get与poset连接方法的实现
---

# curl连接实现

## curl get连接方法

```php
//准备数据
$url = 'http://xxxx';
//初始化，创建一个新cURL资源
$ch = curl_init();
//设置URL和相应的选项
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE); //跳过证书检查
curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, FALSE); //从证书中检查SSL加密算法是否存在
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); //返回数据不输出到屏幕
curl_setopt($ch, CURLOPT_HEADER, 0);
//抓取URL并把它传递给浏览器
$result = curl_exec($ch);
//关闭cURL资源，并且释放系统资源
curl_close($ch);
```

## curl post连接方法

```php
//准备数据
$url  = 'http://xxxx';
$data = array(
    'from_name'    => 'admin',
    'from_pwd'     => 'xxxx',
    'service_name' => 'xxxx'
);
$data = http_build_query($data);//调用api时 需要转换成url编码格式
//初始化，创建一个新cURL资源
$ch = curl_init($url);
//设置URL和相应的选项
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $data);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); //返回数据不输出到屏幕
$return = json_edecode(curl_exec($ch));
curl_close($ch);
```


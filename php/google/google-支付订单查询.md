---
title: google-支付订单查询
date: 2017/1/14 20:46:25
categories:
- php
- google
tags:
- php
- google
description: 通过access-token,获取用户基本信息
---

> 官方文档链接:[https://developers.google.cn/android-publisher/](https://developers.google.cn/android-publisher/)

>github链接:[https://github.com/google/google-api-php-client](https://github.com/google/google-api-php-client)

### 查询google的支付订单
```php
public static function curl_google_product(){
    // include your composer dependencies
    require_once 'vendor/autoload.php';
    try{
        putenv('GOOGLE_APPLICATION_CREDENTIALS=/PATH/WEB/xx.json'); //配置系统环境变量
        $client = new \Google_Client(); //实例化链接
        $client->useApplicationDefaultCredentials();
        $client->addScope(\Google_Service_AndroidPublisher::ANDROIDPUBLISHER);//添加使用域
        // OR use environment variables (recommended)
        $service = new \Google_Service_AndroidPublisher($client );
        $packageName='XXX';  //App包名
        $productId='com.XXX'; //商品ID
        $token = 'XXX'; 
        $optps=array();
        $resp = $service->purchases_products->get( $packageName, $productId, $token, $optps );
        $resp = (array)$resp;
        return $resp;
    }catch(Exception $e){
        echo $e->getCode(),'|',$e->getMessage();
        exit;
    }
}
```
##### 如果返回错误为`The current user has insufficient permission...`
```php
Google_Service_Exception: {
    "error": {
        "errors": [{
            "domain": "androidpublisher",
            "reason": "permissionDenied",
            "message": "The current user has insufficient permissions to perform the requested operation."
        }],
        "code": 401,
        "message": "The current user has insufficient permissions to perform the requested operation."
   }
}
```    
需要在谷歌控制台中将xx.json中的`client_email`设置为管理员用户
 
 
### 查询google的订阅订单
```php
private static function curl_google_subscription(){
    // include your composer dependencies
    require_once 'vendor/autoload.php';
    try{
        putenv('GOOGLE_APPLICATION_CREDENTIALS=/PATH/WEB/xx.json'); //配置系统环境变量
        $client = new \Google_Client(); //实例化链接
        $client->useApplicationDefaultCredentials();
        $client->addScope(\Google_Service_AndroidPublisher::ANDROIDPUBLISHER);//添加使用域
        // OR use environment variables (recommended)
        $service = new \Google_Service_AndroidPublisher($client );
        $packageName='XXX';  //App包名
        $subscriptionId='com.XXX'; //订购ID
        $token = 'XXX'; 
        $optps=array();
        $resp = $service->purchases_subscriptions->get( $packageName, $subscriptionId, $token, $optps );
        $resp = (array)$resp;
        return $resp;
    }catch(Exception $e){
        echo $e->getCode(),'|',$e->getMessage();
        exit;
    }
}
```

### 使用公钥进行算法验证(安全性上不高,不推荐)
```php
    public static function curl_post_google($inapp_purchase_data,$inapp_data_signature){
        //$inapp_purchase_data,$inapp_data_signature 为用户付款后,谷歌返回值(或app端返回)
        $google_public_key='公钥';
        $public_key ="-----BEGIN PUBLIC KEY-----\n".chunk_split($google_public_key, 64, "\n")."-----END PUBLIC KEY-----";
        $public_key_handle = openssl_get_publickey($public_key);
        $result = openssl_verify($inapp_purchase_data, base64_decode($inapp_data_signature), $public_key_handle, OPENSSL_ALGO_SHA1);
        if ($result !== 1) {
            return $result;
        }
        return true;
    }
```



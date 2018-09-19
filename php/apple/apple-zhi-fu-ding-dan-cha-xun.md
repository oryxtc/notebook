---
title: apple支付订单查询
date: '2017/1/14 20:46:25'
categories:
  - php
  - apple
tags:
  - php
  - apple
description: '通过curl请求苹果服务器,验证苹果内购订单'
---

# apple-支付订单查询

```php
public function actionInApple(){
    $receipt = Yii::$app->request->post('appleReceipt');     //IOS返回的加密数据
    $transactionId = Yii::$app->request->post('transactionId');      //商品交易号
    $url = 'https://buy.itunes.apple.com/verifyReceipt';     //正式验证地址
    //向apple服务器取得信息
    $verifyJson = ['receipt-data' => $receipt]; //IOS验证数据格式
    $data_string= json_encode($verifyJson);    //将数据转换为json字符串
    $res = self::curl_post_apple($url, $data_string);
    $data = json_decode($res, true);    //解析ios返回的数据
    //如果返回状态值为21007,请求ios的测试验证地址
    //根据产品需求,可以不进行测试地址验证
    if(isset($data['status']) && $data['status']===21007){
        $url = 'https://sandbox.itunes.apple.com/verifyReceipt'; //测试验证地址
        $res = self::curl_post_apple($url, $post_string);
        $data = json_decode($res, true);
    }
    //状态值为0,数据验证通过
    if(isset($data['status']) && $data['status'] === 0){
       //验证商品交易号是否一致
        if($transactionId != $item['transaction_id'] ){
            return false;
        }
    }
 }

 //通过curl请求ios服务器
  private static function curl_post_apple($url, $data_string){
     $curl_handle=curl_init();
     curl_setopt($curl_handle,CURLOPT_URL, $url);
     curl_setopt($curl_handle,CURLOPT_RETURNTRANSFER, true);
     curl_setopt($curl_handle,CURLOPT_HEADER, 0);
     curl_setopt($curl_handle,CURLOPT_POST, true);
     curl_setopt($curl_handle,CURLOPT_POSTFIELDS, $data_string);
     curl_setopt($curl_handle,CURLOPT_SSL_VERIFYHOST, 0);
     curl_setopt($curl_handle,CURLOPT_SSL_VERIFYPEER, 0);
     $response = curl_exec($curl_handle);
     curl_close($curl_handle);
     return $response;
 }
```


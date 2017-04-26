---
title: facebook-检索用户个人信息
date: 2017/1/14 20:46:25
categories:
- php
- facebook
tags: 
- php
- facebook
description:  通过access-token,获取用户基本信息
---

### 检索用户个人信息

>github链接:[https://github.com/facebook/php-graph-sdk](https://github.com/facebook/php-graph-sdk)
>官方文档链接:[https://developers.facebook.com/docs/php](https://developers.facebook.com/docs/php)

```php
$fb = new \Facebook\Facebook([
  'app_id' => '{app-id}',                           //app的id
  'app_secret' => '{app-secret}',                   //app的密令
  'default_graph_version' => 'v2.8',                
  //'default_access_token' => '{access-token}',     //可选配置项
]);

// Use one of the helper classes to get a Facebook\Authentication\AccessToken entity.
//   $helper = $fb->getRedirectLoginHelper();
//   $helper = $fb->getJavaScriptHelper();
//   $helper = $fb->getCanvasHelper();
//   $helper = $fb->getPageTabHelper();

try {
  // Get the \Facebook\GraphNodes\GraphUser object for the current user.
  // If you provided a 'default_access_token', the '{access-token}' is optional.
  
  $response = $fb->get('/me', '{access-token}');
  //$response = $fb->get('/me?fields=id,name,picture,birthday,gender','{access-token}'); 获取该用户的id,姓名,头像,生日,性别
} catch(\Facebook\Exceptions\FacebookResponseException $e) {
  // When Graph returns an error
  echo 'Graph returned an error: ' . $e->getMessage();
  exit;
} catch(\Facebook\Exceptions\FacebookSDKException $e) {
  // When validation fails or other local issues
  echo 'Facebook SDK returned an error: ' . $e->getMessage();
  exit;
}

$me = $response->getGraphUser();
//$me = $response->getGraphUser()->asArray();   直接获取数组

echo 'Logged in as ' . $me->getName();
```


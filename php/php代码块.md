---
title: php代码块
date: 2017/1/14 20:46:25
categories: php
tags: 
- php
description:  货号单 0000001的生成方法&通过goole Api以经纬度获取城市名...
---
### 货号单 0000001的生成方法
```php
function _get_sn($sn) {
    if ($sn) {
        return $sn;
    }
    if (($numT = M('GoodsNumber')->getFieldByDate(date('Ymd'), 'count')) === null) {
        $res = M('GoodsNumber')->add(array('date' => date('Ymd'), 'count' => str_pad(1, 6, 0, STR_PAD_LEFT)));
        $num = str_pad(1, 6, 0, STR_PAD_LEFT);
    } else {
        M('GoodsNumber')->where(array('date' => date('Ymd')))->setInc('count');
        $num = str_pad($numT + 1, 6, 0, STR_PAD_LEFT);
    }
    $sn = 'sn' . date('Ymd') . $num;
    return $sn;
}
```

### 通过goole Api以经纬度获取城市名
```php
public static function curlGet( $latlng )
{
    $url = "http://maps.google.com/maps/api/geocode/json?latlng={$latlng}&sensor=false";
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE); //跳过证书检查
    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, FALSE); //从证书中检查SSL加密算法是否存在
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_HEADER, 0);
    $output = curl_exec($ch);
    curl_close($ch);
    return json_decode($output);
}
```

### 生成uuid方法

 * 使用PHP内置函数实现
    ```php
    uniqid(prefix,more_entropy)
    ```

* 使用自定义方法实现
    ```php
    public function getUuid(){ 
        return sprintf('%04x%04x%04x-%04x%04x-%04x%04x%04x', 
            // 32 bits for "time_low" 
            mt_rand(0, 0xffff), mt_rand(0, 0xffff), 
            // 16 bits for "time_mid" 
            mt_rand(0, 0xffff), 
            // 16 bits for "time_hi_and_version", 
            // four most significant bits holds version number 4 
            mt_rand(0, 0x0fff) | 0x4000, 
            // 16 bits, 8 bits for "clk_seq_hi_res", 
            // 8 bits for "clk_seq_low", 
            // two most significant bits holds zero and one for variant DCE1.1 
            mt_rand(0, 0x3fff) | 0x8000, 
            // 48 bits for "node" 
            mt_rand(0, 0xffff), mt_rand(0, 0xffff), mt_rand(0, 0xffff) 
        ); 
    }
    ```

### 验证身份证函数
```php
puhlic function validateIdCard($value){
    if (!preg_match('/^\d{17}[0-9xX]$/', $value)) { //基本格式校验
        return false;
    }
    $parsed = date_parse(substr($value, 6, 8));
    if (!(isset($parsed['warning_count']) 
        && $parsed['warning_count'] == 0)) { //年月日位校验
        return false;
    }
    $base = substr($value, 0, 17);
    $factor = [7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2];
    $tokens = ['1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2'];
    $checkSum = 0;
    for ($i=0; $i<17; $i++) {
        $checkSum += intval(substr($base, $i, 1)) * $factor[$i];
    }
    $mod = $checkSum % 11;
    $token = $tokens[$mod];
    $lastChar = strtoupper(substr($value, 17, 1));
    return ($lastChar === $token); //最后一位校验位校验
}
```

### 加密解密函数
```php
$str = '111021';
$key = 'APPYJJ-PHONE-LAZY';
//加密函数
function encode($str,$key){
    $res = base64_encode($str);
    $code = $res^$key;
    return $code;
}
$str = encode($str,$key);
print_r($str);
echo "<hr>";
//解密函数
function decode($str,$key)
{
    return base64_decode($str^$key);
}
print_r(decode($str,$key));
```

### 菜单栏数据递归实现
```php
/**
* 格式化菜单数据
* @param $menuData
* @param array $finalMenuData
* @return array
*/
public static function formatMenu(&$menuData, &$finalMenuData = [])
{
    foreach ($menuData as $key => $item) {
        if ($item['parent_id'] === 0) {
            $finalMenuData[] = $item;
            continue;
        }
        self::formatMenuByParentId($item, $finalMenuData);
    }
    return $finalMenuData;
}

/**
* 通过parent_id 格式化菜单数据
* @param $item
* @param array $finalMenuData
*/
public static function formatMenuByParentId($item, &$finalMenuData = [])
{
    foreach ($finalMenuData as &$value) {
        if ($item['parent_id'] === $value['id']) {
            $value['node'][] = $item;
            continue;
        }
        if (!empty($value['node'])) {
            self::formatMenuByParentId($item, $value['node']);
        }
    }
}
```

### 自动捕获Fatal Error
```php
register_shutdown_function( "fatal_handler" );
set_error_handler("error_handler");

define('E_FATAL',  E_ERROR | E_USER_ERROR |  E_CORE_ERROR | E_COMPILE_ERROR | E_RECOVERABLE_ERROR| E_PARSE );

//获取fatal error
function fatal_handler() {
    $error = error_get_last();
    if($error && ($error["type"]===($error["type"] & E_FATAL))) {
        $errno   = $error["type"];
        $errfile = $error["file"];
        $errline = $error["line"];
        $errstr  = $error["message"];
        error_handler($errno,$errstr,$errfile,$errline);
  }
}
//获取所有的error
function error_handler($errno,$errstr,$errfile,$errline){
$str=<<<EOF
     "errno":$errno
     "errstr":$errstr
     "errfile":$errfile
     "errline":$errline
EOF;
//获取到错误可以自己处理，比如记Log、报警等等
echo $str;
}
``` 


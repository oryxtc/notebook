# curl get连接方法
```php
//初始化，创建一个新cURL资源
$ch = curl_init();
//2.设置URL和相应的选项
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE); //跳过证书检查
curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, FALSE); //从证书中检查SSL加密算法是否存在
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); //返回数据不输出到屏幕
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_HEADER, 0);
//3.抓取URL并把它传递给浏览器
$result=curl_exec($ch);
//4.关闭cURL资源，并且释放系统资源
curl_close($ch);
```

#curl post连接方法
```php
$data=array(
    'from_name'=>'admin',
    'from_pwd'=>'a12345',
    'service_name'=>'iZ115it3c8tZ',
    'server_host'=>'xmpp.palmobo.net',
    'to_user'=>'1@iZ115it3c8tZ',
    'mess_content'=>'content',
    'send_port'=>5222,
);
$data=http_build_query($data);//调用api时  需要转换成url编码格式
$url='http://xxxx';
$ch = curl_init ($url);
curl_setopt ( $ch, CURLOPT_URL, $url );
curl_setopt ( $ch, CURLOPT_POST, 1 );
curl_setopt ( $ch, CURLOPT_RETURNTRANSFER, 1 );
curl_setopt ( $ch, CURLOPT_POSTFIELDS, $data );
$return = json_edecode(curl_exec( $ch ));
curl_close ( $ch );
```
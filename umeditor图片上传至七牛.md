**1.确定图片上传实例化类的路径,在`umeditor/php/index.php`**

```php
header("Content-Type:text/html;charset=utf-8");
error_reporting(E_ERROR | E_WARNING);
date_default_timezone_set("Asia/chongqing");
include "uploaderController.php"; //上传配置
$config = array(
    "savePath" => "upload/", //存储文件夹
    "maxSize" => 1000, //允许的文件最大尺寸，单位KB
    "allowFiles" => array(".gif", ".png", ".jpg", ".jpeg", ".bmp") //允许的文件格式
); //实例化图片上传类
$up = new uploaderController("upfile", $config);
$type = $_REQUEST['type']; $callback = $_GET['callback'];
$info = $up->getFileInfo();
/**
* 返回数据
*/
if ($callback) {
    echo '<script>' . $callback . '(' . json_encode($info) . ')</script>';
} else {
    echo json_encode($info);
}
```

**2.修改`uploderController.php` 上传类 `upfile()`**

```php
// $folder = $this->getFolder();
//不上传到本地
// if ( $folder === false ) { // $this->stateInfo = $this->getStateInfo( "DIR_ERROR" );
//     return;
// }
// $this->fullName = $folder . '/' . $this->getName();
// if ( $this->stateInfo == $this->stateMap[ 0 ] ) {
//     if ( !move_uploaded_file( $file[ "tmp_name" ] , $this->fullName ) ) {
//         $this->stateInfo = $this->getStateInfo( "MOVE" );
//     }
// }
//使用七牛上传
$result=$this->updateQiniu($file[ "tmp_name" ]);
$this->fullName='http://xxxxxxx.bkt.clouddn.com/'.$result['hash'];
```

**3.实现`updateQiniu()` 方法**
```php
private function updateQiniu($tmp_name){
    include '../../../../../vendor/qiniu/php-sdk/autoload.php'; //十分重要
    $accessKey = 'xxxxxxxxx';
    $secretKey = 'xxxxxxxxx';
    $auth = new Auth($accessKey, $secretKey);
    $bucket = 'ulala-test';
    $token = $auth->uploadToken($bucket);
    // 要上传文件的本地路径
    $filePath =$tmp_name;
    $uploadMgr =new UploadManager();
    $key=null;
    list($ret, $err) = $uploadMgr->putFile($token,$key, $filePath);
    if ($err !== null) {
        return $err;
    } else {
        //成功的返回数组 hash 和key
        return $ret;
    }
}
```



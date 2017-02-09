###1.空操作处理

在控制器里添加一个_empty()的方法
```php
public function _empty(){
    //echo '您访问的操作不存在 ('.MODULE_NAME.'/'.CONTROLLER_NAME.'/'.ACTION_NAME.')';;
    $this->display('Manager@Public:empty');
}
```
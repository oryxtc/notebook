#通过Fcm实现消息推送

```php
public static function sendMessages() {
    try {
        //准备配置
        $url = 'https://fcm.googleapis.com/fcm/send';
        $api_key = 'AIzaSyBIteo6XFgX_xxxx';
        //准备消息内容
        $data = [
            'message'    => xxx,
            'noticeType' => xxx,
        ];
        $fields = array(
            //群发使用registration_ids[为数组]  单个用户使用to
            //'to'             => xxx
            'registration_ids' => xxx,  
            'data'             => $data,
            'time_to_live'     => xxx,
        );
        $headers = array(
            'Authorization: key=' . $api_key,
            'Content-Type: application/json'
        );
        // Open connection
        $ch = curl_init();
        // Set the url, number of POST vars, POST data
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_POST, true);
        curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_TIMEOUT, 5);    //请求超时时间这里设置为5s
        // Disabling SSL Certificate support temporarly
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
        curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($fields));
        // Execute post
        $result = curl_exec($ch);
        // Close connection
        curl_close($ch);
        return $result;
    } catch (Exception $e) {
        return false;
    }
}

```
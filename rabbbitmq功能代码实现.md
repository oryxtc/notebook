### php + RabbitMQ 功能实现
引入php-amqplib类库,类库地址为[https://github.com/php-amqplib/php-amqplib](https://github.com/php-amqplib/php-amqplib)

**1.简单的示例代码实现**

![](/assets/QQ截图20161212205751.png)

+ 生产者(发送消息者) 
```php
    public function producer(){
        //创建连接实例
        $connection = new AMQPStreamConnection('127.0.0.1', 5672, 'guest', 'guest');
        //创建一个连接通道
        $channel = $connection->channel();
        //声明队列,如果该队列不存在会创建
        $channel->queue_declare('hello', false, false, false, false);
        //创建消息实例
        $msg = new AMQPMessage('Hello World1!');
        //通过通道,推送消息到队列中
        $channel->basic_publish($msg, '', 'hello');
        //关闭通道
        $channel->close();
        //关闭连接
        $connection->close();
        echo " [x] Sent 'Hello World!'\n";
    }
```
+ 消费者(获取消息者)
```php
    public function consumer(){
        //创建连接实例
        $connection = new AMQPStreamConnection('127.0.0.1', 5672, 'guest', 'guest');
        //创建一个连接通道
        $channel = $connection->channel();
        //声明队列,如果该队列不存在会创建
        $channel->queue_declare('hello', false, false, false, false);
        //创建一个实例(这里用于回调)
        $callback_model=new Callback();
        //通过通道消费队列中的信息,并执行回调(这里为array($callback_model,'getQueueInfo'))
        $channel->basic_consume('hello', '', false, false, false, false,array($callback_model,'getQueueInfo'));
        //当存在回调时,这里将进入无限循环,每当队列中被推送新值,就会执行回调
        while(count($channel->callbacks)) {
            $channel->wait();
        }
        //关闭通道
        $channel->close();
        //关闭连接
        $connection->close();
    }
```
+ 回调方法
```php
class Callback extends Model{
    //$channel->basic_consume 执行回调方法时,会传入$msg对象
    public static function getQueueInfo($msg){
        //我这里将$msg中的主体(队列中的消息值) 和 当前进程号 存入表中
        $test_model=new Table();
        $test_model->content=$msg->body;
        $test_model->num=getmypid();
        $test_model->save();
        //注意:$channel->basic_consume 的第四个参数为true时(即 no ack),则为关闭消息确认
        //$channel->basic_consume 的第四个参数为false时,则为开启消息确认
        //开启消息确认机制后,回调方法执行消息确认后,该信息才会被消耗
        $msg->delivery_info['channel']->basic_ack($msg->delivery_info['delivery_tag']);
    }
}
```